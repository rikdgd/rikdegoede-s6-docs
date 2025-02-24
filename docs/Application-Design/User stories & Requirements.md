---
sidebar_position: 1
---
## User stories

User stories zijn gebruikt om de functionaliteiten die de **LockBox** applicatie nodig gaat hebben in kaart te brengen. Ze geven aan hoe verschillende soorten gebruikers de applicatie moeten kunnen gebruiken. 

1. Als gebruiker wil ik een bestand *veilig* kunnen opslaan.
2. Als gebruiker wil ik een opgeslagen bestand kunnen downloaden.
3. Als gebruiker wil ik een bestanden kunnen verwijderen van de applicatie.
4. Als gebruiker wil ik dat er een account systeem aanwezig is.
	1. Account aanmaken.
	2. Inloggen.
	3. Uitloggen.
	4. Account verwijderen.
5. Als gebruiker wil ik meldingen ontvangen van belangrijke gebeurtenissen. 
6. Als gebruiker wil ik bestanden kunnen delen via een tijdelijke URL.
7. Als admin wil ik in staat zijn bestanden te verwijderen, van zowel mijn eigen als andermans account.
8. Als admin wil ik in staat zijn andermans account te verwijderen. 
9. Als admin wil ik inzicht hebben op verschillende statistieken van de applicatie. 

---
## Non-functional requirements

De uitgewerkte non-functional requirements geven aan welke eisen zijn opgesteld voor de **LockBox** applicatie. Deze eisen betreffen de algemene werking van de applicatie, maar ook de performance en security worden meegenomen.

1. Opgeslagen bestanden worden versleuteld met AES-GCM met 256 bits keys.
2. Bestanden worden enkel versleuteld over het netwerk verstuurd.
3. De app maakt gebruik van JSON web tokens (JWT) voor authenticatie.
4. De applicatie maakt gebruik van HTTPS (TLS).
5. Gebruikers gegevens en geüploade bestanden worden verwijderd bij account verwijdering. 
6. De applicatie heeft een up-time van **98%**.
7. Het opslaan van een bestand duurt maximaal 1 minuut. 
8. Het verwijderen van een bestand duurt maximaal 1 minuut.
9. Het verwijderen van een account duurt maximaal 5 minuten.
10. Het systeem moet minimaal 500.000 gebruikers te gelijkertijd aankunnen.
11. Het systeem moet horizontaal schalen om meer/minder resources te gebruiken wanneer nodig. 
12. Het systeem bevat een **RBAC** systeem zodat iedere gebruiker de juiste rechten bezit. 
13. Het systeem moet voldoen aan de eisen van **GDPR**.
14. Data moet worden gebackup-ed iedere 5 minuten.
15. Applicatie en systeem logs worden bewaard voor op zijn minst 30 dagen.
16. Het systeem bevat real-time monitoring van API gebruik, beschikbare opslag en performance.
