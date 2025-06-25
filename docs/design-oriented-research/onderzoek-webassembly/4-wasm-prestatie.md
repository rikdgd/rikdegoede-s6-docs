---
sidebar_position: 5
---
# 4. Hoe presteert WebAssembly in vergelijking met JavaScript bij het versleutelen van bestanden?

de hoofdvraag van dit onderzoek: **"Hoe kan WebAssembly ingezet worden bij het versleutelen van bestanden in de frontend van de LockBox applicatie?"** is eigenlijk al beantwoord. Alleen is een ding nog niet duidelijk: is het het echt waard om WebAssembly te gebruiken. Bij deze deelvraag zal worden achterhaald hoe de WebAssembly implementatie presteert in verhouding tot een JavaScript implementatie. 


## Performance test
Voor de LockBox applicatie is de snelheid waarop de encryptie plaatsvind van belang volgens non-functional requirement 6: *"Het opslaan van een bestand duurt maximaal 1 minuut"*. Daarom is een performance test uitgevoerd op `AES-GCM` implementaties in WebAssembly en JavaScript om te zien welke een snellere encryptie tijd heeft. Hierbij zijn de volgende constanten gebruikt: 
- Hetzelfde test bestand van `2.5 MiB` is gebruikt
- De meting is enkel uitgevoerd van de start tot stop van het encryptie proces. Dus niet de file upload time.
- Het wachtwoord *"pass"* is steeds gebruikt.
- De applicatie wordt gehost op een python server via het shell command: `python3 -m http.server`

Dit gaf de volgende resultaten:

### WebAssembly / Rust implementatie
Vijf keer achter elkaar het bestand versleutelen gaf de volgende resultaten, deze zijn allemaal in **milliseconden**:

| min | max | gemiddeld |
| --- | --- | --------- |
| 350 | 368 | 356       |

Dit is aardig snel, de vraag is alleen of de JavaScript implementatie veel minder snel is. 

### JavaScript implementatie
Vijf keer achter elkaar het bestand versleutelen gaf de volgende resultaten, deze zijn allemaal in **milliseconden**:

| min | max | gemiddeld |
| --- | --- | --------- |
| 42  | 59  | 41.8      |

Dit is bijna een factor 10 sneller dan de WebAssembly implementatie.


## Conclusie
De WebAssembly implementatie koste niet alleen veel meer moeite om te implementeren, de encryptie snelheid is bijna het tienvoudige! Dit is niet het verwachtte antwoord van dit onderzoek en geeft daarom een onverwachte wending aan het design van LockBox. De belangrijkste requirement voor de encryptie (buiten de veiligheid, die we met `AES-GCM` dekken) is de snelheid. Daarom is het waarschijnlijk beter om in de toekomst terug te stappen naar een JavaScript implementatie.