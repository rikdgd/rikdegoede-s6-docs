---
sidebar_position: 4
---
# 3. Hoe kan WebAssembly worden geïntegreerd in het LockBox project?

Om te achterhalen hoe WebAssembly gebruikt kan worden voor de encryptie in de LockBox frontend is onderzoek gedaan naar hoe WebAssembly in LockBox geïntegreerd kan worden. Hiervoor is eerst in  een hackthon een soort *proof of concept* opgezet, en zijn een aantal best practices achterhaald die voor LockBox belangrijk zijn. 


## Hackathon
Om niet te veel tijd kwijt te verliezen en toch een goed idee te krijgen van de bruikbaarheid van WebAssembly voor encryptie van bestanden in de frontend is  een hackathon gehouden. De hackathon implementatie is gemaakt binnen een tijdslimiet van 1.5 uur, en is ontwikkeld in de stappen die hier worden uitgelegd.
### 1. Compileren naar Wasm
De eerste stap bij het ontwikkelen van [de WebAssembly encryptie oplossing](https://github.com/LockBox-CS-S7/lockbox-wasm-aes) is het schrijven van de encryptie logica die naar WebAssembly gecompileerd moet worden. Dit kan in meerdere talen worden gedaan, LockBox gebruikt hiervoor **Rust**. Er zal een speciaal project type gebruikt moeten worden om te zorgen dat het project kan compileren. Daarom wordt het type van het project naar een `cdylib` veranderd, en een dependency toegevoegd voor het genereren van WebAssembly bindings: 
```toml
[lib]  
crate-type = ["cdylib"]  
  
[dependencies]  
wasm-bindgen = "0.2"
```


### 2. Blootstellen van Wasm functions
Voor web projecten wordt WebAssembly meestal aangeroepen via JavaScript. Hiervoor moeten wel bindings gemaakt worden naar de WebAssembly logica die JavaScript kan aanroepen. Voor AES-GCM encrypie zijn daarom in LockBox de volgende bindings aangemaakt:
```rust
#[wasm_bindgen]
pub fn encrypt_bytes(bytes: Vec<u8>, password: &str) -> Vec<u8> {
    encrypt(&bytes, password).expect("Failed to encrypt the received data")
}

#[wasm_bindgen]
pub fn decrypt_bytes(bytes: Vec<u8>, password: &str) -> Vec<u8> {
    let data = EncryptedData::from_bytes(bytes);
    decrypt(data, password).expect("Failed to decrypt the ciphertext")
}
```
De `encrypt_bytes` binding maakt het mogelijk om een reeks bytes te versleutelen met AES-GCM en het gebruik van een wachtwoord. De `decrypt_bytes` is juist weer in staat om versleutelde bytes te ont-sleutelen.

### 3. Gebruiken van Wasm module
Wanneer de Wasm logica is gecompileerd kan deze worden gebruikt door de browser. In het voorbeeld hier onder is zichtbaar hoe dit op een minimalistisch manier kan worden gedaan:
```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>hello-wasm example</title>
  </head>
  <body>
    <h1>WebAssembly AES encryption demo</h1>
    <input id="password" type="password" placeholder="password"/>
    <button id="encrypt-button">encrypt</button>
    <script type="module">
      <!-- Import the AES encryption bindings -->
      import init, { encrypt_bytes, decrypt_bytes } from "./pkg/aes_encryption.js";

      init().then(() => {
        document.getElementById("encrypt-button").addEventListener('click', () => {
          const demoData = "File data here...";
          const pass = document.getElementById('password').value;
          const ciphertext = encrypt_bytes(demoData, pass);
        });
      });
    </script>
  </body>
</html>

```

Deze simpele HTML pagina laadt de WebAssembly library in door de `init` functie aan te roepen. Wanneer deze is voltooid kan de WebAssembly logica gebruikt worden, daarom wordt hierna pas de click handler uitgewerkt. 

Ook is het belangrijk dat de JavaScript code van het type `module` is om het gebruik van WebAssembly mogelijk te maken. Anders kunnen de `init` functie en WebAssembly bindings niet geïmporteerd worden. 

### Resultaat
Er is nu een minimale implementatie voor het versleutelen van bestanden in de frontend aanwezig. Helaas kan deze nog niet gebruikt worden zoals deze er nu uitziet aangezien *random number generation* niet werkt in de huidige implementatie. Dit is nodig voor encryptie een maakt de huidige versie onbruikbaar. Helaas was er alleen niet genoeg tijd over in de hackathon om dit werkend te krijgen. Het probleem zou opgelost kunnen worden door de volgende handleiding te volgen: https://docs.rs/getrandom/latest/getrandom/#webassembly-support

Wel is nu duidelijk hoe de WebAssembly implementatie gebruikt kan worden in de frontend. Dit is een goede stap aangezien de frontend hier nu al op ingericht zou kunnen worden. 

---

## Veilig en efficient WebAssembly gebruik
WebAssembly kan op verschillende manieren gebruikt worden met ieder zijn voor- en nadelen. Wel zullen er een aantal handelingen zijn die meestal goed zijn om uit te voeren, of juist helemaal niet. Om te zorgen dat WebAssembly op een correcte manier wordt gebruikt door LockBox zijn ook een aantal *best, good and bad practices* achterhaald. Deze zijn hier terug te vinden.

### Memory management
WebAssembly komt niet met een garbage collector, de ontwikkelaar zal daarom zelf geheugen moeten alloceren en de-alloceren. Hier zijn verschillende strategieën voor, zo kan er voor gekozen worden om C/C++ `malloc / free` te gebruiken. Ook is het mogelijk om Rust te gebruiken en het ownership model van deze taal te gebruiken. Dit is deels de reden waarom voor LockBox gekozen is om Rust te gebruiken voor de encryptie implementatie.

### WebAssembly limitaties
WebAssembly is een geweldige technology, maar het heeft uiteraard ook zijn limitaties. Zo is het bijvoorbeeld vrij lastig om WebAssembly te debuggen, of werken niet alle packages zoals verwacht. Dit was bijvoorbeeld al te merken bij de **hackathon** met random number generation. Om deze reden is het vaak een goed idee om de software niet onmiddellijk naar WebAssembly to compileren, maar eerst naar een ander soort binary zodat deze makkelijker getest kan worden. 

### Complexere oplossing
Een ander probleem dat WebAssembly heeft, is het feit dat het veel meer moeite kost om WebAssembly te ontwikkelen dan JavaScript. Wanneer performance niet van belang is kost het vaak te veel tijd om WebAssembly te gebruiken. Het is simpelweg niet altijd de moeiete waard. Daarom is het belangrijk om goed te testen hoe groot het verschil in performance gaat zijn, en om te achterhalen hoe belangrijk de performance überhaupt is. 

### Samenvatting
WebAssembly is de natuurlijke vervanger van JavaScript, dit komt door de grote performance verbeteringen en het feit dat ze beiden voor hetzelfde platform bedoeld zijn. Wel zijn er een aantal practices waar ontwikkelaars rekening mee moeten houden bij het ontwikkelen van WebAssembly. Het is niet altijd de beste oplossing en kost vaak meer moeite om te ontwikkelen. 

---

## Conclusie
Om WebAssembly te integreren in de LockBox applicatie voor encryptie zijn een aantal stappen nodig. Het is niet heel lastig en zorgt ervoor dat de encryptie waarschijnlijk sneller zal verlopen, en dat de ontwikkelaars met een taal kunnen werken waar zij comfortabel mee zijn. Toch heeft de implementatie wel als nadeel dat deze meer stappen nodig zal hebben in het integratie proces. Voor LockBox zal dit geen groot probleem zijn aangezien WebAssembly enkel voor de encryptie gebruikt zal worden.