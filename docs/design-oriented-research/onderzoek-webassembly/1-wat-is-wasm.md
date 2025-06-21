---
sidebar_position: 2
---
# 1. Wat is WebAssembly en hoe werkt het?

Voor achterhaald kan worden of WebAssembly *(Wasm)* goed gebruikt kan worden voor LockBox, moet onderzocht worden wat het überhaupt is en waarom het bestaat. Om hier achter te komen moet eerst onderzocht worden welk probleem Wasm probeert op te lossen. 

## Het probleem met webapplicaties
Met de jaren zijn de web applicaties die we ontwikkelen steeds complexer geworden. Alle websites moeten interactief zijn, mooie effecten bevatten, en vaak ook logica afhandelen. Hier zijn verschillende opmerkingen over te maken, maar een ding is gegarandeerd: al deze extra logica maakt de applicaties zwaarder om te draaien. 

De populairste manier om deze logica uit te werken is met behulp van `JavaScript`. Dit is een interpreted programmeer taal die enorm veel wordt gebruikt. De taal is simpel in gebruik en enorm bekend wat het meestal een goede keuze maakt voor het ontwikkelen van webapplicaties. Wel heeft de taal een nadeel: performance.

Het feit dat webapplicaties steeds complexer worden heeft er voor gezorgd dat er behoefte is aan een andere, snellere oplossing voor het verwerken van logica in webapplicaties.


## Wat is WebAssembly?
Op de website van WebAssembly is de volgende definitie terug te lezen: <br/>
*WebAssembly (abbreviated Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications.* [[1]](https://webassembly.org)

Dit is een hele boel informatie in één zin, maar kort samengevat is WebAssembly een binair formaat dat uitgevoerd kan worden door de browser. WebAssembly is dus geen eigen taal, en er kunnen verschillende talen gebruikt worden voor het ontwikkelen van WebAssembly. Het voordeel hiervan is dat er geen losse interpreter meer nodig is om de code uit te kunnen voeren. Dit maakt WebAssembly een stuk efficiënter en sneller om uit te voeren dan JavaScript code. 

WebAssembly is niet gelimiteerd aan de browser, het kan ook draaien in de backend door bijvoorbeeld Node.js te gebruiken [[3]](https://nodejs.org/en/learn/getting-started/nodejs-with-webassembly). Dit maakt het mogelijk om de gecompileerde WebAssembly te hergebruiken in zowel de frontend als backend. 

## Het gebruiken van WebAssembly
Om WebAssembly the kunnen gebruiken in een webapplicatie moet deze worden aangeroepen via JavaScript. Bij het compileren van WebAssembly wordt bij de meeste talen automatisch een JavaScript module aangemaakt met bindings naar de gecompileerde WebAssembly. Dit kan de ontwikkelaar overigens ook zelf handmatig implementeren zonder al te veel moeite. Hoe dan ook zijn deze bindings nodig om de WebAssembly logica aan te roepen. 

### Waarvoor wordt WebAssembly gebruikt?
WebAssembly kan voor veel verschillende doeleinden worden toegepast, maar het meest wordt het gebruikt in webapplicaties. WebAssembly kan echter wel in de backend worden gebruikt, alleen is dit niet het gebied waarop WebAssembly het best presteert. 

In de officiele documentatie staan een aantal voorbeelden gegeven waarvoor WebAssembly ideaal gebruikt kan worden, een aantal staan hier genoteerd [[1]](https://webassembly.org/docs/use-cases/):

- *Games*
- *Language interpreters and virtual machines.*
- *POSIX user-space environment, allowing porting of existing POSIX applications.*
- *Developer tooling (editors, compilers, debuggers, …).*
- *Remote desktop.*
- *VPN.*
- *Encryption.*
- *Local web server.*

Zoals te zien staat hier ook **encryption** tussen, laat dit nu precies zijn waar dit onderzoek voor wordt uitgevoerd. De hoofdvraag van dit onderzoek luidt namelijk: ***"Hoe kan WebAssembly ingezet worden bij het versleutelen van bestanden in de frontend van de LockBox applicatie?"***

## Samenvatting
WebAssembly is een vorm van bytecode die door de browser en interpreters kan worden uitgevoerd. Het is een stuk efficiënter dan JavaScript voor het uitvoeren van berekeningen, wat applicaties een betere performance kan bieden. Ook kunnen ontwikkelaars verschillende talen gebruiken om naar WebAssembly te compileren. Dit maakt dat de ontwikkelaars een taal kunnen gebruiken waar ze bekend mee zijn. Hierdoor is WebAssembly een stuk makkelijker te integreren.

Kortom lijkt WebAssembly ideaal voor versleuteling in de frontend van de LockBox applicatie.

## Bronnen:
1. https://webassembly.org/
2. https://webassembly.github.io/spec/js-api/index.html
3. https://nodejs.org/en/learn/getting-started/nodejs-with-webassembly