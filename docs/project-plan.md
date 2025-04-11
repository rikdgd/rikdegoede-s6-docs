---
sidebar_position: 2
---
# Project plan

## 1. Professional standard
Het ontwikkelen van een applicatie die is opgesplitst in meerdere losse micro-services. Dit zorgt ervoor dat de software automatisch omhoog/omlaag geschaald kan worden in resources, afhankelijk van hoeveel er per moment nodig is. Dit kan gedaan worden met tools zoals ‘Docker’ en ‘Kubernetes’.

Bij enterprise software kan men beargumenteren dat stabiliteit extra belangrijk is, daarom zullen er ook pipelines opgezet worden om de software automatisch te testen op verschillende onderdelen zoals onder andere: softwarekwaliteit, security en performance.

In het groepsproject zal er ook goed rekening gehouden moeten worden met de wensen van de stakeholder. Daarom zal er op een agile manier gewerkt worden, volgens het SCRUM framework.

## 2. Personal leadership
Het kan in een langlopend project moeilijk zijn om zelf het overzicht te houden. Hierdoor wordt het moeilijk om juiste keuzes te maken voor het project en dat is ongewenst. Om te zorgen dat ik zelf goed kan heersen over mijn project wil ik een scrum planning gaan bijhouden met alle epics zowel als leeruitkomsten als categorieën. Hierdoor kan ik zelf altijd makkelijk zien hoe het er met de applicatie voor staat, en met mijn leeruitkomsten.

## 3. Scalable architectures
Om deze leeruitkomst aan te tonen wil ik als eerst onderzoek gaan doen naar de verschillende strategieën voor het opzetten/indelen van de architectuur van enterprise software. Hoe hebben bekende bedrijven dit aangepakt? Wat zijn de voor en nadelen van verschillende strategieën?

De applicatie zal ook opgedeeld worden in meerdere losse microservices. De micro services kunnen worden opgedeeld in containers met tools zoals Docker. Wel zal kort onderzocht moeten worden of dit de beste oplossing is en uiteraard waarom.

Ook wil ik kijken naar hoe mijn applicatie voordeel kan halen uit het gebruiken van een message broker. Dit is een veel gebruikt onderdeel in micro-service applicaties en zou mij mogelijk kunnen helpen bij het schaalbaar maken van de architectuur.

De door mij ontwikkelde software zal ook getest moeten worden op performance en de scalability. Er zijn tools die dit mogelijk maken, bijvoorbeeld door een grote hoeveelheid gebruikers te simuleren. Hoe deze tools op mijn applicatie het best gebruikt kunnen worden, en welke onderdelen het belangrijkst zijn om te testen, zal ook onderzocht moeten worden.

Gemaakte ontwerpen zullen UML volgen om ervoor te zorgen dat andere softwareontwikkelaars de ontwerpen ook begrijpen.

## 4. DevOps
Om de kwaliteit van de ontwikkelde software aan te tonen, zal gebruikgemaakt worden van automatische tests. In het groepsproject zal hiervoor een pipeline worden opgezet. Deze zal enkel CI onderdelen bevatten, automatische deployment is namelijk niet de bedoeling van de opdrachtgever. In het individuele project kan Continuous Deployment worden toegepast. Als groep hebben we ook regels vastgesteld rondom het aantonen van kwaliteit, zo hebben we bijvoorbeeld gekeken naar welke onderdelen belangrijk zijn om te testen. Hierbij zijn tools uitgezocht die ons kunnen helpen in dit proces. 

## 5. Cloud native
Cloud services geven de ontwikkelaar de mogelijkheid om hardware/software te gebruiken die voor hen anders te veel geld zouden kosten om aan te schaffen/ontwikkelen. Dit kan bijvoorbeeld voordelig zijn wanneer een bepaalde service meer ‘traffic’ heeft dan de hardware van de ontwikkelaar aankan. Het is eigenlijk een vorm van huren, de ontwikkelaar kan ‘resources’ van een andere partij ‘huren’ voor zijn/haar eigen benodigdheden.

Om aan te tonen dat ik met deze services om kan gaan, zal ik een aantal van deze services moeten gebruiken/onderzoeken. Ook zal er een berekening gemaakt moeten worden van de verwachte kosten van deze cloud services, afhankelijk van de verwachte hoeveelheid gebruikers.


## 6. Security by design
Het ontwikkelen van een veilige applicatie is erg lastig, de ontwikkelaar moet goed rekening houden met bekende zwakheden in software en hoe deze voorkomen moeten worden.

Om veilige software te ontwikkelen zal ik dit semester onderzoek doen naar veel voorkomende zwakheden in (web-) applicaties. Ook zal onderzoek gedaan worden naar mogelijke tools die kunnen helpen bij het ontwikkelen van een veilige applicatie.

Buiten dit zal ik gebruik gaan maken van security testing om te verifieren dat mijn software voldoet aan de gestelde security eisen.

Tot slot ga ik kijken naar het OWASP SAMM framework om security in het gehele ontwikkelproces te betrekken.

## 7. Distributed data
Data is waardevol en in enterprise software is er vaak veel data aanwezig. Hoe men het best om kan gaan met deze data is altijd afhankelijk van de situatie, maar het is belangrijk om rekening te houden met de veiligheid en toegankelijkheid van de data. De data moet niet makkelijk toegankelijk zijn voor hackers, maar ook moet de data niet zomaar verloren kunnen raken. Hiervoor zijn verschillende oplossingen en strategieën en hier zal ik onderzoek naar doen.

Een ander probleem waar men tegenaan zal lopen is het feit dat opgeslagen data mogelijk niet altijd hetzelfde blijft tussen de verschillende microservice instanties aangezien deze vaak een eigen, losse database hebben. Hiervoor zal ook nog een oplossing gevonden moet worden.

Om deze leeruitkomst aan te tonen zal ik onderzoek doen naar deze problemen, zowel als hoe ze kunnen worden opgelost. Zo zal ik bijvoorbeeld kijken naar hoe verschillende type databases mij hiermee kunnen helpen.