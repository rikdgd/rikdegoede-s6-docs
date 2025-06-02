---
sidebar_position: 2
---
# Implementatie in LockBox
Volgens de non-functional requirements van **LockBox** moet de applicatie rond de `900.000` gelijktijdige gebruikers aankunnen (NF-09). Dit maakt dat de keuze is gemaakt om LockBox te implementeren volgens een microservice architectuur. Hier in dit document is terug te lezen hoe dit is gedaan voor het LockBox project.

## Design
Het ontwikkelen van een microservice architectuur begint bij de design fase. Als eerst zal de applicatie moeten worden opgedeeld in losse services die onafhankelijk van elkaar hun doel zouden moeten kunnen voltooien. Voor het LockBox project is het design uitgewerkt in een **C2 container diagram**, deze is terug te vinden in het [design document](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/Design-Document). 

## Containerization
Vervolgens is het belangrijk dat iedere service volledig afgesloten is van de buitenwereld. Hiermee wordt bedoeld dat de applicatie zelf alle dependencies moet bevatten om te kunnen draaien, maar ook hoort de service niets af te weten van andere services. Als dit wel het geval is, zouden services alsnog niet vervangbaar en onafhankelijk zijn. 

Een goede manier om dit voor elkaar te krijgen is door een applicatie te *"containerizen"*. Dit houdt in dat de applicatie, samen met al zijn dependencies en een operating system wordt gebundeld. Dit zorgt ervoor dat wanneer de applicatie op een machine draait, hij waarschijnlijk overal draait. <br/>
*Fun fact: containerizen is nog steeds niet 100% foutbestendig aangezien de kernel van een machine niet wordt meegenomen. Dit maakt VM's in sommige gevallen beter.*

Voor het onctainerizen van applicaties in het LockBox project is gekozen om **Docker** te gebruiken. Dit omdat deze technologie veel wordt gebruikt en hier daarom veel support voor te vinden is. 

Een voorbeeld dockerfile van het LockBox project is hier onder zichtbaar, dit is de dockerfile die wordt gebruikt voor de service verantwoordelijk voor het opslaan van bestanden.

```dockerfile
FROM rust:latest AS Build  
  
WORKDIR /app  
  
COPY . .  
  
RUN cargo test
RUN cargo build --release  
  
  
FROM debian:trixie-slim AS Release  
  
WORKDIR /app  
  
COPY --from=Build /app/target/release/lockbox-fs-service .  
EXPOSE 8080  
  
ENV ROCKET_ADDRESS=0.0.0.0
ENV ROCKET_PORT=8080
ENV DATABASE_URL="mysql://example:example@file-db:3306/file-db"
  
CMD ["./lockbox-fs-service"]
```

## Orchestration met Kubernetes
Container orchestrators zoals Kubernetes spelen een cruciale rol in het moderne landschap van softwareontwikkeling en -implementatie. Ze bieden een platform voor het automatiseren van de inzet, schaling en het beheer van containerized applicaties. Kubernetes, een van de meest populaire container orchestrators, is een open-source systeem ontwikkeld door Google en nu beheerd door de Cloud Native Computing Foundation. Het helpt bij het efficiënt beheren van clusters van containers, waardoor applicaties betrouwbaar en schaalbaar kunnen draaien in verschillende omgevingen, van ontwikkeling tot productie. 

Orchestration met Kubernetes is ideaal voor het LockBox project om te voldoen aan [non-functional requirement 9](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/analyse-document#non-functional-requirements). Met behulp van Kubernetes kan de applicatie automatisch hortizontaal schalen om de benodigde hoeveelheid gebruikers aan te kunnen. Dit geldt echter ook wanneer het aantal active gebruikers daalt, dan zal de applicatie omlaag schalen om minder resources te gebruiken. Dit kan enorm helpen met het besparen van kosten, vooral bij het gebruik van cloud services.

## Deployment
Om de LockBox applicatie naar Kubernetes te deployen zijn verschillende stappen doorlopen. Hier zal worden samengevat hoe dit is gedaan. 

### Deploy Docker images
De eerste stap voor het deployen naar Kubernetes is zorgen dat de Docker images op een (publieke) image repository zoals **Docker hub** staan. In het LockBox project gebeurd dit automatisch via de automated pipelines op GitHub. Voor meer informatie zie: [DevSecOps > CI/CD Pipeline > Deployment](https://rikdgd.github.io/rikdegoede-s6-docs/docs/DevSecOps/ci-cd-pipeline#deployment). 

### Kubernetes opzet
Om de verschillende microservices in Kubernetes op te zetten worden meestal configuratie bestanden gebruikt. Met behulp van deze configuratie bestanden kan de LockBox applicatie in Kubernetes opgezet en geconfigureerd worden. Hier zullen de verschillende onderdelen die zijn geconfigureerd worden behandeld.

#### Pods
Een pod in Kubernetes is het kleinste onderdeel van een deployment. Een pod is een instance van een container met wat extra logica voor monitoring en management. Om de verschillende service van LockBox in een Kubernetes cluster te draaien worden Pods gebruikt. Toch worden de services niet direct als pods opgezet, maar als een *"deployment"*. 

#### Deployments
Een deployment is een beheerde groep aan pods. Via een deployment kan worden aangegeven hoeveel instances van een pod moeten worden opgezet, hoeveel resources de pods mogen gebruiken, maar ook health checks kunnen worden uitgevoerd. 

Een voorbeeld van een deployment configuratie is hier te zien. De `YAML` configuratie hier onder wordt gebruikt voor het opzetten van een deployment voor de *"lockbox-notification-service"*:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: lockbox-notification-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: lockbox-notification-service
  template:
    metadata:
      labels:
        app: lockbox-notification-service
    spec:
      containers:
      - name: lockbox-notification-service
        image: rikdegoede/lockbox-notification-service:latest 
        ports:
          - containerPort: 8080
        env:
          - name: MONGODB_CONN_STRING
            valueFrom:
              secretKeyRef:
                name: lockbox-secrets
                key: mongodb-conn-string
          
          - name: RABBITMQ_CONN_STRING
            valueFrom:
              secretKeyRef:
                name: lockbox-secrets
                key: rabbitmq-conn-string
```


De deployment configuratie zorgt ervoor dat:
- Er 1 *replica* wordt aangemaakt, dit houdt in dat er maar 1 containers van de micro-service tegelijkertijd wordt gedraaid. Later zal horizontale schaling ervoor zorgen dat dit aantal afhankelijk is van de load op de applicatie.
- Port `8080` open wordt gezet op de pods om verbindingen van buitenaf toe te staan.
- De benodigde environment variables worden ingesteld met gebruik van secrets.
	- De MongoDB cloud database connection string.
	- De RabbitMQ cloud broker connection string.

<br/>
#### Services
Een deployment op zichzelf is niet genoeg om de applicatie te gebruiken. Hiervoor kan een **service** worden opgezet. Een service in Kubernetes zorgt ervoor dat een deployment als een network service benaderd kan worden. Dit geldt zowel voor het benaderen vanuit binnen het cluster, als van buitenaf. Voor de *"lockbox-notification-service"* is ook een service opgezet. De configuratie ziet er als volgt uit:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: lockbox-notification-service
spec:
  type: LoadBalancer
  selector:
    app: lockbox-notification-service
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
```

Een paar dingen aan deze configuratie zijn belangrijk om op te merken:
- De service wordt opgezet volgens het type *"LoadBalancer"*. Een LoadBalancer is in staat om automatisch de computational load te verdelen over de verschillende draaiende pods. 
- Port `8080` wordt door gerouteerd naar de onderliggende pods.

<br/>
#### Horizontal Pod Autoscaler (HPA)
Momenteel is er een applicatie opgezet in een Kubernetes cluster, en is deze bereikbaar gemaakt via een bijbehorende **service**. Een van de belangrijkste eisen voor LockBox is echter dat de applicatie horizontaal kan schalen en dit is zo nog niet geïmplementeerd. Hiervoor kan een *"Horizontal Pod Autoscaler"* (HPA) gebruikt worden. 

Een HPA zorgt ervoor dat er automatisch pods worden aangemaakt wanneer de load groeit, en dat deze ook weer worden afgebroken wanneer de load verkleind en ze niet meer nodig zijn. Voor het LockBox project is de volgende configuratie gebruikt om een HPA op te zetten:
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: lockbox-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: lockbox-notification-service
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

Deze configuratie zet een HPA op met de volgende configuratie:
- De target is ingesteld als de "lockbox-notification-service" deployment om aan te geven dat dit horizontaal geschaald moet worden.
- Het minimale aantal "replicas" is op 1 gezet zodat er altijd op zijn minst een pod toegankelijk is.
- Het maximale aantal replicas is op 10 gezet om te voorkomen dat de applicatie te hoog gaat schalen en te veel kosten meebrengt. Denk bijvoorbeeld aan een DDOS aanval.
- Er is een metric aangegeven om de deployment te schalen: wanneer het gemiddelde CPU gebruik boven de 50% komt wordt een extra pod aangemaakt.

Om toegang te krijgen tot de metrics van het cluster is ook een metric server nodig. Gelukkig is deze niet moeilijk op te zetten en kan hier een standaard deployment van Kubernetes voor gebruikt worden. Om deze op te zetten kan het volgende shell command gebruikt worden: 
```sh
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

## Samenvatting
Door de benoemde configuratie toe te passen in een Kubernetes cluster is een schaalbare versie van een enkele microservice opgezet. Wanneer alle microservices van het LockBox project op deze manier worden opgezet zou de applicatie automatisch moeten kunnen schalen. Wel moet dit uiteraard nog getest worden, hoe dit gedaan wordt is terug te lezen in het volgende hoofdtsuk: [Load Testing](https://rikdgd.github.io/rikdegoede-s6-docs/docs/scalable-architectures/Microservices/load-testing).