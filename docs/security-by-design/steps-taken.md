---
sidebar_position: 2
---
# Genomen stappen
Bij het ontwikkelen van software doorloopt de ontwikkelaar meestal verschillende fases. al deze fases bij elkaar noemt men ook wel de **Software Development Life Cycle** (SDLC). Deze fases zijn in de afbeelding hier onder nog eens gevisualiseerd, en zoals te zien is het een cyclus die de ontwikkelaar meerdere malen zal doorlopen zolang de eisen van de software veranderen. 

Om software op een veilige manier te ontwikkelen is het belangrijk dat de ontwikkelaar in iedere fase van deze cyclus bezig is met de security. Het ontwikkelen van een veilige applicatie vereis namelijk meer dan het toepassen van een aantal standaarden. Iedere applicatie is anders en fouten in het design kunnen tot grote gevolgen leiden.

In dit document is terug te lezen hoe voor het LockBox project rekening is gehouden met security in iedere fase van de **SDLC**. 

![SDLC diagram](./SDLC.png)


## 1. Planning
Bij het maken van een planning voor het LockBox project is ten eerste de keuze gemaakt om Agile te werk te gaan. Zo ontstaan er minder problemen in de planning wanneer een vulnerability wordt gevonden die spontaan prioriteit krijgt. 

Ook is voor LockBox de OWASP top 10 erbij gepakt om te zien welke vulnerabilities waarschijnlijk belangrijk gaan zijn voor LockBox. Aan de hand hiervan is een misuse-case diagram opgesteld, dit diagram laat zien wat potentieel de zwakke plekken zullen zijn voor de verschillende functionaliteiten. 

## 2. Defining Requirements
Bij het opstellen van requirements is ook goed nagedacht over de beveiliging. In de planningsfase is al achterhaald dat cryptografische problemen ernstig zouden zijn voor LockBox. Daarom is de volgende non-functional requirement opgezet: 
*"Opgeslagen bestanden worden versleuteld met `AES-GCM` met 256 bits keys."* 
AES-GCM is een algoritme dat voldoet aan de hedendaagse [standaarden voor cryptografie volgens NIST](https://csrc.nist.gov/projects/block-cipher-techniques). 

Ook zijn er een aantal andere non-functional requirements opgesteld met als doel de veiligheid van de applicatie waarborgen:
- **(NF-03)** De applicatie maakt gebruik van `HTTPS`.
	- Versleuteld netwerk verkeer voorkomt grotendeels dat anderen gevoelige informatie kunnen stellen uit HTTP requests.
- **(NF-12)** Data moet iedere 5 minuten worden gebackuped.
	- Data backups voorkomen geen vulnerabilities, maar verminderen wel de schade die deze kunnen aanichten.
	- Hoe meer backups, hoe minder dataverlies bij ongevallen.
- **(NF-13)** Applicatie en systeem logs worden bewaard voor minstens 30 dagen.
	- Logs helpen bij het opsporen van problemen, het is belangrijk dat deze lang genoeg worden bewaard aangezien deze niet altijd snel gevonden hoeven te worden. 
- **(NF-14)** Het systeem bevat real-time monitoring van API gebruik, beschikbare opslag en performance.
	- Monitoring kan in het algemeen goed helpen bij het detecteren van problemen, bijvoorbeeld wanneer het aantal requests ineens 10x zo groot wordt. 
- **(NF-16)** De applicatie bevat geen vulnerabilities met een CVE score hoger dan 8.0.

*Deze non-functional requirements zijn ook [hier](https://rikdgd.github.io/rikdegoede-s6-docs/docs/Application-Design/analyse-document#non-functional-requirements) terug te vinden.*

## 3. Designing Architecture
Bij het designen van de architectuur zijn een aantal keuzes gemaakt met security in gedachten...

*Voorbeelden*
- Auth0 gebruik, geen eigen auth implementatie
- File scanning service voor malware detectie
- *Gebruik van bekende, up-to-date, frameworks*

## 4. Developing Product

## 5. Product Testing and Integration

## 6. Deployment and Maintenance of Products
