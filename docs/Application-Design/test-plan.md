---
sidebar_position: 3
---
# Test plan

## Introductie
LockBox is een micro-service applicatie waarmee gebruikers veilig bestanden kunnen opslaan. Ze kunnen de bestanden uploaden, downloaden, verwijderen, en delen met elkaar. De applicatie bevat gebruikers accounts om de geüploade bestanden to koppelen aan de gebruiker, en om ze prive te houden. 

## Test doelen
 - Zorg dat alle functionaliteiten werken zoals verwacht, zoals het uploaden, downloaden, verwijderen en delen van bestanden.
 - Verifiëren dat de applicatie mee schaalt met het aantal gebruikers, en dat de applicatie het aantal gebruikers zoals aangegeven in de non-functional requirements aan kan.
 - Controleren of de applicatie voldoet aan de gestelde security eisen. 

---

## Test scope
Er zijn een aantal functionaliteiten extra belangrijk om te testen voor de LockBox applicatie. Dit kan komen door het feit dat de functionaliteit nodig is om de applicatie te gebruiken, of bijvoorbeeld omdat er een hoge kans op vulnerabilities is. Hier zal worden aangegeven welke specifieke onderdelen getest moeten worden.

### core-functionaliteiten
 - Uploaden van een bestand.
 - Downloaden van een geuploaded bestand.
 - Verwijderen van een bestand.
 - Delen van een bestand.
 - Aanmaken van een account.
 - Inloggen op een account.
 - Aanpassen van account details.
 - Verwijderen van een account.
 - Ontvangen van een melding.
 - Bestand verwijderen als admin.
 - Accounts beheren als admin.
	 - Aanmaken van accounts.
	 - Aanpassen van account details.
	 - Verwijderen van accounts.
 - Automatisch schalen van de applicatie.

### security
- Statische code analyse voor bekende vulnerabilities.
- Cryptografische analyse van versleutelde bestanden. <br/> 
  *Dit is van belang voor het geval dat `AES-GCM` encryptie verkeerd is geïmplementeerd.*
- Docker testing voor het vinden van vulnerabilities in containers.

### out of scope
- Responsiveness van de frontend applicatie op verschillende apparaten/schermen.

---
## Test aanpak
### Functionele tests
De functionele tests worden gebruikt om te verifiëren dat alle applicatie functionaliteiten werken zoals verwacht. Bij deze tests is het daarom niet enkel belangrijk om losse onderdelen te testen, maar het geheel van de applicatie net zo goed.

**End to end testing** zal worden gebruikt om te controleren dat de applicatie in zijn geheel functioneert zoals verwacht. End to end testing is hiervoor ideaal aangezien zo gebruikers acties het meest accuraat geautomatiseerd kunnen worden. <br/>
De tool die gebruikt zal worden voor end to end testing is *"Cypress"*.

**Unit testing** is ideaal voor het testen van kleinere onderdelen van de applicatie. Unit testing zal er voor zorgen dat sommige fouten al vroeg in het test proces kunnen worden tegengehouden. <br/>
Voor unit testing zullen verschillende tools gebruikt moeten worden, afhankelijk van de taal/framework die iedere service zal gebruiken. 

**Load testing** moet worden toegepast om te controleren dat de applicatie het aantal gebruikers aankan waarvoor het is ge-designed. Ook zal load testing worden gebruikt om te verifiëren dat de applicatie automatisch horizontaal schaalt. <br/>
Voor het uitvoeren van load testing zal *"Locust"* worden gebruikt. 

**Security testing (static code analysis)** is cruciaal om te voorkomen dat vulnerabilities in de software van het LockBox project belanden. Met behulp van *"static code analysis"* kunnen automatisch rapportages worden gemaakt van de gevonden vulnerabilities. <br/>
*"Snyk"* zal worden gebruikt voor het uitvoeren van security testing.

---

## Test cases

| Test case | Use case | Invoer | Verwachte uitvoer | Resultaat |
| --------- | -------- | ------ | ----------------- | --------- |
| TC 01     |          |        |                   |           |


---
## Risico's en problemen
Highlight hier de risico's en problemen die kunnen ontstaan tijdens het testen van de software.