---
sidebar_position: 3
---
# Messaging in LockBox

De LockBox applicatie zal gebruik maken van een message broker voor de communicatie tussen verschillende micro services. Hier zijn twee goede redenen voor:
1. Services zijn onafhankelijk van elkaar, ze hoeven namelijk onder anderen niet te weten waar een andere service zich bevind op het netwerk, of op welke poort deze luistert. De services hoeven kort gezegd niets meer van elkaar te weten.
2. Grote taken kunnen asynchroon worden uitgevoerd. Een bericht om een langdurige taak te starten kan simpelweg in een message queue worden gegooid, en de benodigde service zal deze oppakken wanneer er tijd voor is, zelfs als dit pas over 3 dagen is.

Voor een microservice-architectuur is het dus gewenst om een message broker te gebruiken voor een groot deel van de communicatie tussen de verschillende services. LockBox maakt daarom ook gebruik van messaging. 

---

## Implementatie
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
