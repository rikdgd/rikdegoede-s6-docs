---
sidebar_position: 1
---
# Database Keuze
In de LockBox applicatie worden verschillende soorten databases gebruikt, afhankelijk van het type gegevens en de vereisten van de functionaliteit. Dit document beschrijft de keuze voor zowel relationele (**SQL**) als niet-relationele (**NoSQL**) databases binnen LockBox, en licht toe waarom specifiek gekozen is voor **MySQL** en **MongoDB**. Door per use-case de juiste databaseoplossing te gebruiken, blijft de applicatie schaalbaar, onderhoudbaar en flexibel.

## MySQL (SQL)
Voor een groot deel van de LockBox applicatie worden SQL databases gebruikt. Een **SQL-database** is een type **relationele database** waarin gegevens worden opgeslagen in tabellen en beheerd met behulp van **SQL** (*Structured Query Language*). SQL is een standaardtaal waarmee gebruikers data kunnen opvragen, toevoegen, bewerken en verwijderen. SQL-databases zijn ideaal voor gestructureerde gegevens en garanderen consistentie en integriteit via relaties tussen tabellen.

### Waarom in LockBox?
Een goed voorbeeld is de database die LockBox gebruikt voor het opslaan van de metadata van bestanden. Deze zal namelijk altijd hetzelfde formaat aanhouden, en de bestanden moeten gekoppeld worden aan een zogeheten *"Vault"*. Het is niet mogelijk voor een bestand om **geen** vault te hebben, dus moet deze relatie gegarandeerd kunnen worden. Dit maakt SQL een goede keuze voor deze data.

Verder is voor LockBox gekozen om MySQL te gebruiken aangezien hier al ervaring mee was bij de ontwikkelaars. Het zou later echter een goed plan zijn om naar PostgreSQL over te stappen aangezien deze database uitgebreid support heeft voor plugins. Deze kunnen PostgreSQL verbeteren in de omgang met microservices, handig aangezien een van de non-functional requirements van LockBox hetvolgende is: *"Het systeem moet horizontaal schalen om meer/minder resources te gebruiken wanneer nodig"*.

## MongoDB (NoSQL)
Sommige onderdelen van de LockBox applicatie zullen in de toekomst geregeld moeten veranderen. Een goed voorbeeld hiervan zijn de notificaties vereist volgens de volgende user story: *"Als gebruiker wil ik meldingen ontvangen van _belangrijke gebeurtenissen"*. Deze notificaties moeten werken voor verschillende functionaliteiten die allemaal kunnen veranderen. Dit maakt dat de notificaties hun structuur hoogstwaarwschijnlijk later zal moeten veranderen. Hiervoor is een **NoSQL** database ideaal.

### Waarom in LockBox?
Een NoSQL-database is een type databasesysteem dat is ontworpen voor het opslaan en beheren van grote hoeveelheden ongestructureerde of semi-gestructureerde data, zonder het gebruik van traditionele tabellen zoals bij SQL-databases. In plaats daarvan gebruikt NoSQL flexibele datamodellen zoals *documenten*, *sleutel-waardeparen*, *grafen* of *kolom-gebaseerde structuren*. Dit maakt NoSQL ideaal voor toepassingen die snel moeten schalen, zoals sociale media, real-time analyses of big data. Ook maakt dit het mogelijk om de structuur van data aan te passen, zonder dat alle voorgaande entries aangepast hoeven te worden. 

Momenteel ziet de structuur van een notificatie in LockBox er als volgt uit:
```json
{
    Title: "Voorbeeld norificatie",
    Description: "Dit is een voorbeeld beschrijving.",
    UserId: "mqh9y8wqGh29ughwihgJuw"    // Voorbeeld id
}
```
Mogelijk kan de structuur hiervan later veranderen. Er zullen namelijk mogelijk notificaties komen voor specifieke functionaliteiten, of sommige notificaties zullen extra data bevatten. In het geval van bijvoorbeeld een geüpload bestand zal er mogelijk de status van een "file scan" bij komen te staan. Deze extra data heeft echter niet iedere notificatie nodig. Dit maakt NoSQL ideaal.

NoSQL wordt dus in LockBox gebruikt voor de notificaties die gebruikers ontvangen. Het wordt echter ook gebruikt voor de resultaten van *"file scans"* aangezien deze ook niet altijd dezelfde output zullen hebben. en dus een flexibele structuur verwacht. 

Voor LockBox is de keuze gemaakt om specifiek **MongoDB** te gebruiken. Deze keuze is gemaakt aangezien deze NoSQL database veel wordt gebruikt en goede documentatie en support heeft. 