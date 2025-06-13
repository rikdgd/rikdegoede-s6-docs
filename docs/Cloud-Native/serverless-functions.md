---
sidebar_position: 3
---
# Serverless functions
_Serverless functions_ zijn kleine stukjes code die draaien in de cloud zonder dat je zelf servers hoeft te beheren. In plaats van een hele applicatie continu te hosten, worden deze functies automatisch uitgevoerd als reactie op specifieke gebeurtenissen, zoals een HTTP-aanvraag, een bestand dat wordt geüpload of een bericht in een wachtrij.

Het grote voordeel is dat je je alleen hoeft te focussen op de logica van je code, terwijl de cloudprovider (zoals AWS Lambda, Azure Functions of Google Cloud Functions) zorgt voor de infrastructuur, schaalbaarheid en uptime. Je betaalt bovendien alleen voor het daadwerkelijke gebruik – dus geen kosten als de functie niet draait.


## Serverless functions in LockBox
Voor de LockBox applicatie is ook de keuze gemaakt om servless functions te gebruiken. Hiervoor wordt het [Deno Deploy](https://deno.com/deploy) platform gebruikt aangezien Deno al wordt gebruikt voor LockBox, en het platform heeft een gratis oplossing om mee te starten. 

Serverless functions kunnen op verschillende manieren worden toegepast, maar over het algemeen zijn ze vooral efficient voor het uitvoeren van simpele taken. Een van de services van het LockBox project kan daarom mogelijk goed vervangen worden door serverless functions: de "file scanning service". Deze service zal namelijk enkel de geüploade bestanden controleren op malware, een redelijk simpele taak. 

Om te controleren of deze service daadwerkelijk goed vervangen kan worden door serverless functions is een voorbeeld function opgezet voor het LockBox project. Hoe dit is gedaan, en de voor- en nadelen van deze oplossing, is allemaal hier terug te lezen.