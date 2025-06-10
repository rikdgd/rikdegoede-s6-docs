---
sidebar_position: 3
---
# 2. Eisen encryptie in frontend.

Deelvraag 2 van dit onderzoek luidt: *"Welke functionele en niet-functionele eisen zijn belangrijk voor veilige en efficiënte encryptie in de frontend?"*

LockBox is een file storage webapplicatie vergelijkbaar met Google Drive. Hierbij is het belangrijk om bestanden te versleutelen om te voorkomen dat onbevoegden toegang kunnen krijgen tot gevoelige data. Deze versleuteling is al geïmplementeerd voor LockBox, alleen heeft deze implementatie nog een aantal problemen.

Deze deelvraag zal worden gebruikt om te achterhalen wat de problemen zijn met de huidige implementatie, en of WebAssembly daadwerkelijk goed gebruikt kan worden in het LockBox project. Hiervoor zullen een aantal criteria van LockBox worden behandeld die belangrijk zijn voor de encryptie. Vervolgens kan achterhaald worden of WebAssembly hierbij daadwerkelijk beter is voor LockBox dan een standaard JavaScript implementatie. 

## Het probleem
De huidige manier waarop AES-GCM encryptie is geïmplementeerd voor LockBox is het best te uit te leggen aan de hand van het [C2 container diagram](http://localhost:3000/rikdegoede-s6-docs/docs/Application-Design/Design-Document#c2---containerdiagram). Deze bevat namelijk een service genaamd de *"FileStorageService"*. Deze service is verantwoordelijk voor het opslaan van bestanden in een database zodat gebruikers deze later kunnen ophalen. Op het moment is dit niet het enige wat de service doet, deze service versleuteld namelijk ook de bestanden.

Deze implementatie werkt prima, maar komt niet heel betrouwbaar over en brengt een aantal risico's met zich mee. 

### Betrouwbaarheid
Het grootste probleem van deze implementatie is dat dit geen **end-to-end** encryption is. Momenteel worden bestanden pas versleuteld in de backend. Dit betekend dat zowel het niet versleutelde bestand, als het wachtwoord om deze mee te versleutelen naar de backend gestuurd moeten worden. Het maakt hierbij niet uit of deze implementatie veilig is, want voor de gebruiker is dit sowieso al niet te vertrouwen. *Wie weet wat LockBox met het verkregen wachtwoord doet.* Aangezien deze in de backend ontvangen wordt zou dit kunnen betekenen dat de ontwikelaars van LockBox toegang hebben tot alle gebruikers encryptie wachtwoorden. Niet heel betrouwbaar.

### Extra risico
- Niet versleutelde data over netwerk
- meestal geen probleem door HTTPS, maar toch extra risico.

---
## Criteria
De volgende criteria zijn belangrijk voor het LockBox project en het encryptie process dat zich plaatsvindt. Deze criteria kunnen later worden gebruikt om te achterhalen of WebAssembly mogelijk een betere keuze is dan JavaScript. 

### Encryptie standaard
(Must have) [Non-functional requirement 1](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/analyse-document#non-functional-requirements) van het LockBox project luidt: *"Opgeslagen bestanden worden versleuteld met `AES-GCM` met 256 bits keys."* Deze requirement is opgezet om er voor te zorgen dat de meest recente encryptie standaard wordt gebruikt. De reden dat voor AES-GCM met 256 bit keys is gekozen, is omdat dit algoritme *onder de door NIST goedgekeurde algoritmes valt.* 

### Bestand opslaan tijd
(Should have) [Non functional requirement 6](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/analyse-document#non-functional-requirements) van het LockBox project luidt: *"Het opslaan van een bestand duurt maximaal 1 minuut."* Dit geldt uiteraard voor de ervaring van de gebruiker, en WebAssembly zou deze tijd kunnen verlengen bij een slechte performance. 

Voor deze non-functional requirement zal het gebruik van WebAssembly gelukkig niet veel uit moeten maken aangezien het de huidige JavaScript implementatie kan vervangen. 

### Bestand's grootte
(Should have) [Non functional requirement 15](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/analyse-document/#non-functional-requirements) luidt: *"Gebruikers kunnen bestanden opslaan met een maximale grote van `1 GB`"*. Dit wil zeggen dat de applicatie ook in staat moet zijn om bestanden van dit formaat te versleutelen. Deze eis is voornamelijk in combinatie met de snelheidsvereisten mogelijk lastig te combineren. 