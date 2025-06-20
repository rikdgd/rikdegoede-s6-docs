---
sidebar_position: 3
---
# Change, Risk, Security & Release Management

Er is geen standaard manier om een DevSecOps proces in te richten. Dit komt natuurlijk door het feit dat ieder bedrijf en iedere applicatie anders is. Dit document zal duidelijk proberen te maken hoe het DevSecOps process voor LockBox is ingericht, en wat de voor- en nadelen hiervan zijn. Om dit gestructureerd te doen zal het DevSecOps proces van LockBox worden opgedeeld in change, risk, security en release management.


## Change management
Voor het beheren van veranderingen/updates aan de software wordt gebruik gemaakt van version control, hiervoor is de keuze gemaakt om Git te gebruiken aangezien dit veel wordt gebruikt in de industrie. De Git repositories worden op GitHub gehost om samenwerken makkelijker te maken. 

### Branching strategy
Voor het gebruik van branches is bij het LockBox project afgesproken om *feature branches* te gebruiken. Dit wil zeggen dat voor het ontwikkelen van een nieuwe feature een nieuwe geïsoleerde branch wordt aangemaakt. Op deze branch wordt de feature ontwikkeld, en deze branch wordt pas samengevoegd wanneer de feature implementatie getest is en alle pipelines slagen. 

Om altijd een werkende versie te hebben van de applicatie wordt de `main` branch gebruikt. Deze branch moet daarom extra volledig getest worden aangezien dit de versie van de software is die de gebruikers zullen zien. Over het algemeen zullen er niet veel branches naar `main` worden ge-merged, hiervoor wordt namelijk de `development` branch gebruikt.

De `development` branch heeft een belangrijk doel, het is namelijk de plek waar gemaakt features worden samengevoegd. Het samenvoegen van features is namelijk een actie die vaak in fouten resulteert, en dit zou niet handig zijn wanneer dit gebeurd op de `main` branch. 

### Feature merging
Om later de verschillende ontwikkelde features weer samen te voegen zullen GitHub merge requests gebruikt worden. Aan deze merge request moet altijd een extra reviewer worden gekoppeld zodat de kans groter is dat fouten worden tegengehouden. Ook is het belangrijk dat degene die de merge request aanmaakt, deze niet zelf mag goedkeuren. Verder moeten alle workflows succesvol worden afgerond voordat een merge request goedgekeurd mag worden. 

---
## Risk management
### Risico's
Er zijn verschillende risico's bij het ontwikkelen van software die in iedere fase van een project kunnen ontstaan. Het is slim om over deze risico's na te denken en een plan op te stellen voor het geval ze toch plaatsvinden. Voor het LockBox project zijn ook een aantal risico's uitgewerkt om hier op voorbereid te zijn. Deze zijn hier terug te lezen.

#### File storage database capaciteit limiet
De LockBox applicatie heeft als hoofd functionaliteit het uploaden van bestanden. Wanneer dit niet meer goed werkt kan de applicatie niet goed gebruikt worden. Een manier waarop deze situatie kan ontstaan is wanneer de database met bestandsdata zijn opslag capaciteit limiet bereikt. Om dit risico te voorkomen zal de status van de bestandsdatabase goed gemonitord moeten worden. Ook zullen gebruikers een limiet moeten hebben op hun maximale opslag grote. Een andere mogelijke oplossing is om automatisch oude bestanden te verwijderen wanneer de gebruiker dit toestaat. 

#### File storage gerelateerde bottlenecks 
Zoals eerder benoemd is het opslaan en beheren van bestanden de hoofdfunctionaliteit van LockBox. Dit wil zeggen dat deze features waarschijnlijk het meest gebruikt gaan worden. Hierdoor zou een bottleneck kunnen ontstaan op zowel de services die deze features organiseren, als de database waarin bestandsdata staat opgeslagen. Om dit op tijd op te merken kan monitoring gebruikt worden, maar dit zal het probleem niet oplossen. Wel kunnen de services verantwoordelijk voor het beheren van bestanden automatisch opgeschaald worden om meer load te verdragen met behulp van Kubernetes. Voor het opschalen van de database zou een cloud service gebruikt kunnen worden. 

#### Bestandsdata raakt verloren
Een andere risico rondom bestanden is het verloren raken van de data. Dit zou kunnen gebeuren wanneer een database wordt gedropt, of niet meer bereikbaar is. Om dit te voorkomen kunnen regelmatig automatische back-ups gemaakt worden. Zo is altijd nog een verouderde versie van de data toegankelijk. Dit zal wat dataverlies als gevolg hebben, maar is beter dan alle data kwijtraken. 

#### Onjuiste gebruikers rechten
LockBox maakt gebruik van een **RBAC systeem**. Dit houdt in dat gebruikers accounts rollen hebben, en aan de hand van deze rollen worden de rechten per account bepaald. Het zou kunnen dat er een fout gemaakt wordt waardoor gebruikers te veel/weinig rechten hebben. Dit zou kunnen betekenen dat de gebruiker de applicatie niet goed kan gebruiken, of dat de gebruiker toegang heeft tot functionaliteiten en data die hij/zij niet zou moeten hebben. Ook hierbij kan monitoring goed gebruikt worden om dit soort problemen op te sporen. Verder wordt gebruikgemaakt van **Auth0** voor het hosten van gebruikersaccounts. Dit maakt het makkelijker om gebruikers rollen en accounts te beheren/aanpassen.

### Mitigate
Het LockBox project heeft verschillende risico's waarop de ontwikkelaars voorbereid moeten zijn. Het is echter niet genoeg om enkel te weten wat de risico's zijn, er moet ook een plan zijn om deze risico's op te lossen mochten ze plaatsvinden. Hier is daarom uitgewerkt hoe de hierboven benoemde risico's opgelost kunnen worden.

#### File storage database capaciteit limiet
In het geval dat de bestandsdatabase zijn limiet aan data opslag bereikt zal de opslagcapaciteit moeten worden uitgebreid. Dit zal echter niet lukken binnen enkele seconden op te lossen, daarom is het een slim idee om de gebruikers van de applicatie op de hoogte te stellen van de technische problemen. Zo kan vermeden worden dat gebruiker bestanden proberen op te slaan wanneer de database dit niet meer aankan. LockBox bevat een notificatie systeem, dit is mogelijk de ideale manier om gebruikers op de hoogte te stellen. 

#### File storage gerelateerde bottlenecks
Wanneer er een bottleneck ontstaat op de database met bestandsdata is de beste aanpak om de database horizontaal te schalen. Dit is uiteraard mogelijk met een self-hosted database, maar dit kost veel onderhoud. Een beter manier om dit te doen is door een cloud hosted oplossing te gebruiken die dit voor ons doet. Voor de microservices van de LockBox applicatie zelf kan ook horizontaal schalen als oplossing gebruikt worden. In het geval dat er dan nog steeds een bottleneck plaatsvind, kan simpelweg het maximale aantal services omhoog gezet worden. 

#### Bestandsdata raakt verloren
Bij dit risico was al aangegeven dat het regelmatig maken van backups een oplossing is voor het probleem. Het terugzetten van deze backup kan echter ook wat tijd kosten, en er zal mogelijk wat data verloren raken aangezien de backup nooit 100% up-to-date zal zijn met de applicatie. Hiervoor geldt dus dat wanneer mogelijk de backup het best teruggezet kan worden wanneer de applicatie niet veel wordt gebruikt om zo min mogelijk gebruikers te hinderen. Ook hier is het een slim idee om de gebruikers op de hoogte te stellen van de technische problemen. 

#### Onjuiste gebruikers rechten
In het geval dat (sommige) gebruikers onbedoelde rechten bezitten zullen deze moeten worden ingetrokken. Afhankelijk van welke rechten dit betreft kan het slim zijn om deze gebruikers tijdelijk niet toe te laten op de applicatie om schade te voorkomen. Vervolgens kunnen de rechten worden aangepast, en wanneer deze weer kloppen kunnen de gebruikers opnieuw worden toegelaten. Ook hier geldt weer dat het een goed idee is om de gebruikers die hier last van ondervinden op de hoogte te stellen. 

---

## Security Management
Bij het ontwikkelen van software zijn er veel verschillende risico's. Een van de gebruikte packages kan een vulnerability bevatten, gevoelige data kan perrongeluk in de code terechtkomen, of de ontwikkelaar kan zelf een fout maken in zijn/haar geschreven code. Het voorkomen van deze risico's is daarom een onmogelijke taak om met de hand te doen, hier moet een andere strategie voor gebruikt worden.

Voor het voorkomen van vulnerabilities in software zijn twee stappen nodig, namelijk het detecteren van vulnerabilities en het oplossen ervan. Hoe deze twee stappen worden toegepast in het LockBox project is hier in meer detail te lezen.

### Detecteren van vulnerabilities
Om vulnerabilities automatisch te detecteren wordt voor het LockBox project gebruik gemaakt van SAST (*Static Application Security Testing*). Dit wil zeggen dat de code automatisch wordt gescand op bekende vulnerabilities. De tool die hiervoor wordt gebruikt is **Snyk**, een veel gebruikt security testing tool gehost in de cloud. 

Het opzetten van Snyk security scanning is in het LockBox project gedaan via de CI/CD pipelines, deze zijn geïmplementeerd in GitHub actions. hoe dit werkt kan het best worden uitgelegd aan de hand van de opgezette configuratie.
```yaml
name: dotnet Snyk SAST

on: 
  push:
    branches: ["main", "development"]
  pull_request:
    branches: ["main", "development"]

permissions:
  contents: read
  security-events: write
    
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      
      - name: Setup .NET SDK
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.x'
      
      - name: Restore .NET dependencies
        run: dotnet restore
      
        
      - name: Run Snyk
        uses: snyk/actions/dotnet@master
        continue-on-error: true
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
        with:
          args: --sarif-file-output=snyk.sarif
          
      - name: Upload result to GitHub Code Scanning
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: snyk.sarif
```

In de configuratie zijn meerdere *"steps"* gedefinieerd, dit zijn stappen die in de pipeline worden uitgevoerd. Deze stappen kunnen als volgt worden samengevat:
1. **Setup .NET SDK** - Zorgt ervoor dat de .NET SDK toegankelijk is in de pipeline voor het bouwen van het project.
2. **Restore .NET dependencies** - Installeert alle benodigde dependencies van het project. 
3. **Run Snyk** - Voert security testing uit met behulp van Snyk. Dit is de belangrijkste stap aangezien hier de vulnerabilities gevonden worden mochten deze aanwezig zijn.
4. **Upload result to GitHub Code Scanning** - Deze stap zorgt ervoor dat de resultaten van Snyk als een **SARIF** bestand worden opgeslagen onder de GitHub security tab van de repository. Zo zijn de resultaten direct in de repository zichtbaar. 


### Oplossen van vulnerabilities
Wanneer een vulnerability wordt gevonden zal deze (meestal) ook verholpen moeten worden. Voor het LockBox project is bijvoorbeeld de volgende non-functional requirement opgesteld: *"De applicatie bevat geen vulnerabilities met een CVE score hoger dan 8.0"*. Dit wil zeggen dat vulnerabilities die gevonden worden enkel onmiddellijk opgelost hoeven te worden wanneer ze een CVE score van hoger dan 8 hebben. Vulnerabilities met een lagere score worden geaccepteerd, maar wel goed bijgehouden in de SARIF bestanden zodat deze later verholpen kunnen worden. 

### Secret management
De LockBox applicatie maakt net als de meeste webapplicaties gebruik van secrets. Met een secret wordt hier gerefereerd naar data die in de verkeerde handen veel schade kan aanrichten. Denk hierbij bijvoorbeeld aan de inloggegevens van een database of server. De software van het LockBox project heeft deze secrets voor verschillende redenen nodig, maar ze direct (hardcoded) in de code verwerken maakt het makkelijk deze data te achterhalen via reverse engineering of de git geschiedenis. 

Om te zorgen dat secret niet in de code terechtkomen maakt LockBox gebruik van *environment variables* om deze data in te laden. Zo komen ze nooit in de code terecht, en als kunnen ze worden aangepast zonder de noodzaak om de software opnieuw te compileren. Om dit proces makkelijker te maken wordt voor LockBox gebruik gemaakt van `.env` bestanden. In deze bestanden kunnen de environment variables worden gedefinieerd en automatisch ingeladen door de software. Deze `.env` bestanden worden enkel gebruikt voor development en zijn niet in de software repositories terug te vinden om de secrets geheim te houden. 

---

## Release management
Het uitrollen van een applicatie is vaak en grote opgave, en voor iedere applicatie ziet het proces er anders uit. Een nieuwe release uitrollen is vaak een spannend moment aangezien fouten in de applicatie voor problemen kunnen zorgen. Om dit proces zo gecontroleerd mogelijk te laten  verlopen is hier een *release management* uitgewerkt. Hier staat uitgelegd hoe nieuwe releases van de LockBox software worden uitgerold. 

### Build management
Voor de nieuwe versie van de LockBox software uitgerold kan worden, zal deze eerst gebouwd moeten worden. Om dit proces te automatiseren wordt voor LockBox gebruik gemaakt van GitHub actions workflows. Deze workflows voeren per repository op zijn minst de volgende stappen uit:
1. Compileren van de software.
2. Bouwen van een docker image.
3. Pushen van docker image een image repository. 

*In realiteit voeren de workflows meer stappen uit. De stappen die hierboven staan zijn enkel de stappen die worden gebruikt voor het uitrollen van een nieuwe release.*

De LockBox applicatie wordt uitgerold in een Kubernetes cluster, daarom wordt een docker image gebruikt voor het draaien van de verschillende services. Zo zijn alle dependencies, de software, en het operating system samengevoegd waardoor beter te garanderen is dat de software zal werken. *Een docker image is überhaupt nodig om een deployment in Kubernetes aan te kunnen maken.*

### Deployment omgevingen
Zoals uitgelegd wordt de software van het LockBox project automatisch als docker images uitgerold. Deze images kunnen automatisch opgehaald worden door het Kubernetes cluster om zo de software up-to-date te houden. Dit is echter een versimpelde uitleg van hoe dit proces er uit moet gaat zien voor LockBox.

LockBox zal voor deployment later twee verschillende omgevingen gebruiken: *development* en *release*. Deze hebben beide hun eigen rol binnen het release proces:

#### Development
De Development omgeving is een omgeving met relatief weinig resources tot zijn beschikking. Deze omgeving wordt namelijk gebruikt voor het testen/controleren van de software. Voor deze omgeving worden losse databases en brokers gebruikt om te garanderen dat de gebruikers data veilig blijft. Deze omgeving geeft de ontwikkelaars de kans om de software goed te bekijken, maar ook om hier automated testing op uit te voeren met behulp van DAST.

#### Release
In de release omgeving wordt de LockBox applicatie gedraait voor de eindgebruikers. Deze deployment heeft toegang tot een stuk meer resources om te zorgen dat de applicatie een grote load aankan wanneer nodig. Ook heeft deze deployment toegang tot de productie databases en message broker. 

### Roll-back strategie
Hoe goed het release proces ook is ingedeeld, er is altijd een kans dat fouten gemaakt worden. Mocht er een probleem zijn met een nieuwe release van de software, dan is het belangrijk om te zorgen dat dit probleem zo snel mogelijk verholpen wordt. Hiervoor is een roll-back strategie onmisbaar om te zorgen dat er duidelijk stappen gevolgd kunnen worden om de software terug te zetten naar een vorige versie. 

Aangezien de LockBox applicatie in een Kubernetes cluster wordt uitgerold is het terugzetten van een vorige versie vrij simpel. Kubernetes deployments hebben namelijk van zichzelf support voor rollbacks naar vorige versies. Deze versies worden automatisch bijgehouden door Kubernetes. 

De roll-backs kunnen echter wel deels worden geautomatiseerd. In Kubernetes kunnen namelijk automatisch *"health checks"* worden uitgevoerd om te controleren dat een deployment correct functioneert. Voor LockBox zal dit later ook worden gedaan, alleen dit is enkel bruikbaar voor simpele health checks zoals memory gebruik en zal dus niet alle problemen tegenhouden.

#### Blue-Green strategie
Om het uitrollen van nieuwe versies nog veiliger te maken zal ook gebruik gemaakt worden van de *"Blue-Green"* deployment strategie. Dit wil zeggen dat bij iedere nieuwe versie de volgende stappen worden uitgevoerd:

1. Nieuwe versie van de software (Green) wordt gedeployed naast de vorige versie (Blue). Zowel de oude als nieuwe versie kunnen nu worden gebruikt.
2. Er worden health checks uitgevoerd op de Green deployment.
	1. Bij een succesvolle health check zal de nieuwe deployment (Green) worden gebruikt.
	2. Bij een falende health check zal de oude deployment (Blue) gebruikt blijven worden. 

Door de voorgaande versie van de software altijd online te houden tot de nieuwe versie succesvol blijkt te werken, is het super makkelijk om terug te stappen naar de vorige versie. Enkel de routing hoeft namelijk te worden aangepast. 