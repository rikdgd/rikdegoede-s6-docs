---
sidebar_position: 2
---
# Life Cycle
Bij het ontwikkelen van software doorloopt de ontwikkelaar meestal verschillende fases. Al deze fases bij elkaar worden ook wel de **Software Development Life Cycle** (SDLC) genoemd. Deze fases zijn in de afbeelding hier onder nog eens gevisualiseerd, en zoals te zien is het een cyclus die de ontwikkelaar meerdere malen zal doorlopen zolang de eisen van de software veranderen. 

Om software op een veilige manier te ontwikkelen is het belangrijk dat de ontwikkelaar in iedere fase van deze cyclus bezig is met de security. Het ontwikkelen van een veilige applicatie vereis namelijk meer dan het toepassen van een aantal standaarden. Iedere applicatie is anders en fouten in het design kunnen tot grote gevolgen leiden.

In dit document is terug te lezen hoe voor het LockBox project rekening is gehouden met security in iedere fase van de **SDLC**. Hierbij is vooral de focus gelegd op de volgende twee [user stories](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/analyse-document#user-stories):
- **(US-01)** Als gebruiker wil ik bestanden _veilig_ kunnen opslaan.
- **(US-02)** Als gebruiker wil ik een opgeslagen bestand kunnen downloaden.


![SDLC diagram](./SDLC.png)


## 1. Planning
Bij het maken van een planning voor het LockBox project is ten eerste de keuze gemaakt om Agile te werk te gaan. Zo ontstaan er minder problemen in de planning wanneer een vulnerability wordt gevonden die spontaan prioriteit krijgt. 

Ook is voor LockBox de OWASP top 10 erbij gepakt om te zien welke vulnerabilities waarschijnlijk belangrijk gaan zijn voor LockBox. Aan de hand hiervan is een misuse-case diagram opgesteld, dit diagram laat zien wat potentieel de zwakke plekken zullen zijn voor de verschillende functionaliteiten. 

Wat betreft het uploaden en downloaden van bestanden, hier is genoeg tijd voor ingepland. Aangezien deze functionaliteiten zo belangrijk zijn is er goed op gelet om hier genoeg tijd voor vrij te maken. Ook is specifiek tijd ingepland om te onderzoeken hoe deze functionaliteiten het best beveiligd kunnen worden. Hier kwam in eerste instantie uit dat lokale opslag prima kan werken, tot dat tijdens onderzoek de cloud service *"[Supabase - storage](https://supabase.com/storage)"* gevonden werd. 

Dit onderzoek heeft ook geleid tot de volgende non-functional requirements: 
- **(must-have NF-01)** *"Opgeslagen bestanden worden versleuteld met `AES-GCM` met 256 bits keys."* 
- **(Should-have NF-01)** *"Bestanden worden enkel versleuteld over het netwerk verstuurd."*

Door de bestanden goed te versleutelen zijn deze automatisch ontoegankelijk zonder de private key. Dit maakt het in ieder geval vrij lastig om file content te lekken. De enige manier waarop dit nog zou kunnen gebeuren is door een fout in de encryptie implementatie, of in de toekomst door quantum computers.


## 2. Defining Requirements
Bij het opstellen van requirements is ook goed nagedacht over de beveiliging. In de planningsfase is al achterhaald dat cryptografische problemen ernstig zouden zijn voor LockBox. Daarom is de volgende non-functional requirement opgezet: 
*"Opgeslagen bestanden worden versleuteld met `AES-GCM` met 256 bits keys."* 
AES-GCM is een algoritme dat voldoet aan de hedendaagse [standaarden voor cryptografie volgens NIST](https://csrc.nist.gov/projects/block-cipher-techniques). 

Ook is de volgende non-functional requirement opgezet om te voorkomen dat de bestanden "gebruikt" worden door LockBox voordat deze versleuteld zijn:
*"Bestanden worden enkel versleuteld over het netwerk verstuurd."*

Buiten deze twee zijn er een aantal andere non-functional requirements opgesteld met als doel de veiligheid van de (web)applicatie waarborgen:
- **(NF-03)** De applicatie maakt gebruik van `HTTPS`.
	- Versleuteld netwerk verkeer voorkomt grotendeels dat anderen gevoelige informatie kunnen stellen uit HTTP requests.
- **(NF-12)** Data moet iedere 5 minuten worden gebackuped.
	- Data backups voorkomen geen vulnerabilities, maar verminderen wel de schade die deze kunnen aanrichten.
	- Hoe meer backups, hoe minder dataverlies bij ongevallen.
- **(NF-13)** Applicatie en systeem logs worden bewaard voor minstens 30 dagen.
	- Logs helpen bij het opsporen van problemen, het is belangrijk dat deze lang genoeg worden bewaard aangezien deze niet altijd snel gevonden hoeven te worden. 
- **(NF-14)** Het systeem bevat real-time monitoring van API gebruik, beschikbare opslag en performance.
	- Monitoring kan in het algemeen goed helpen bij het detecteren van problemen, bijvoorbeeld wanneer het aantal requests ineens 10x zo groot wordt. 
- **(NF-16)** De applicatie bevat geen vulnerabilities met een CVE score hoger dan 8.0.

*Deze non-functional requirements zijn ook [hier](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/analyse-document#non-functional-requirements) terug te vinden.*

Buiten dit is iedere sprint gecontroleerd of nieuwe features de veiligheid van de opgeslagen bestanden kunnen beïnvloeden. Daarom is bijvoorbeeld de *"File scanning service"* opgezet wanneer gebruikers bestanden moesten kunnen delen met elkaar. Hierover meer bij het onderdeel **"3. Designing Architecture"**.


## 3. Designing Architecture
Bij het designen van de architectuur zijn een aantal keuzes gemaakt met security in gedachten. Een verandering was echter bij verre het meest belangrijk voor de veiligheid van de opgeslagen bestanden. 

### end-to-end encryption
In de oude architectuur werd encryption namelijk uitgevoerd door de "file storage service". Aangezien deze service HTTPS gebruikt zijn de bestandsgegevens wel versleuteld op het netwerk, maar niet wanneer ze aankomen bij de service. Dit heeft 2 grote nadelen:
1. Mocht een malafide actor toegang krijgen tot de backend, dan kan deze technisch gezien de bestandsdata zien van bestanden die worden ge- upload/download.
2. Dit is geen end-to-end encryption, aangezien de bestanden niet bij de gebruiker worden versleuteld.

Daarom is de keuze gemaakt om de versleuteling in de frontend plaats te laten vinden. Zo is end-to-end encryptie aanwezig en wordt het een stuk lastiger om gevoelige data te stelen. Ook maakt dit de LockBox applicatie een stuk beter te vertrouwen aangezien de gebruiker nu zeker weet dat ook de ontwikkelaars van LockBox niet de bestanden kunnen uitlezen. 

### RBAC
Zo heeft de LockBox applicatie bijvoorbeeld de non-functional requirement: *"Het systeem bevat een `RBAC` systeem zodat iedere gebruiker de juiste rechten bezit"*. Het goed implementeren van een RBAC systeem is alleen geen makkelijke taak. Fouten in dit soort systemen zijn vaak catastrofaal aangezien deze fouten er voor kunnen zorgen dat gebruikers te veel rechten krijgen. 

Om te zorgen dat voor het LockBox project de kans op fouten in het RBAC systeem zo laag mogelijk blijven, is de keuze gemaakt om hier een cloud service voor te gebruiken. Daarom maakt LockBox gebruik van [Auth0](https://auth0.com/) voor het implementeren van een RBAC systeem. 


### File scanning service/function
In de tweede sprint is ook de keuze gemaakt om een extra service toe te voegen aan de applicatie. Deze service heeft als taak het controleren van geüploade bestanden om te garanderen dat deze geen malware bevatten. Als de bestanden malware bevatten maakt dit niet veel uit voor LockBox zelf aangezien de bestanden worden versleuteld. Wel zou het uitmaken voor de klanten aangezien op deze manier wel malware verspreid kan worden wanneer gebruikers de bestanden weer downloaden. 

In Sprint 6 is echter weer de keuze gemaakt om deze service te vervangen door een cloud function op Deno Deploy aangezien de service maar een functionaliteit bezit. Dit bespaard kosten en moeite wat betreft de hosting. Hier is meer over te lezen in het document: [Serverless functions](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Cloud-Native/serverless-functions).

## 4. Developing Product
Tijdens het ontwikkelen van het product zijn ook een aantal maatregelen genomen om security gerelateerde problemen te vermijden. Ook hierbij is uiteraard rekening gehouden met de veiligheid van de bestanden van de gebruiker. 

Ten eerste is voor development een docker compose omgeving opgezet met hierin een testing database voor bestanden. Zo kan de applicatie makkelijk worden getest zonder de daadwerkelijke gebruikers gegevens op het spel te zetten. 

Verder zijn voor de verschillende secrets, zoals bijvoorbeeld de connection string van de file database, steeds environment variables gebruikt in combinatie met lokale `.env` bestanden. Zo is voorkomen dat gevoelige data wordt gelekt, zonder dat dit veel moeite kost tijdens development.

Het LockBox project maakt ook gebruik van verschillende CI/CD pipelines. Hieraan is Snyk toegevoegd om automatisch SAST toe te passen op de LockBox applicatie. Voor het uploaden en downloaden van bestanden is het vooral belangrijk dat de applicatie goede input validation heeft. Zo niet dan zouden gebruikers namelijk bestanden kunnen uploaden die bijvoorbeeld te groot zijn, dit zou namelijk voor financiële problemen kunnen zorgen.  Om dit te voorkomen is daarom het testen van de frontend vooral van belang. Het testenvan de file storage service is ook geprobeerd, maar deze is in `Rust` geschreven en hiervoor hebben de populairste SAST tools (waaronder Snyk) nog geen support. 

*Klaar zo? Of nog iets toevoegen?* <br/>
*Iets uit change, risk, security, release -management misschien?*


## 5. Product Testing and Integration
- Automated testing
	- unit tests
	- Snyk SAST (ook hier, in beide fases wordt dit gebruikt)
- GitHub Actions pipelines
	- Gebruik van secrets in pipelines

## 6. Deployment and Maintenance of Products
- Deployement to dockerhub (voor scalable NF)
	- gebruik secrets
- logging
- Monitoring
	- prometheus
	- grafana