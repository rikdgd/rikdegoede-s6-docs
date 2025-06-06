---
sidebar_position: 2
---
# Implementatie in LockBox
Voor het LockBox project is ook de keuze gemaakt om een aantal cloud services te gebruiken. Voor ieder van deze services is zo zijn reden, en die zal hier terug te vinden zijn. Ook zal hier voor ieder van de gebruikte cloud service worden uitgelegd waarvoor ze worden gebruikt, en kort hoe dit is geïmplementeerd. 

## MongoDB Atlas
MongoDB Atlas is een volledig beheerde cloud-database service voor MongoDB, een populaire NoSQL-database. Het stelt ontwikkelaars in staat om snel en eenvoudig schaalbare, flexibele databases te hosten in de cloud, zonder zich zorgen te hoeven maken over onderhoud, updates of beveiliging. Atlas draait op cloudplatforms zoals AWS, Google Cloud en Microsoft Azure, en biedt functies zoals automatische backups, realtime monitoring en ingebouwde beveiliging. Het is ideaal voor moderne applicaties die hoge prestaties en flexibiliteit nodig hebben.

### Waarom in LockBox?
De keuze is gemaakt om NoSQL te gebruiken voor de *"notification service"* van het LockBox project. Dit omdat zolang de applicatie blijft groeien, nieuwe soorten notificaties zullen ontstaan. Dit vereist een flexibele database, en NoSQL is hiervoor ideaal.

Verder is de volgende non-functional requirement gesteld voor LockBox: *"Het systeem moet minimaal `900,000` gebruikers tegelijkertijd aankunnen."*.
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

*Kosten berekening hier...*