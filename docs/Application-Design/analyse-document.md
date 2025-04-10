---
sidebar_position: 1
---
# Analyse document
# User stories & Requirements 

Hier zijn de user stories en non-functional requirements voor de LockBox applicatie terug te lezen. De user stories geven aan welke functionaliteiten de applicatie zal moeten bezitten, terwijl de non-functional requirements aangeven aan welke eisen de applicatie moet voldoen. 

## User stories

User stories zijn gebruikt om de functionaliteiten die de **LockBox** applicatie nodig heeft in kaart te brengen. Ze geven aan hoe verschillende soorten gebruikers de applicatie moeten kunnen gebruiken. 

1. Als gebruiker wil ik bestanden *veilig* kunnen opslaan.
2. Als gebruiker wil ik een opgeslagen bestand kunnen downloaden.
3. Als gebruiker wil ik bestanden kunnen verwijderen van de applicatie.
4. Als gebruiker wil ik dat er een accountsysteem aanwezig is.
	1. Als gebruiker wil ik een account kunnen aanmaken.
	2. Als gebruiker wil ik kunnen inloggen op een account.
	3. Als gebruiker wil ik kunnen uitloggen van een account.
	4. Als gebruiker wil ik mijn account kunnen verwijderen.
5. Als gebruiker wil ik meldingen ontvangen van *belangrijke gebeurtenissen*. 
6. Als gebruiker wil ik bestanden kunnen delen via een tijdelijke URL.
7. Als admin wil ik in staat zijn bestanden te verwijderen, van zowel mijn eigen als andermans account.
8. Als admin wil ik in staat zijn accounts te beheren.
	1. Als admin wil ik een overzicht hebben van alle bestaande accounts.
	2. Als admin wil ik een account kunnen aanmaken.
	3. Als admin wil ik een account kunnen aanpassen.
	4. als admin wil ik een account kunnen verwijderen.
9. Als admin wil ik inzicht hebben in verschillende statistieken van de applicatie. 
<br/>
---
## Non-functional requirements

De uitgewerkte non-functional requirements geven aan welke eisen zijn opgesteld voor de **LockBox** applicatie. Deze eisen betreffen de algemene werking van de applicatie, maar ook de performance en security worden meegenomen.

### Must have
1. Opgeslagen bestanden worden versleuteld met `AES-GCM` met 256 bits keys.
2. Het systeem moet horizontaal schalen om meer/minder resources te gebruiken wanneer nodig. 

### Should have
1. Bestanden worden enkel versleuteld over het netwerk verstuurd.
2. De app maakt gebruik van JSON web tokens (JWT) voor authenticatie.
3. De applicatie maakt gebruik van `HTTPS`.
4. Gebruikers gegevens en geüploade bestanden worden verwijderd bij account verwijdering. 
5. De applicatie heeft een up-time van `99.3%`. <br/>
   *Het is belangrijk dat gebruikers zo goed als altijd toegang hebben tot hun opgeslagen bestanden. Het zou niet juist zijn als gebruikers meer dan 10 minuten per dag niet bij hun bestanden kunnen. Dit resulteert in een up-time percentage van `99.3%`.*
6. Het opslaan van een bestand duurt maximaal 1 minuut. 
7. Het verwijderen van een bestand duurt maximaal 1 minuut.
8. Het verwijderen van een account duurt maximaal 5 minuten.
9. Het systeem moet minimaal `900,000` gebruikers tegelijkertijd aankunnen. <br/>
   *De verwachting is dat LockBox mogelijk meerdere miljoenen gebruikers zal hebben. Deze zullen niet allemaal tegelijkertijd de applicatie gebruiken, dit zal uiterlijk liggen rond de 2 uur per dag in extreme gevallen. Stel de applicatie heeft 10.000.000 gebruikers die de applicatie allemaal 2 uur per dag gebruiken, dan resulteert dit in `10,000,000 * (2 / 24) = 833,333` gelijktijdige gebruikers. Dit is naar boven afgerond tot `900,000` om er zeker van te zijn dat het systeem het aantal gebruikers aan kan.*
10. Het systeem bevat een `RBAC` systeem zodat iedere gebruiker de juiste rechten bezit. 
11. Het systeem moet voldoen aan de eisen van **GDPR**.
12. Data moet iedere 5 minuten worden gebackuped.
13. Applicatie en systeem logs worden bewaard voor minstens 30 dagen.
14. Het systeem bevat real-time monitoring van API gebruik, beschikbare opslag en performance.
15. Gebruikers kunnen bestanden opslaan met een maximale grote van `1 GB`.
16. De applicatie bevat geen vulnerabilities met een CVE score hoger dan 8.0.
17. De applicatie heeft een *"test coverage"* van `60%`.


Zoals te zien zijn er twee non-functional requirements die vallen onder de categorie *"must have"*, dit zijn de belangrijkste requirements voor het LockBox project. Gedurende dit semester zal daarom de focus vooral op deze non-functional requirements liggen. De eerste requirement is belangrijk om te zorgen dat de gebruiker zijn bestanden daadwerkelijk veilig online kan opslaan. De andere requirement is extra belangrijk vanwege het grote aantal gebruikers dat de applicatie zal hebben. Altijd de applicatie op het volle vermogen laten draaien zou te duur zijn, dus moet de applicatie horizontaal kunnen schalen.

---

# Use-cases


| Naam         | UC-01: Bestand uploaden                                                                                                                                                                                                                                                                |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Samenvatting | Een ingelogde actor kan een bestand uploaden in de LockBox applicatie.                                                                                                                                                                                                                 |
| Actors       | gebruiker                                                                                                                                                                                                                                                                              |
| Aannamen     | De actor is ingelogd.                                                                                                                                                                                                                                                                  |
| Scenario     | 1. De actor klikt op "bestand uploaden"<br/>2. Het systeem opent een file upload menu.<br/>3. De actor selecteert een bestand van zijn/haar locale machine.<br/>4. De actor klikt op "upload".<br/>5. Het systeem laat een melding zien dat het bestand wel/niet succesvol is ge-uploaded. |
| Uitzondering | Het bestand is groter dan `1GB` (non-functional requirement 15)                                                                                                                                                                                                                        |
| Resultaat    | De actor heeft het bestand ge-uploaded op het ingelogde account.                                                                                                                                                                                                                       |

| Naam         | UC-02: Bestand downloaden.                                                                                                                                                                                                                                                                                              |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Samenvatting | De actor kan een opgeslagen bestand downloaden.                                                                                                                                                                                                                                                                         |
| Actors       | gebruiker                                                                                                                                                                                                                                                                                                               |
| Aannamen     | De actor is ingelogd.<br/>De actor heeft een bestand opgeslagen in de applicatie.                                                                                                                                                                                                                                       |
| Scenario     | 1. De actor navigeert naar het gewenste bestand en klikt op de bestand's naam.<br/>2. Het systeem laat details zien van het bestand, zowel als verschillende acties die de gebruiker kan ondernemen.<br/>3. De actor klikt op "download".<br/>4. Het systeem haalt het bestand op uit de database en start de download. |
| Uitzondering | De actor heeft geen rechten tot het bestand<br/>*De actor zou niet in staat moeten zijn het bestand te zien, toch is extra controle hierop nodig.*                                                                                                                                                                      |
| Resultaat    | De actor heeft het bestand gedownload naar zijn/haar locale machine.                                                                                                                                                                                                                                                    |

| Naam         | UC-03: Bestand verwijderen                                                                                                                                                                                                                                                                                   |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Samenvatting | De actor kan een opgeslagen bestand uit de applicatie verwijderen.                                                                                                                                                                                                                                           |
| Actors       | gebruiker                                                                                                                                                                                                                                                                                                    |
| Aannamen     | De actor is ingelogd.<br/>De actor heeft een bestand opgeslagen in de applicatie.                                                                                                                                                                                                                            |
| Scenario     | 1. De actor navigeert naar het gewenste bestand en klikt op de bestand's naam.<br/>2. Het systeem laat details zien van het bestand, zowel als verschillende acties die de gebruiker kan ondernemen.<br/>3. De actor klikt op "delete".<br/>4. Het systeem verwijderd het de bestand's data uit de database. |
| Uitzondering | Het bestand is al in het proces van verwijdering.<br/>De actor heeft geen rechten tot het bestand.                                                                                                                                                                                                           |
| Resultaat    | Het gewenste bestand is verwijderd uit het systeem.                                                                                                                                                                                                                                                          |

