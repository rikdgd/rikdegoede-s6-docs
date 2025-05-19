---
sidebar_position: 3
---
# Change, Risk, Security & Release Management

Er is geen standaard manier om een DevSecOps proces in te richten. Dit komt natuurlijk door het feit dat ieder bedrijf en iedere applicatie anders is. Dit document zal duidelijk proberen te maken hoe het DevSecOps process voor LockBox is ingericht, en wat de voor- en nadelen hiervan zijn. Om dit gestructureerd te doen zal het DevSecOps proces van LockBox worden opgedeeld in change, risk, security en release management.


## Change management
- Use of version control (e.g., Git branches for features, pull requests)
- Code reviews and approval process

Voor het beheren van veranderingen/updates aan de software wordt gebruik gemaakt van version control, hiervoor is de keuze gemaakt om Git te gebruiken aangezien dit veel wordt gebruikt in de industrie. De Git repositories worden op GitHub gehost om samenwerken makkelijker te maken. 

### Branching strategy
feature branches...


## Risk management
- How you identify risks (e.g., insecure packages, bad merges)
- How you mitigate (e.g., automated testing, peer review)


## Security Management
- Secure coding practices
- Static/dynamic code analysis tools
- Secrets management (e.g., using `.env`, avoiding secrets in repos)


## Release management
- Tagged releases
- Deployment environments (staging vs production)
- Rollback strategies (if any)