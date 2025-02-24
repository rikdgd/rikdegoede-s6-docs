---
sidebar_position: 2
---
# Design  Document 

## Inleiding
Dit document probeert het design van de gebouwde applicatie duidelijk te maken aan de hand van verschillende diagrammen en bijbehorende documentatie. 

## C4 diagrammen
C4 diagrammen worden gebruikt om de applicatie op een steeds dieper niveau uit te werken. Hier zullen een aantal van de C4 diagrammen worden gebruikt om het design van de applicatie te verduidelijken.

### C2 - Containerdiagram
De onderstaande containerdiagram toont het ontwerp van de LockBox-toepassing. Het geeft de verschillende containers weer die kunnen worden gebruikt om deze applicatie te ontwikkelen. Let op dat zowel standaard gebruikers als beheerders verbinding maken met dezelfde frontend. Dit komt doordat de applicatie gebruik zal maken van **RBAC** (Role-Based Access Control).

![C2-diagram](./C2-LockBox.png "C2 lockBox")

**Some extra details:**
- Uses **Rust** and **Rocket** in the file service to increase performance of the main functionality: file storage.
- Uses **Express.js**, **Deno** and **TypeScript** for the "*user service*" because it is easy to integrate with most popular authentication/authorisation services.

---
## Misuse-case diagram

Misuse-case diagrammen kunnen worden gebruikt om overzichtelijk te maken op welke manieren een applicatie mogelijk misbruikt kan worden. Dit doet een misuse-case diagram door aan te geven wie potentieel de applicatie zou willen misbruiken. Vervolgens kan beredeneerd worden hoe deze verschillende actoren misbruik zouden kunnen maken, bijvoorbeeld XSS aanvallen op een frontend. Al deze potentiële strategieën kunnen vervolgens gekoppeld worden aan de verschillende services binnen de applicatie. Zo is heel duidelijk zichtbaar welke services gevaar lopen, en voor wat voor aanvallen.

Hieronder is het misuse-case diagram dat is gemaakt voor LockBox zichtbaar.

![misuse-case diagram](./misuse-cases.png "misuse-case diagram")