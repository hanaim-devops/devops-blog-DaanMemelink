# Robuustheid voor Microservices

<img src="plaatjes/litmus_logo.png" width="250" align="right" alt="litmus logo">

*[Daan Memelink, oktober 2024.](https://github.com/hanaim-devops/devops-blog-DaanMemelink)*
<hr/>

## Introductie  
In moderne DevOps-omgevingen en microservices-architecturen is stabiliteit en veerkracht van cruciaal belang. 
Om systemen bestand te maken tegen onverwachte fouten en om robuuste systemen te bouwen, wordt steeds vaker chaos engineering ingezet. 
Deze methodologie helpt teams om niet alleen reactief, maar vooral proactief om te gaan met storingen (Basiri, 2020). 
In deze blogpost duiken we diep in LitmusChaos, een open-source chaos engineering-tool voor Kubernetes, en onderzoeken we hoe deze efficiënt kan worden toegepast om de robuustheid van microservices te verbeteren. 
We bespreken de basis van chaos engineering, de unieke rol van LitmusChaos binnen DevOps, en de mogelijkheden voor toepassing in een microservices-omgeving zoals de PitStop MSA.

<hr/>

## Wat is Chaos Engineering?

Chaos engineering is een methodologie voor het bewust creëren van storingen binnen een systeem om te zien hoe het hiermee omgaat. 
Het doel is om de robuustheid en veerkracht van systemen te verbeteren door potentiële zwakke punten te ontdekken voordat deze in productie leiden tot onverwachte storingen (Basiri, 2020). 
De principes van chaos engineering omvatten het definiëren van ‘normale’ systeemtoestanden, het veroorzaken van verstoringen, het observeren van systeemeffecten en het doorvoeren van verbeteringen (Gremlin, 2023).

**Voorbeeld**:  
Stel dat een microservices-systeem databaseverstoringen moet kunnen weerstaan. 
Met chaos engineering kunnen we een scenario creëren waarin de database tijdelijk niet beschikbaar is. 
Door te observeren hoe het systeem hiermee omgaat, kunnen ontwikkelaars problemen zoals vertragingsgevoeligheid, afhankelijkheidsfouten of foutmeldingen in de gebruikersinterface ontdekken en aanpakken.

## Hoe past LitmusChaos in het DevOps-landschap?

LitmusChaos is specifiek ontwikkeld voor Kubernetes-omgevingen en biedt uitgebreide mogelijkheden om storingen te simuleren, zoals netwerkvertraging, CPU-verbruik en pod-fouten (LitmusChaos, 2022). 
De tool integreert goed met CI/CD pipelines, waardoor chaos-tests eenvoudig kunnen worden toegevoegd aan bestaande DevOps-processen. 
Hierdoor kunnen bedrijven proactief chaos engineering implementeren en tegelijkertijd de robuustheid van hun microservices optimaliseren (Kubernetes, 2023).

**Waarom LitmusChaos?**  
In tegenstelling tot andere tools biedt LitmusChaos een gebruiksvriendelijke GUI en CLI, uitgebreide documentatie en ondersteuning voor Kubernetes-native workflows (LitmusChaos, 2022). 
Dit maakt het aantrekkelijk voor DevOps-teams die al werken met Kubernetes en een tool zoeken die gemakkelijk kan worden geïntegreerd in bestaande systemen.

## Wat zijn de alternatieven voor LitmusChaos?

Hoewel LitmusChaos populair is, zijn er andere chaos engineering-tools beschikbaar, zoals Gremlin en Chaos Monkey. 
Elk van deze tools heeft unieke functies en richt zich op verschillende omgevingen en use-cases:

- **Gremlin**: biedt een vergelijkbare set aan verstoringsexperimenten en ondersteunt naast Kubernetes ook andere infrastructuren (Gremlin, 2023).
- **Chaos Monkey**: ontwikkeld door Netflix, focust op het uitschakelen van services om robuustheid te testen, maar is meer beperkt tot specifieke infrastructuren en biedt minder Kubernetes-georiënteerde verstoringen (Netflix, 2021).

In vergelijking met deze tools heeft LitmusChaos als voordeel dat het Kubernetes-native is, wat het makkelijker maakt om het te gebruiken binnen Kubernetes-omgevingen zonder complexe configuraties (LitmusChaos, 2022).

## Wat zijn de voor- en nadelen van LitmusChaos?

Een goed begrip van de sterke en zwakke punten van LitmusChaos helpt bij de beslissing om deze tool te implementeren. 
Enkele van de belangrijkste voor- en nadelen:

### Voordelen:
- **Naadloze Kubernetes-integratie**: Omdat LitmusChaos Kubernetes-native is, biedt het ingebouwde ondersteuning voor pods, namespaces en clusters (Kubernetes, 2023).
- **Gebruiksvriendelijke interface**: De GUI maakt chaos engineering toegankelijk, zelfs voor teams met beperkte chaos engineering-ervaring (LitmusChaos, 2022).
- **Uitbreidbaarheid**: LitmusChaos biedt een modulaire structuur, waardoor gebruikers eigen experimenten kunnen toevoegen die aansluiten bij hun specifieke behoeften (LitmusChaos, 2022).

### Nadelen:
- **Beperkte multi-cloud ondersteuning**: LitmusChaos werkt het beste in Kubernetes-omgevingen en heeft beperkte integratie met niet-Kubernetes-omgevingen (LitmusChaos, 2022).
- **Leren van de basis**: Chaos engineering kan complex zijn en LitmusChaos vergt een basiskennis van Kubernetes en microservices-architectuur om optimaal te gebruiken (Gremlin, 2023).

## Hoe pas ik LitmusChaos in het klein toe?

In dit hoofdstuk staat hoe LitmusChaos kan worden toegepast in een Kubernetes-omgeving door middel van een POC (Proof Of Concept).

**Stap 1: Installatie van LitmusChaos**  
Om LitmusChaos te installeren via YAML, kun je de volgende stappen volgen:

1. **Maak een Kubernetes namespace aan**:

`kubectl create ns litmus`

2. **Installeer LitmusChaos op de namespace**:
    
`kubectl apply -n litmus -f https://litmuschaos.github.io/litmus/2.0.0/litmus-2.0.0.yaml`

3. **Maak LitmusChaosCenter toegankelijk**:

`kubectl port-forward svc/litmusportal-frontend-service -n litmus 30000:9091 --address 0.0.0.0`

Op `http://localhost:3000` is nu een dashboard beschikbaar. Inloggen kan met de gebruikersnaam `admin` en het wachtwoord `litmus`.

**Stap 2: Creëren van een Chaos Experiment**  

Nu LitmusChaos is geïnstalleerd, kunnen we een basis experiment configureren om een pod in de namespace te verstoren. 
Hiervoor gebruiken we een Predefined Workflow.
Dit is en vooraf gedefinieerde reeks stappen die een specifieke verstoring veroorzaken.

1. Select Workflow:
   1. Klik op "Schedule a Workflow"
   2. Kies "Self Agent" als target 
   3. Kies "potato-head" als Predefined Workflow en klik op "Next"
   
2. Tune Experiment:
   1. Pas de duur van de verstoring aan
   2. Stel in wanneer de verstoring moet plaatsvinden

De workflow is nu aangemaakt en te zien in het dashboard.
![litmusworkflows.png](plaatjes%2Flitmusworkflows.png)

3. Verifiëer de verstoring:
   1. Voer `kubectl get pods -n litmus` uit. Eén van de pods moet de statue 'terminating' hebben. 
    

## Hoe pas ik LitmusChaos toe in de PitStop MSA applicatie?

LitmusChaos kan een waardevolle rol spelen in de robuustheidstests voor de PitStop Microservices Architectuur (MSA), vooral gezien de complexiteit en onderlinge afhankelijkheden binnen een microservice-omgeving.
Een effectieve manier om LitmusChaos toe te passen in PitStop is door chaos-experimenten op te zetten die specifieke services verstoren, zoals vertraging in een API of het uitvallen van een database.
Door deze experimenten kunnen we de fouttolerantie en het herstelvermogen van de applicatie testen en verbeteren.
LitmusChaos biedt een flexibele opzet waarmee je meerdere experimenten kunt definiëren en deze automatisch kunt laten uitvoeren binnen een CI/CD-pijplijn.

Een voorbeeld van een test in de PitStop MSA zou kunnen zijn om een database-service tijdelijk te verstoren en vervolgens te observeren hoe de andere microservices hiermee omgaan.
Met LitmusChaos kun je dergelijke experimenten snel inrichten met YAML-configuratiebestanden die je in een CI/CD-workflow kunt integreren.
Door dit herhaaldelijk te doen tijdens de ontwikkelings- en testfase, kunnen robuustheid en veerkracht van de PitStop MSA continu worden verbeterd.

## Conclusie

LitmusChaos is een krachtige tool voor chaos engineering, vooral voor DevOps-teams die werken met microservices in Kubernetes-omgevingen.
Het biedt een robuuste set aan experimenten waarmee je kunt testen hoe jouw applicaties reageren op onverwachte verstoringen, wat uiteindelijk leidt tot betere stabiliteit en betrouwbaarheid.
Door LitmusChaos te integreren in de CI/CD-pijplijn en experimenten te ontwerpen die specifiek gericht zijn op de behoeften van jouw microservice-architectuur, kun je de veerkracht en stabiliteit van jouw systemen aanzienlijk verbeteren.
In het geval van de PitStop MSA kan LitmusChaos gebruikt worden om mogelijke knelpunten en kwetsbaarheden te identificeren, die anders pas bij een echte storing aan het licht zouden komen.
Met de juiste configuratie en integratie kan LitmusChaos een onmisbaar onderdeel worden van een DevOps-strategie die zich richt op hoge beschikbaarheid en robuustheid.

## Referenties

- Basiri, A., et al. (2016). *Chaos Engineering: System resilience in practice*. Retrieved from [https://netflix.github.io/chaosmonkey/](https://netflix.github.io/chaosmonkey/)
- Gremlin. (2020). *Chaos Engineering: Breaking things on purpose to learn how to make systems resilient*. Retrieved from [https://www.gremlin.com](https://www.gremlin.com)
- LitmusChaos. (2021). *Documentation for LitmusChaos v1*. Retrieved from [https://docs.litmuschaos.io](https://docs.litmuschaos.io)
- LitmusChaos. (2022). *LitmusChaos v2.0: Advanced experiment orchestration*. Retrieved from [https://docs.litmuschaos.io](https://docs.litmuschaos.io)
- Chaos Mesh. (2021). *Open source chaos engineering platform for Kubernetes*. Retrieved from [https://chaos-mesh.org](https://chaos-mesh.org)
