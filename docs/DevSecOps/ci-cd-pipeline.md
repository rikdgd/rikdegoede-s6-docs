---
sidebar_position: 4
---
# CI/CD Pipeline

introduction

## Overview
(Describe the pipeline in prose and optionally as a diagram)


## Pipeline stages
- **Build**: Tools used (e.g., Webpack, Maven, etc.)
- **Code Analysis / Review**: Linters, SonarQube, ESLint, etc.
- **Testing**: Unit tests, integration tests, test coverage tools
- **Deployment**: Automated to dev/staging/prod? Manual approval gate?

### Deployment
De verschillende microservices worden ge- deployed in de vorm van een Docker image. Dit is nodig om ze in Kubernetes te gebruiken, en het maakt de applicatie makkelijk vervangbaar. 

Aangezien de docker images automatisch van een image repository gehaald kunnen worden, is de beste om de losse services te deploy-en via zo'n repository. LockBox maakt hiervoor gebruik van **Docker hub** aangezien dit makkelijk te gebruiken is tijdens development. 

Om de images van de verschillende service to bouwen en deployen wordt een GitHub actions workflow gebruikt. De `YAML` configuratie van deze workflow/pipeline ziet er als volgt uit:
```yaml
name: Build and push Docker image  
  
on:  
  push:  
    branches: [ "main" ]  
  pull_request:  
    branches: [ "main" ]  
  
jobs:  
  build-and-push:  
    runs-on: ubuntu-latest  
  
    steps:  
    - name: Login to Docker  
      uses: docker/login-action@v3  
      with:  
        username: ${{ vars.DOCKERHUB_USERNAME }}  
        password: ${{ secrets.DOCKERHUB_TOKEN }}  
          
    - uses: actions/checkout@v4  
    - name: Build the Docker image  
      run: docker build . --file lockbox-notification-service/Dockerfile --tag rikdegoede/lockbox-notification-service:latest
    
    - name: Push to Dockerhub  
      run: docker push rikdegoede/lockbox-notification-service:latest
```


## Tools & Services
- GitHub Actions, GitLab CI, Jenkins, etc.
- Docker, Kubernetes (if any)
- Any third-party tools for scanning/security/testing


## Execution results
Resultaten van een uitgevoerde run laten zien als screenshot met korte beschrijving.