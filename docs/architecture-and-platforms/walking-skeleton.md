---
sidebar_position: 2
---
# Walking Skeleton

Er zijn in het design van de LockBox applicatie een aantal keuzes gemaakt wat betreft de gebruikte technologieën. Om te controleren of de bedachte tech-stack daadwerkelijk goed uitpakt is een *"walking skeleton"* opgezet. 

Een walking skeleton is een minimalistische implementatie van de architectuur. Dit wil zeggen dat een walking skeleton **geen** functionaliteit zal bevatten. Het doel van een walking skeleton is om te controleren dat het gemaakte design uitvoerbaar is. 

## Componenten
De componenten die zijn toegevoegd aan de walking skeleton zijn in de tabel zichtbaar. De componenten zijn allemaal zo minimalistisch mogelijk opgezet om complexiteit te voorkomen. 

| Component           | Gebruikte technologieën                                 | Beschrijving                                                                                                                                                                                                      |
| ------------------- | ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Frontend            | - Node.js<br />- React.js<br />- Vite<br />- TypeScript | Get ingangspunt voor gebruikers is de frontend. De frontend in de walking skeleton zal enkel een simpel form bevatten om te testen of het mogelijk is om verbinding te maken met de API Gateway.                  |
| API Gateway         | - Spring<br />- Kotlin                                  | De API Gateway is verantwoordelijk voor het doorsturen van requests die vanuit de frontend binnenkomen.                                                                                                           |
| message bus         | - RabbitMQ                                              | *RabbitMQ* is de open-source message broker die LockBox zal gebruiken voor het versturen van messages tussen services.                                                                                            |
| NotificationService | - Deno<br />- amqplib<br />- TypeScript                 | De NotificationService zal momenteel enkel een hard-coded *notificatie* versturen.                                                                                                                                |
| PostService         | - Rust<br />- Lapin                                     | De PostService en *"posts"* zullen niet worden uitgewerkt in de werkelijke applicatie, maar functioneren als een simpel voorbeeld functionaliteit.<br />De PostService zal enkel een hard-coded *post* versturen. |
