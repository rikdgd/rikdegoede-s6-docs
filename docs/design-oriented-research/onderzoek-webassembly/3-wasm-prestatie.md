---
sidebar_position: 4
---
# 3. Hoe presteert WebAssembly in vergelijking met JavaScript bij het versleutelen van bestanden?

bla bla bla

## Risico's 
Een potentieel risico bij het gebruik van WebAssembly voor encryptie zijn *"Timing leaks"*. Een timing leak bij encryptie houdt in dat aan de hand van de encryptie tijd informatie achterhaald kan worden zoals het formaat van de data. Dit hoort niet het geval te zijn, het proces hoort altijd ongeveer even lang te duren. 

Een ander risico zit hem in het "exposen" van de WebAssembly logica naar JavaScript. Wanneer dit op een verkeerde manier wordt gedaan kan dit er voor zorgen dat aanvallers direct het geheugen van de applicatie kunnen aanpassen. 

Tot slot gebruikt niet ieder gebruiker dezelfde web browser. Dit wil zeggen dat de WebAssembly logica mogelijk niet overal exact hetzelfde wordt uitgevoerd/aangeroepen. Dit zou voor grote problemen kunnen zorgen, maar dit is moeilijk te bepalen. Het beste zou daarom zijn om dit uitgebreid te testen.