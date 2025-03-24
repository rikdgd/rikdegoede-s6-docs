---
sidebar_position: 1
---
# Technology Selection

Bij het ontwikkelen van enterprise software is het belangrijk om een goede keuze te maken uit de verschillende technologieën die gebruikt kunnen worden. Er zijn veel verschillende frameworks met ieder zo zijn voor- en nadelen, en deze zullen tegen elkaar afgewogen moeten worden om een goede keuze te maken. 

Om voor het LockBox project een goede keuze te maken zijn een aantal criteria opgesteld die voor het project van belang zijn. Zo kunnen de verschillende frameworks makkelijker met elkaar worden vergeleken. 

## Criteria
De belangrijkste criteria voor het LockBox project zijn hier terug te zien met een korte beschrijving erbij. 

- **Scalability** - De LockBox applicatie zal een groot aantal gebruikers aan moeten kunnen. Hiervoor is het daarom belangrijk dat de gebruikte technologieën in staat zijn mee te schalen met het aantal gebruikers.
- **Active community** - Een active community helpt enorm bij het ontwikkelen van software, er is dan namelijk meer hulp online te vinden. 
- **Documentatie** - Het product moet goed gedocumenteerd zijn. Dit maakt het makkelijker om geavanceerde functionaliteiten in te bouwen, en de technologie efficient te gebruiken. 
- **Actief onderhouden** - De technologie moet actief onderhouden zijn. Software die niet langer actief onderhouden wordt zal vroeg of laat security vulnerabilities bevatten. 
- **Open source** - Het product moet open-source zijn. Het LockBox project zal ook een open-source licentie bevatten, dus moeten de licenties van de gebruikte technologieën dit ook zijn. 
- **Security** - Het product moet een focus hebben op security. Security is een belangrijk aspect voor enterprise software in het algemeen. Voor LockBox is dit niet minder waar aangezien gebruikers geregeld gevoelige bestanden zullen uploaden. 
- **Ontwikkel snelheid** - Er is maar 1 semester de tijd om aan het LockBox project te werken. Deze limitering maakt dat het product een snel ontwikkel proces moet ondersteunen, en niet limiteren. 
- **Ervaring bij developers** - Of er al wel of geen ervaring is bij de ontwikkelaar (Rik de Goede) van het LockBox project. Als er al ervaring is met het product scheelt dit enorm in ontwikkel snelheid.


## Keuze Matrices

### Web frameworks:

| Criteria           | weight | .NET | Spring | Rocket | Express.js | Rails | Flask |
| ------------------ | ------ | ---- | ------ | ------ | ---------- | ----- | ----- |
| Scalability        | 2      | x    | x      | x      | x          | x     | x     |
| Community          | 1      | x    | x      |        | x          | x     | x     |
| Documentatie       | 2      | x    | x      | x      | x          | x     | x     |
| Actief onderhouden | 3      | x    | x      | x      | x          | x     | x     |
| Open source        | 1      | x    | x      | x      | x          | x     | x     |
| Security           | 2      | x    | x      | x      | x          | x     | x     |
| Ontwikkel snelheid | 2      |      |        | x      | x          | x     | x     |
| Ervaring           | 1      | x    | x      | x      | x          |       | x     |
1. **Express.js / Flask:** 14 pts
2. **Rocket / Rails:** 13 pts
3. **.NET / Spring:** 12 pts
---
### Frontend frameworks:

| Criteria           | weight | React.js | Vue.js | Angular | Next.js | Svelte |
| ------------------ | ------ | -------- | ------ | ------- | ------- | ------ |
| Scalability        | 2      |          | x      | x       | x       | x      |
| Community          | 1      | x        | x      | x       | x       | x      |
| Documentatie       | 2      | x        | x      | x       | x       | x      |
| Actief onderhouden | 3      | x        | x      | x       | x       | x      |
| Open source        | 1      | x        | x      | x       | x       | x      |
| Security           | 2      |          | x      | x       | x       | x      |
| Ontwikkel snelheid | 2      | x        | x      |         | x       | x      |
| Ervaring           | 1      | x        |        |         | x       |        |
1. **Next.js:** 14 pts
2. **Vue.js / Svelte:** 13 pts
3. **React.js:** 12 pts
4. **Angular:** 11 pts
---
### Databases:

| Criteria           | weight | MongoDB | PostgreSQL | MySQL | Redis |
| ------------------ | ------ | ------- | ---------- | ----- | ----- |
| Scalability        | 2      | x       | x          | x     | x     |
| Community          | 1      | x       | x          | x     | x     |
| Documentatie       | 2      | x       | x          | x     | x     |
| Actief onderhouden | 3      | x       | x          | x     | x     |
| Open source        | 1      |         | x          | x     |       |
| Security           | 2      | x       | x          | x     | x     |
| Ontwikkel snelheid | 2      | x       |            |       | x     |
| Ervaring           | 1      | x       |            | x     |       |
1. **MongoDB:** 13 pts
2. **MySQL / Redis:** 12 pts
3. **PostgreSQL:** 11 pts

*Note: MySQL heeft enkel een goede scalability bij gebruik van de enterprise edition vanwege de "thread pooling" functionaliteit.*

---
## Conclusie
De keuze matrices geven een duidelijk overzicht van de verschillende producten, en hoe goed ze voldoen aan de gestelde criteria. Hier zal kort worden toegelicht welke producten gebruikt gaan worden, zowel als waarom. 

**Web frameworks** <br/>
Bij het onderdeel web frameworks waren er 2 producten die aan alle criteria voldoen, namelijk: *Express.js* en *Flask*. Hierbij is de keuze gemaakt om Express.js te gaan gebruiken. De grootste reden hiervoor is het feit dat dezelfde taal (TypeScript) gebruikt kan worden voor zowel de backend als de frontend.

De LockBox applicatie zal echter niet enkel gebruik maken van Express.js, maar ook van Rocket. Dit framework heeft minder community support aangezien het relatief nieuw is. Toch zal LockBox Rocket gebruiken voor de services waarbij performance van extra belang is zoals de "*file storage service*".

**Frontend frameworks** <br/>
Voor de keuze van Frontend frameworks was *Next.js* de winnaar. Het framework wint echter enkel van *Vue.js* en *Svelte* door het feit dat er al ervaring is met Next.js.

Wel moet opgemerkt worden dat voor een eerste proof of concept mogelijk React.js gebruikt gaat worden. Dit omdat dit development tijd bespaard, en security en scalability niet belangrijk zijn voor een proof of concept. 

**Databases** <br/>
*MongoDB* is de winnaar bij de database vergelijking. Het enige nadeel aan MongoDB is het feit dat de software niet open source is. 

Op de tweede plaats komen *MySQL* en *Redis*. MySQL heeft als enige nadeel de ontwikkel snelheid. Dit komt door het feit SQL databases vaak meer aandacht nodig hebben dan NoSQL databases. <br/>
Redis had kunnen winnen als zij hun software open source hadden gelaten. Redis heeft toch nog een ander nadeel, het is namelijk standaard enkel een in-memory database. Om Redis persistent te maken is ook extra setup nodig. 
