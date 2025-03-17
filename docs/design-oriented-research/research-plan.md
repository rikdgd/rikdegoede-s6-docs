---
sidebar_position: 1
---
# Research Plan

## Hoofdvraag
**Hoe kan WebAssembly helpen bij de performance zodat mijn applicatie bestanden sneller kan versleutelen?**

De LockBox applicatie versleuteld momenteel alle data in de backend. Deze aanpak is echter voor de eindgebruiker niet zo betrouwbaar aangezien zij niet weten wat er in de backend met de cryptografische key wordt gedaan. Daarom zou het ideaal zijn om de encryptie in de frontend uit te voeren. 
Er is al eerder gekeken aan een JavaScript implementatie, maar het gebruiken van WebAssembly zou hierbij de performance voor de eindgebruiker potentieel verder kunnen verbeteren. 

Ik ben vooral tot deze hoofdvraag gekomen wegens mijn interesse in het onderwerp. Ik heb zelf nog helemaal geen ervaring met WebAssembly en zou hier graag meer over leren. 


## Deelvragen
1. Wat is WebAssembly en hoe werkt het?
2. Binnen welke tijd moet de LockBox applicatie bestanden kunnen versleutelen?
3. Hoe presteert WebAssembly in vergelijking met JavaScript bij het versleutelen van bestanden?
4. Hoe kan WebAssembly worden geïntegreerd in het LockBox project?

### Research methods (DOT framework)
**Deelvraag 1 (library / field)** <br/>
`Literature research` zal voor deze vraag vooral van belang zijn om te achterhalen wat WebAssembly is en hoe het gebruikt kan worden. Ook zal de officiële documentatie geraadpleegd worden, wat onder `document analysis` valt. 

**Deelvraag 2 (field / workshop)** <br/>
Om een duidelijk beeld te krijgen van het probleem met de huidige implementatie zal `problem analysis`  gebruikt worden.
Verder zijn er verschillende criteria zoals security, complexity en performance waarmee rekening gehouden moet worden bij het maken van de keuze tussen het wel of niet gebruiken van WebAssembly. `Multi-criteria decision making` is hiervoor de ideale onderzoeksmethode. 

**Deelvraag 3 (lab)** <br/>
De belangrijkste vraag is of een WebAssembly implementatie mogelijk sneller is dan een JavaScript versie. Hiervoor kan `benchmark testing` goed gebruikt worden om de verschillende implementaties met elkaar te vergelijken. 
Ook zullen de bestaande non-functional requirements meegenomen moeten worden in de nieuwe WebAssembly implementatie. Daarom zal `non-functional testing` hier ook gebruikt worden.

**Deelvraag 4 (library / workshop)** <br/>
Om een eerste implementatie te maken in de LockBox applicatie is een `hackathon` ideaal. Zo kan zonder te veel tijd te besteden een betere keuze gemaakt worden hoe WebAssembly het best geïntegreerd kan worden. 
Ook kan het waardevol zijn om te achterhalen wat de beste manier is om WebAssembly te gebruiken. Hiervoor is het uitzoeken van `best good and bad practices` daarom ideaal. 

