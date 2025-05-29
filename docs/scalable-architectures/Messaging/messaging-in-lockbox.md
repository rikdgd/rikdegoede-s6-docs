---
sidebar_position: 3
---
# Messaging in LockBox

De LockBox applicatie zal gebruik maken van een message broker voor de communicatie tussen verschillende micro services. Hier zijn twee goede redenen voor:
1. Services zijn onafhankelijk van elkaar, ze hoeven namelijk onder anderen niet te weten waar een andere service zich bevind op het netwerk, of op welke poort deze luistert. De services hoeven kort gezegd niets meer van elkaar te weten.d
2. Grote taken kunnen asynchroon worden uitgevoerd. Een bericht om een langdurige taak te starten kan simpelweg in een message queue worden gegooid, en de benodigde service zal deze oppakken wanneer er tijd voor is, zelfs als dit pas over 3 dagen is.

Voor een microservice-architectuur is het dus gewenst om een message broker te gebruiken voor een groot deel van de communicatie tussen de verschillende services. LockBox maakt daarom ook gebruik van messaging. 

---

## Implementatie - file upload
In de LockBox applicatie is als eerst gebruik gemaakt van messaging voor het genereren van (gebruikers) notificaties. Zo zal de gebruiker bijvoorbeeld een notificatie ontvangen wanneer een bestand succesvol is opgeslagen. 

Dit is een goed voorbeeld om messaging op te implementeren. Het uploaden van een of meerdere bestanden kan namelijk best lang duren. Het zou niet efficient zijn om bij na het uploaden van een bestand, eerst te wachten tot de gebruiker zijn notificatie heeft ontvangen. Dit zou namelijk betekenen dat het bestanden uploaden langer duurt dan nodig, en dat de "FileStorageService" onnodig moet wachten op een andere service. 

Met deze reden zal de FileStorageService simpelweg een message plaatsen met een event dat aangeeft dat een bestand is opgeslagen, deze kan de "NotificationService" dan verwerken op zijn eigen tempo. Ook maakt dit dat de services niets van elkaar hoeven te weten, wat ze ontkoppelt van elkaar. Dit is ideaal voor een microservice architectuur.

### Message structuur:
De berichten die de FileStorageService verstuurd hebben een standaard indeling om het makkelijker te maken deze te interpreteren in andere services. De implementatie van deze structuur is zichtbaar in het stukje code hier onder.

```rust
#[derive(Debug, Clone, Serialize, Deserialize)]
pub struct FileMessageData {
    pub event_type: String,
    pub timestamp: String,
    source: String,
    pub user_id: String,
    pub file: Option<FileData>,  // Option: kan "null" zijn
}

#[derive(Debug, Clone, Serialize, Deserialize)]
pub struct FileData {
    id: String,
    name: String,
    timestamp: String,
}
```


Er worden verschillende waarden meegestuurd, hier zijn ze in het kort uitgelegd:
- `event_type` - Deze waarde wordt gebruikt om aan te geven wat voor event heeft plaatsgevonden. In het geval van het uploaden van een bestand, zal de waarde hiervan **"FILE_UPLOADED"** zijn.
- `timestamp` - De UTC gebaseerde date-time, zo kan onthouden worden wanneer het bericht is geplaatst.
- `source` - Een string die aangeeft welke service de message verstuurd heeft. In het geval van de FileStorageService is de waarde hiervan **"FileStorageService"**.
- `user_id` - De id van de gebruiker die het event heeft veroorzaakt, als er een is. 
- `file` - Bevat potentieel extra bestandsdata, kan ook leeg blijven aangezien niet alle events met (specifieke) bestanden te maken zullen hebben.
	- `id` - Het id van het bestand in kwestie.
	- `name` - De bestandsnaam, bijvoorbeeld **"afbeelding.jpg"**.
	- `timestamp` - De date-time wanneer het bestand was geüpload.


### Broker configuratie
Voor deze eerste opzet is gebruik gemaakt van een "RabbitMQ" message broker. Deze is lokaal gehost via een docker container door het volgende command uit te voeren:

```bash
docker run -it --name rabbitmq-testing -p 5672:5672 -p 15672:15672 rabbitmq:4-management
```

Dit start een RabbitMQ container met twee poorten geopend:
- `5672` - Wordt gebruikt voor het ontvangen en versturen van berichten naar de broker. Deze poort zullen services aanroepen message queues te gebruiken.
- `15672` - Deze poort wordt gebruikt om een management GUI te hosten. Deze GUI kan worden gebruikt om statistieken te bekijken of de broker te beheren.

Op deze RabbitMQ broker is vervolgens een message queue aangemaakt genaamd *"file-queue"*. Deze wordt gebruikt om messages op te publiceren die te maken hebben met geüploade bestanden. 

De *"file-queue"* is nog niet gekoppeld aan een **"exchange"**. In RabbitMQ (en veel andere brokers) worden exchanges gebruikt om messages door te routeren naar de juist queue(s) of andere exchanges. Deze extra laag maakt het mogelijk om complexe patronen te implementeren. Voor deze eerste feature is hier nog geen gebruik van gemaakt aangezien het nog om een enkele message gaat.