---
sidebar_position: 4
---
# OWASP - SAMM
SAMM (Software Assurance Maturity Model) is een framework dat kan helpen bij het ontwikkelen van veilige software, of het beveiligen van al bestaande software. Het framework is een project van OWASP, een gemeenschap die voornamelijk bekend staat om de OWASP top 10. 

Het doel van het framework is zoals ze zelf vermelden:
*"Our mission is to provide an effective and measurable way for all types of organizations to analyze and improve their software security posture."* ([OWASP SAMM](https://owaspsamm.org/))

Het framework zal dus niet magisch alle security problemen oplossen, maar eerder een goed inzicht geven over de huidige staat van een applicatie op het gebied van security. Hierbij worden ook aspecten zoals de voorlichting van de medewerkers meegenomen. Het framework is opgezet om meetbaar te zijn zodat er geen discussie kan ontstaan of een applicatie wel of niet voldoet aan bepaalde voorwaarden. 

Het SAMM framework bestaat op het hoogste niveau uit 5 categorieën: governance, design, implementation, verification en operations. Deze categorieën worden ook wel *"business functions"* genoemd. De business functions bevatten ieder op zich weer drie *"security practices"*. Dit zijn gebieden met security gerelateerde activiteiten die kunnen worden gebruikt om zekerheid te geven over de security binnen die business function. 

Tot slot hebben de security practices activiteiten die kunnen worden uitgevoerd om de security te verbeteren binnen het gebied van de security practice. Deze activiteiten zijn ook opgesplitst over twee zogeheten ‘streams’. Het doel van deze streams is om verschillende aspecten binnen een security practice tot de aandacht te brengen. Er zijn ook altijd aanraders aanwezig voor de verschillende maturity levels waarop een organisatie zich mogelijk kan bevinden. 


## Invulling voor LockBox
Hier beneden zal het SAMM model worden doorlopen voor maturity level 1. Dit is het laagste maturity level, maar kan nog steeds goede aanraders opleveren voor de stappen die nog doorlopen kunnen worden. Aanraders waaraan het LockBox project niet voldoet zullen worden ~~doorgestreept~~.

Merk op dat dit framework is ingevuld in week 7 van het project, en sommige onderdelen mogelijk hier nog staan aangegeven als onvoldoende, terwijl dit mogelijk later in het semester naar voldoende is gebracht.

Ook is het belangrijk om te vermelden dat dit framework zich veel richt op bedrijven en bedrijfsprocessen. Deze onderdelen zijn daarom voor mijn individuele project niet goed in te vullen. 

### Governance
#### Strategy en metric:
*Identify organization drivers as they relate to the organization's risk tolerance.* <br/>
Lastig in te vullen voor een school project, maar bedrijven die secure file storage leveren zullen waarschijnlijk het vertrouwen van de klant serieus nemen. 
De applicatie zal, wanneer deze in Europa wordt uitgerold, ook moeten voldoen aan GDPR wetgeving. 

~~*Define metrics with insight into the effectiveness and efficiency of the Application Security Program.*~~

#### Policy and compliance
~~*Determine a security baseline representing organization's policies and standards.*~~

~~*Identify 3rd-party compliance drivers and requirements and map to existing policies and standards.*~~

#### Education and Guidance
*Provide security awareness training for all personnel involved in software development.* <br/>
Er is geen bedrijf aanwezig voor LockBox, maar aangezien ik de student er aan werk, kan toch wel worden ingevuld dat ik awareness training krijg, aangezien ik twee semesters cyber security heb gevolgd.

~~*Identify a "Security Champion" within each development team.*~~

### Design
#### Threat assessment
~~*A basic assessment of the application risk is performed to understand likelihood and impact of an attack.*~~

*Perform best-effort, risk-based threat modeling using brainstorming and existing diagrams with simple threat checklists.* <br/>
Er is een misuse-case diagram opgezet om een simpele visualisatie te hebben van de verschillende aanvallen die mogelijk gebruikt kunnen worden per functionaliteit van de applicatie. Hierbij is gebruik gemaakt van de OWASP top 10 aangezien LockBox een web-applicatie is.

#### Security Requirements
*High-level application security objectives are mapped to functional requirements.* <br/>
Er zijn non-functional requirements opgezet om bepaalde security aspecten te garanderen. Een paar voorbeelden zijn:
- Bestanden worden ge-encrypt met AES-GCM.
- De app maakt gebruik van JSON Web Tokens (JWT) voor authenticatie.
- De applicatie maakt gebruik van HTTPS (TLS)
Deze moeten uiteraard wel bijgewerkt blijven worden gedurende het ontwikkelproces. 


~~*Evaluate the supplier based on organizational security requirements.*~~ <br/>
Er zal geen outsourcing plaatsvinden gedurende het schoolproject. Libraries en frameworks vallen ook niet binnen dit onderwerp.


#### Secure architecture
*Teams are trained on the use of basic security principles during design.* <br/>
Er zijn uiteraard geen teams in een eenmansproject. Dit punt ga ik in deze uitwerking toch afvinken aangezien ik zelf momenteel secure development volg, en dus wel op de hoogte ben van basis security principes.

*Elicit technologies, frameworks and integrations within the overall solution to identify risk.* 


### Implementation
#### Secure build
~~*Create a formal definition of the build process so that it becomes consistent and repeatable.*~~ <br/>
Er wordt gebruikgemaakt van Docker voor deployment, wat deels het build proces automatiseert. Alleen is dit uiteraard lang niet genoeg om dit punt af te vinken.

~~*Create records with Bill of Materials of your applications and opportunistically analyze these.*~~ <br/>
Er is geen SBOM (Software Bill of Material) bijgehouden voor LockBox op het moment. 

Een SBOM kan als volgt worden omschreven:
‘formal record containing the details and supply chain relationships of various components used in building software’ (Improving the Nation’s Cybersecurity, 2021)


#### Secure Deployment
~~*Formalize the deployment process and secure the used tooling and processes.*~~

*Introduce basic protection measures to limit access to your production secrets.* <br/>
Voor het gebruik van secrets worden in de pipelines worden GitHub secrets gebruikt. Verder bevatten de applicaties geen hardcoded secrets, hiervoor worden environment variables gebruikt. <br/>
De keys die worden gebruikt voor de repositories verlopen automatisch. <br/>
De repositories worden automatisch gescand op hardcoded secrets, en push requests die secrets bevatten worden automatisch afgewezen. <br/>
De applicatie maakt gebruik van RBAC.

#### Defect management
*Introduce a structured tracking of security defects and make knowledgeable decisions based on this information.* <br/>
Er is automatische vulnerability scanning opgezet in de CI/CD pipelines van LockBox. De gevonden vulnerabilities worden automatisch aan CVE nummers gekoppeld en als SARIF data bijgehouden per repository.

~~*Regularly go over previously recorded security defects and derive quick wins from basic metrics.*~~ <br/>

### Verification
#### Architecture Assesment
~~*Identify application and infrastructure architecture components and review for basic security provisioning.*~~

~~*Ad-hoc review of the architecture for unmitigated security threats.*~~

#### Requirements-driven Testing
~~*Test for software security controls.*~~ <br/>
Er wordt zoals eerder genoemd al gebruik gemaakt van automatische security tests met Snyk. Dit controleert een deel van de security controls. <br/>
Echter om dit onderwerp beter aan te pakken in het LockBox project zou het slim zijn om ook automatische DAST te implementeren, en code reviews te houden (met medestudenten). 

~~*Perform security fuzzing testing.*~~ 


#### Security testing
*Utilize automated security testing tools.* <br/>
Zoals eerder genoemd wordt Snyk gebruikt voor het automatisch uitvoeren van security tests, zowel als het automatisch verzamelen van de test resultaten.

~~*Perform manual security testing of high-risk components.*~~


### Operations
#### Incident Management
~~*Use available log data to perform best-effort detection of possible security incidents.*~~

~~*Identify roles and responsibilities for incident response.*~~

#### Environment Management
~~*Perform best-effort hardening of configurations, based on readily available information.*~~ <br/>
De ‘file management’ service van de applicatie is al deels ge-hardened. Dit moet nu nog grondiger worden gedaan, en voor de rest van de services.

*Perform best-effort patching of system and application components.* <br/>
Er is nog maar een security vulnerability gevonden, vanwege het feit dat de applicatie nog minimalistisch is. Deze vulnerability kwam voort uit een outdated versie van een dependency en is opgelost.

#### Operational Management
~~*Implement basic data protection practices.*~~

~~*Decommission unused applications and services as identified. Manage customer upgrades/migrations individually.*~~


## Samenvatting
Zoals zichtbaar zijn er voor mijn persoonlijke project een goed aantal punten nog niet voldoende gedekt. Dit is niet heel verbazingwekkend, beseffende dat dit een school project is en het product überhaupt nog niet volledig functioneel is. 

Het invullen van dit framework, alleen al op het laagste maturity level, heeft wel een goed overzicht opgeleverd over de verschillende onderdelen die nog behandeld kunnen worden om de beveiliging van mijn software beter te kunnen controleren/garanderen.