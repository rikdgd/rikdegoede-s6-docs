---
sidebar_position: 1
---

# Introductie
In de hedendaagse wereld worden soms hoge eisen gesteld van software. Software moet geregeld in staat zijn grote hoeveelheden gebruikers tegelijkertijd aan te kunnen, en dit is een enorme opgave. Hier zijn verschillende strategieën en patronen voor die samen kunnen helpen dit doel te verwezenlijken. Een van deze patronen is de zogeheten *"microservice architectuur"*. Hier zal in meer details worden uitgelegd wat een microservice architectuur inhoud, wat de voor- en nadelen ervan zijn, en hoe het is geïmplementeerd in het LockBox project. 

## Wat is een microservice architectuur?
In het verleden werden applicaties vaak ontwikkeld als een *"monoliet"*. Dit houdt in dat de applicatie direct op een server/VPS wordt uitgerold en dus toegang heeft tot alle, en enkel, de resources van die machine. Dit is in de meeste gevallen een prima manier om een applicatie op te zetten en zal voor veruit de meeste projecten geen problemen veroorzaken. Dit veranderd echter wanneer een applicatie meerdere duizenden of zelf miljoenen gebruikers tegelijkertijd moet kunnen helpen. 

In een microservice architectuur wordt de applicatie opgesplitst in meerdere, kleinere services die elk verantwoordelijk zijn voor een specifieke taak of functionaliteit. In plaats van één groot geheel dat alles regelt, bestaat het systeem uit losse componenten die met elkaar communiceren via bijvoorbeeld HTTP API’s, messaging queues of andere vormen van netwerkverkeer.

### Voor- en nadelen
Elke microservice is onafhankelijk te ontwikkelen, testen, deployen en schalen. Dit biedt grote voordelen op het gebied van flexibiliteit en onderhoudbaarheid. Als bijvoorbeeld de betalingsfunctionaliteit veel gebruikt wordt, kan alleen die service opgeschaald worden zonder dat de rest van de applicatie mee hoeft te groeien. Bovendien kan elke service gebouwd worden in een technologie die het beste past bij het specifieke doel ervan, wat technologische diversiteit binnen één systeem mogelijk maakt.

Microservices worden vaak uitgerold in containers (zoals Docker) en beheerd via orchestratieplatformen zoals Kubernetes. Dit maakt het mogelijk om geautomatiseerd te schalen, fouten op te vangen en updates uit te rollen zonder downtime. Wel vereist dit een solide infrastructuur en een doordachte benadering van zaken zoals authenticatie, logging, monitoring en foutafhandeling.