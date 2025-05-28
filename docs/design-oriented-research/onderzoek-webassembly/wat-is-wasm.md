---
sidebar_position: 2
---
# 1. Wat is WebAssembly en hoe werkt het?

Voor achterhaald kan worden of WebAssembly goed gebruikt kan worden voor LockBox, moet onderzocht worden wat het überhaupt is en waarom het bestaat. Om hier achter te komen moet eerst onderzocht worden welk probleem Wasm probeert op te lossen. 

## Het probleem met webapplicaties
Met de jaren zijn de web applicaties die we ontwikkelen steeds complexer geworden. Alle websites moeten interactief zijn, mooie effecten bevatten, en vaak ook logica afhandelen. Hier zijn verschillende opmerkingen over te maken, maar een ding is gegarandeerd: al deze extra logica maakt de applicaties zwaarder om te draaien. 

De populairste manier om deze logica uit te werken is met behulp van `JavaScript`. Dit is een interpreted programmeer taal die enorm veel wordt gebruikt. De taal is simpel in gebruik en enorm bekend wat het meestal een goede keuze maakt voor het ontwikkelen van webapplicaties. Wel heeft de taal een nadeel: performance.

Het feit dat webapplicaties steeds complexer worden heeft er voor gezorgd dat er behoefte is aan een andere, snellere oplossing voor het verwerken van logica in webapplicaties.


## Wat is WebAssembly?
Op de website van WebAssembly is de volgende definitie terug te lezen: <br/>
*WebAssembly (abbreviated Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications.*

Dit is een hele boel informatie in één zin, maar kort samengevat is WebAssembly een binair formaat dat uitgevoerd kan worden door de browser. WebAssembly is dus geen eigen taal, en er kunnen verschillende talen gebruikt worden voor het ontwikkelen van WebAssembly. Het voordeel hiervan is dat er geen losse interpreter meer nodig is om de code uit te kunnen voeren. Dit maakt WebAssembly een stuk efficiënter en sneller om uit te voeren dan JavaScript code. 


