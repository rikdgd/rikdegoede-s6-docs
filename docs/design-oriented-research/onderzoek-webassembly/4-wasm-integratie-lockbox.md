---
sidebar_position: 5
---
# 4. Hoe kan WebAssembly worden geïntegreerd in het LockBox project?

Om uit te leggen hoe WebAssembly in LockBox geintegreerd kan worden, kan het beste de  "lifecycle" worden bekeken. Hier zal daarom worden uitgelegd welke stappen worden doorlopen om WebAssembly code te ontwikkelen voor het LockBox project. 

## 1. Compile to Wasm
De eerste stap bij het ontwikkelen van WebAssembly is het schrijven van de logica die naar WebAssembly gecompileert moet worden. Dit kan in meerdere talen worden gedaan, wel moet de software vaak op een bepaalde manier worden ingericht. Wanneer de ontwikkelaar bijvoorbeeld `Rust` gebruikt voor het ontwikkelen van Wasm, dan zal wel een speciaal project type gebruikt moeten worden. Zo moet bijvoorbeeld het type van het project naar een `cdylib` worden veranderd, en een dependency worden toegevoegd: 
```toml
[lib]  
crate-type = ["cdylib"]  
  
[dependencies]  
wasm-bindgen = "0.2"
```

Vervolgens kan Wasm logica worden gebruikt via bindings die in JavaScript kunnen worden aangeroepen. Dit stukje code geeft een voorbeeld van een Wasm binding:
```rust
#[wasm_bindgen]  
pub fn greet(name: &str) {  
    println!("Hello, {}!", name);
}
```

## 2. Load module
Wanneer de Wasm logica is gecompileerd kan deze worden gebruikt door de browser. 