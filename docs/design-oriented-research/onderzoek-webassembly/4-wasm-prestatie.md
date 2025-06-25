---
sidebar_position: 5
---
# 4. Hoe presteert WebAssembly in vergelijking met JavaScript bij het versleutelen van bestanden?

de hoofdvraag van dit onderzoek: **"Hoe kan WebAssembly ingezet worden bij het versleutelen van bestanden in de frontend van de LockBox applicatie?"** is eigenlijk al beantwoord. Alleen is een ding nog niet duidelijk: is het het echt waard om WebAssembly te gebruiken. Bij deze deelvraag zal worden achterhaald hoe de WebAssembly implementatie presteert in verhouding tot een JavaScript implementatie. 


## Performance test
Voor de LockBox applicatie is de snelheid waarop de encryptie plaatsvind van belang volgens non-functional requirement 6: *"Het opslaan van een bestand duurt maximaal 1 minuut"*. Daarom is een performance test uitgevoerd op `AES-GCM` implementaties in WebAssembly en JavaScript om te zien welke een snellere encryptie tijd heeft. Dit gaf de volgende resultaten:

### WebAssembly / Rust implementatie
*Waarom werkt het niet :(*
