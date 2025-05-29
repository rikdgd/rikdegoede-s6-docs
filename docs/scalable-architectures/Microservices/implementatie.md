---
sidebar_position: 2
---
# Implementatie in LockBox
Volgens de non-functional requirements van **LockBox** moet de applicatie rond de `900.000` gelijktijdige gebruikers aankunnen (NF-09). Dit maakt dat de keuze is gemaakt om LockBox te implementeren volgens een microservice architectuur. Hier in dit document is terug te lezen hoe dit is gedaan voor het LockBox project.

## Design
Het ontwikkelen van een microservice architectuur begint bij de design fase. Als eerst zal de applicatie moeten worden opgedeeld in losse services die onafhankelijk van elkaar hun doel zouden moeten kunnen voltooien. Voor het LockBox project is het design uitgewerkt in een **C3 container diagram**, deze is terug te vinden in het [design document](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/Design-Document). 

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

## Kubernetes
bla bla bla