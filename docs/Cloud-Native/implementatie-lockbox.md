---
sidebar_position: 2
---
# Implementatie in LockBox
Voor het LockBox project is ook de keuze gemaakt om een aantal cloud services te gebruiken. Voor ieder van deze services is zo zijn reden, en die zal hier terug te vinden zijn. Ook zal hier voor ieder van de gebruikte cloud service worden uitgelegd waarvoor ze worden gebruikt, en kort hoe dit is geïmplementeerd. 

Tot slot zal ook nog een kostenberekening worden gemaakt per cloud service. Deze kostenberekening zal er steeds vanuit gaan dat de applicatie precies zoveel load zal ervaren als deze moet aankunnen volgens de opgestelde [non-functional requirements](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/analyse-document#non-functional-requirements). Deze kostenberekeningen zullen daarom wel met een *korreltje zout* genomen moeten worden.

## MongoDB Atlas
MongoDB Atlas is een volledig beheerde cloud-database service voor MongoDB, een populaire NoSQL-database. Het stelt ontwikkelaars in staat om snel en eenvoudig schaalbare, flexibele databases te hosten in de cloud, zonder zich zorgen te hoeven maken over onderhoud, updates of beveiliging. Atlas draait op cloudplatforms zoals AWS, Google Cloud en Microsoft Azure, en biedt functies zoals automatische backups, realtime monitoring en ingebouwde beveiliging. Het is ideaal voor moderne applicaties die hoge prestaties en flexibiliteit nodig hebben.

### Waarom in LockBox?
De keuze is gemaakt om NoSQL te gebruiken voor de *"notification service"* van het LockBox project. Dit omdat zolang de applicatie blijft groeien, nieuwe soorten notificaties zullen ontstaan. Dit vereist een flexibele database, en NoSQL is hiervoor ideaal.<br/>(Uitgebreide uitleg: [*Database Keuze*](https://rikdgd.github.io/rikdegoede-s6-docs/docs/distributed-data/database-keuze))

Verder is de volgende non-functional requirement gesteld voor LockBox: *"Het systeem moet minimaal `900,000` gebruikers tegelijkertijd aankunnen"*.
Dit is een groot aantal gebruikers, en hiervoor zal waarschijnlijk horizontale schaling nodig zijn. Dit is echter tijd rovend om zelf op te zetten, aangezien het erg lastig is om meerdere database instanties "in sync" te houden met elkaar. Dit maakt een cloud service die dit voor ons doet zoals MongoDB ideaal. 

Verder is de keuze om MongoDB te gebruiken gemaakt omdat dit de meest gebruikte NoSQL cloud-database is, en omdat hier al ervaring mee is bij de ontwikkelaar van LockBox.
#### Waarvoor?
Notificaties zullen niet altijd direct naar de gebruiker verstuurd kunnen worden, bijvoorbeeld omdat de gebruiker niet online is. Ook zou er iets mis kunnen gaan met de verbinding, en dit kan mogelijk ervoor zorgen dat de gebruiker een notificatie mist. Daarom zullen de notificaties ook in een MongoDB Atlas cloud-database worden opgeslagen.

### Kosten berekening
Als eerst is er een MongoDB Atlas account aangemaakt. Om een database te hosten moet echter ook een "Cluster" worden opgezet. Het cluster is waarvoor de gebruiker betaald, er zijn hiervoor meerdere opties waaruit de gebruiker kan kiezen:

| Tier     | Storage | Ram    | vCPU    | Prijs                                         |
| -------- | ------- | ------ | ------- | --------------------------------------------- |
| **M0**   | 512 MB  | Shared | Shared  | Gratis                                        |
| **Flex** | 5 GB    | Shared | Shared  | Vanaf $0.011 per uur, maximaal $30 per maand. |
| **M10**  | 10 GB   | 2 GB   | 2 vCPUs | $0.09 per uur                                 |
| **M30**  | 40 GB   | 8 GB   | 2 vCPUs | $0.59 per uur                                 |

Voor LockBox wordt momenteel een **M0** cluster gebruikt aangezien de applicatie nog in ontwikkeling is. Later zal er echter een hogere "tier" nodig zijn om het grote aantal gebruikers aan te kunnen. 

Sterker nog, deze tiers zullen geen van alle hoog genoeg zijn voor de LockBox applicatie. Volgens de non-functional requirements van LockBox moet de applicatie minimaal **900,000** gelijktijdige gebruikers aankunnen. Zelf al zou dit het **totaal** aantal gebruikers zijn, dan nog is de storage van een tier **M30** database niet genoeg in de meeste gevallen. 

Stel iedere gebruiker gebruikt maar 10 kilobyte aan storage hiervan, dan nog zou dit resulteren in een totale grote van `900.000 * 10.000` bytes, ofterwijl: `9 GB`. De gebruikers zullen echter veel meer storage gebruiken. Deze database zal namelijk gebruikt worden voor het opslaan van *"notificaties"*, *"file scan reports"*, en *"file vaults"*. Dit is terug te vinden in het [C2 diagram](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/Design-Document#c2---containerdiagram) van de applicatie in design document.

De enige optie die daarom overblijft is om een *"Enterprise Advanced"* contract aan te gaan. Hiervan is de prijs nu niet te bepalen aangezien dit in overleg wordt gedaan, en LockBox nog geen echte applicatie/bedrijf is. Stel de prijs zou per uur exact hetzelfde zijn als die van de **M30** tier database, dan zou het voor LockBox `$0.59 * 24 * 30 = $424.80` per maand kosten. Waarschijnlijk is het veilig om in te schatten dat de echte prijs voor LockBox minimaal het dubbele zal bedragen bij het *"Enterprise Advanced"* contract. 

---
## CloudAMQP
LockBox maakt gebruik van een RabbitMQ message broker. Deze broker is in de cloud gedeployed om onderhoudskosten en tijd te besparen. De broker wordt gehost op [CloudAMQP](https://www.cloudamqp.com/), een cloud service van de makers van RabbitMQ. 

### Waarom in LockBox?
De LockBox applicatie moet horizontaal kunnen schalen vanwege de volgende non-functional requirement: *"Het systeem moet horizontaal schalen om meer/minder resources te gebruiken wanneer nodig"*. Het horizontaal schalen van een message broker is jammer genoeg geen makkelijke taak. 

- *Welke message broker moet een service zijn berichten uit ophalen?*
- *Welke broker moet een service zijn berichten naar posten?*

Deze problemen zijn erg complex om op te lossen. Dit wil niet zeggen dat het onmogelijk is, maar in veel gevallen is het voordeliger om een cloud service te gebruiken die horizontale schalen al heeft geïmplementeerd. Daarom is de keuze gemaakt om een cloud broker te gebruiken voor het LockBox project. 

De keuze om specifiek voor een RabbitMQ broker gehost bij CloudAMQP te gaan, is omdat met RabbitMQ al ervaring is bij de ontwikkelaars. CloudAMQP is gekozen aangezien dit een dienst van hetzelfde bedrijf dat RabbitMQ heeft ontwikkeld. Zij zullen daarom goed weten hoe de broker geconfigureerd moet worden. 

### Kosten berekening
Momenteel is de LockBox applicatie nog in ontwikkeling, en daarom wordt een gratis *"Little Lemur"* broker gebruikt. Het spreekt voor zich dat deze broker niet genoeg capaciteit zal hebben wanneer LockBox in productie gaat. Om alvast een idee te krijgen van de kosten van deze service in de toekomst, is hier een kostenberekening terug te vinden. Hierbij wordt er vanuit gegaan dat de LockBox applicatie precies aan zijn [non-functional requirements](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/analyse-document#non-functional-requirements) zal voldoen. 

CloudAMQP komst met een aantal standaard subscription tiers, dit zijn er te veel om hier uit te werken, maar ze zijn [hier](https://www.cloudamqp.com/plans.html#rmq) terug te vinden. De verschillende tiers hebben allemaal de volgende twee eigenschappen:
- Maximaal aantal **messages** per seconde
- Maximaal aantal gelijktijdige **connecties**.
Om het maximale aantal connecties te verhogen kunnen ook meerdere nodes aangeschaft worden. Hiervoor geldt de prijs: `tier_prijs * aantal_nodes`.

Voor LockBox is de volgende non-functional requirement opgezet: *"Het systeem moet minimaal `900,000` gebruikers tegelijkertijd aankunnen"*. Als deze allemaal iedere 10 seconden een actie uitvoeren waarvoor een message verstuurd moet worden, dan houdt dit in dat er **90.000** message per seconde verstuurd gaan worden. Ook betekend dit dat het aantal gelijktijdige connecties simpelweg **900.000** zal bedragen. 

Om aan het aantal messages per seconde te voldoen is een *"Heavy Hippo"* subscription genoeg. Het jammere is echter dat geen enkele tier voldoet aan het aantal connecties dat LockBox nodig zal hebben. Ook het gebruik van het maximale aantal nodes is hierbij niet goed genoeg.

#### Conclusie: 
Voor LockBox zal minimaal een *"Heavy Hippo"* subscription nodig zijn met een kosten van **$9.995** per maand. Om aan het aantal connecties te voldoen zal de prijs hoger worden maar dit is moeilijk te bepalen hoeveel aangezien de standaard tiers hier niet aan voldoen. Mogelijk is het daarom een goed idee om in de toekomst een andere service te zoeken.