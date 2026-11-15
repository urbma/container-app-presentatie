---
theme: seriph
background: /title-bg.svg
title: Van AKS naar ACA
colorSchema: dark
info: |
  ## Van AKS naar ACA
  Ons migratieproject.
class: text-center
drawings:
  persist: false
transition: fade
comark: true
duration: 35min
---

# Van AKS naar ACA

Ons migratieproject

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Druk op spatie voor de volgende slide <carbon:arrow-right />
</div>

---
transition: fade
---

# Wat zijn Azure Container Apps?

Een **serverless containerplatform** van Azure om containers te draaien zonder zelf een cluster te beheren.

<img src="/aca-schip.svg" class="mx-auto mt-4 h-72 rounded-xl shadow-xl" />

<!--
Kernboodschap: Container Apps zit tussen App Service en AKS in. Je krijgt de flexibiliteit van containers, zonder de operationele last van een Kubernetes-cluster.
-->

---

# Wat krijg je?

- Geen Kubernetes-kennis nodig

<img src="/krijg-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Geen Kubernetes-kennis nodig: Je draait containers zonder je zorgen te maken over nodes, upgrades of manifests.
-->

---

# Wat krijg je?

- <span class="opacity-50">Geen Kubernetes-kennis nodig</span>
- Automatisch schalen, ook naar nul

<img src="/krijg-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Automatisch schalen, ook naar nul: Schaal op HTTP-verkeer, queues of events. Geen verkeer? Dan draait er niets. Opschalen gaat in seconden, ook voor GPU-workloads.
-->

---

# Wat krijg je?

- <span class="opacity-50">Geen Kubernetes-kennis nodig</span>
- <span class="opacity-50">Automatisch schalen, ook naar nul</span>
- Ingress, HTTPS en revisions

<img src="/krijg-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Ingress, HTTPS en revisions: Ingebouwd, zonder ingress controller of certificaatbeheer. Revisions maken gefaseerde uitrol en rollback eenvoudig.
-->

---

# Wat krijg je?

- <span class="opacity-50">Geen Kubernetes-kennis nodig</span>
- <span class="opacity-50">Automatisch schalen, ook naar nul</span>
- <span class="opacity-50">Ingress, HTTPS en revisions</span>
- Betalen per gebruik

<img src="/krijg-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Betalen per gebruik: Je betaalt voor CPU en geheugen per seconde dat je app echt draait. Idle kost niets en het Consumption-plan heeft een gratis maandelijks tegoed.
-->

---

# Wat krijg je?

- <span class="opacity-50">Geen Kubernetes-kennis nodig</span>
- <span class="opacity-50">Automatisch schalen, ook naar nul</span>
- <span class="opacity-50">Ingress, HTTPS en revisions</span>
- <span class="opacity-50">Betalen per gebruik</span>
- Beveiligd en betrouwbaar

<img src="/krijg-5.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Beveiligd en betrouwbaar: Managed identities, netwerkisolatie en toegangsbeheer zijn ingebouwd. Microsoft geeft een SLA van 99,95% (volgens de productpagina; controleer de actuele SLA voor onze configuratie).
-->

---

# Wat krijg je?

- <span class="opacity-50">Geen Kubernetes-kennis nodig</span>
- <span class="opacity-50">Automatisch schalen, ook naar nul</span>
- <span class="opacity-50">Ingress, HTTPS en revisions</span>
- <span class="opacity-50">Betalen per gebruik</span>
- <span class="opacity-50">Beveiligd en betrouwbaar</span>
- Netwerkintegratie (VNet, private endpoints)

<img src="/krijg-6.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Netwerkintegratie: Container Apps draaien in je eigen VNet, met private endpoints naar databases, storage en andere PaaS-diensten. Handig, maar precies de plek waar wij het meeste uit moesten zoeken (zie later).
-->

---

# Wat krijg je?

- <span class="opacity-50">Geen Kubernetes-kennis nodig</span>
- <span class="opacity-50">Automatisch schalen, ook naar nul</span>
- <span class="opacity-50">Ingress, HTTPS en revisions</span>
- <span class="opacity-50">Betalen per gebruik</span>
- <span class="opacity-50">Beveiligd en betrouwbaar</span>
- <span class="opacity-50">Netwerkintegratie (VNet, private endpoints)</span>
- Standaard containers, geen lock-in

<img src="/krijg-7.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Standaard containers, geen lock-in: Deploy vanuit elke container registry (Docker Hub, Azure Container Registry, eigen registry). Open source-bouwblokken (Dapr, KEDA, Envoy) en gewone containers houden de deur open naar andere platformen.
-->

---

# Onder de motorkap

- Draait op Kubernetes

<img src="/motorkap-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Draait op Kubernetes: Maar die laag is volledig voor je verborgen.
-->

---

# Onder de motorkap

- <span class="opacity-50">Draait op Kubernetes</span>
- KEDA, Dapr en Envoy

<img src="/motorkap-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
KEDA, Dapr en Envoy: Event-driven autoscaling, microservice-bouwblokken en ingress/traffic splitting zijn al ingebouwd.

- KEDA (Kubernetes Event-driven Autoscaling): schaalt je app op basis van events, zoals HTTP-verkeer, de lengte van een queue (Service Bus, Kafka) of een cron-schema. Kan terug naar nul instances en weer op.
- Dapr (Distributed Application Runtime): een sidecar met kant-en-klare bouwblokken voor microservices: service-to-service calls, pub/sub, state store, secrets en bindings. Je app praat via HTTP/gRPC met de sidecar, en die regelt de rest, ongeacht de onderliggende techniek. Optioneel in Container Apps.
- Envoy: een proxy die het inkomende verkeer afhandelt: ingress, HTTPS/TLS-terminatie, load balancing en traffic splitting tussen revisions (bijv. 90/10 uitrol).

Allemaal open source en door Azure beheerd, dus wij hoeven ze niet zelf te installeren of te upgraden.
-->

---

# Onder de motorkap

- <span class="opacity-50">Draait op Kubernetes</span>
- <span class="opacity-50">KEDA, Dapr en Envoy</span>
- Container Apps Environment

<img src="/motorkap-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Container Apps Environment: Apps leven samen in een environment: een gedeeld netwerk- en logboundary.
-->

---

# Onder de motorkap

- <span class="opacity-50">Draait op Kubernetes</span>
- <span class="opacity-50">KEDA, Dapr en Envoy</span>
- <span class="opacity-50">Container Apps Environment</span>
- Waarvoor geschikt?

<img src="/motorkap-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Waarvoor geschikt?: API's, microservices, jobs en achtergrondverwerking.
-->

---
layout: center
class: text-center
---

# Waarom weg van AKS?

<img src="/aks-fire.svg" class="mx-auto mt-8 h-72 rounded-xl shadow-xl" />

<!--
Titelslide: AKS was stabiel, dat is niet het probleem. Maar de kosten (nodes 24/7) en het onderhoud, vooral de zware cluster upgrades en migraties, zijn een berg werk voor drie workloads.
-->

---

# Waarom weg van AKS?

- Zelf het cluster onderhouden

<img src="/aks-onderhoud.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Zelf het cluster onderhouden: Kubernetes-versie upgrades, node image updates en patches zijn onze verantwoordelijkheid.
-->

---

# Waarom weg van AKS?

- <span class="opacity-50">Zelf het cluster onderhouden</span>
- Kosten voor 2 microservices en 1 app

<img src="/aks-kosten.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Kosten voor 2 microservices en 1 app: Nodes draaien 24/7, ook als er niets gebeurt. Voor drie workloads betalen we voor veel capaciteit die we niet gebruiken.
-->

---
dragPos:
  square: 691,32,167,_,-16
---

# Waarom weg van AKS?

- <span class="opacity-50">Zelf het cluster onderhouden</span>
- <span class="opacity-50">Kosten voor 2 microservices en 1 app</span>
- Veel zelf inrichten

<img src="/aks-inrichten.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Veel zelf inrichten: Ingress controller, certificaten, monitoring en autoscaling moeten we zelf installeren en beheren.
-->

---

# Waarom weg van AKS?

- <span class="opacity-50">Zelf het cluster onderhouden</span>
- <span class="opacity-50">Kosten voor 2 microservices en 1 app</span>
- <span class="opacity-50">Veel zelf inrichten</span>
- Steile leercurve

<img src="/leercurve.svg" class="absolute right-10 bottom-10 w-96 rounded-xl shadow-xl" />

<!--
Steile leercurve: YAML, Helm, networking en RBAC: veel Kubernetes-kennis voor een klein project.
-->

---

# Waarom weg van AKS?

- <span class="opacity-50">Zelf het cluster onderhouden</span>
- <span class="opacity-50">Kosten voor 2 microservices en 1 app</span>
- <span class="opacity-50">Veel zelf inrichten</span>
- <span class="opacity-50">Steile leercurve</span>
- Kennis bij weinig mensen

<img src="/aks-buszijn.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Kennis bij weinig mensen: Zonder infra team hangt het cluster af van een of twee mensen (bus factor).
-->

---

# Waarom weg van AKS?

- <span class="opacity-50">Zelf het cluster onderhouden</span>
- <span class="opacity-50">Kosten voor 2 microservices en 1 app</span>
- <span class="opacity-50">Veel zelf inrichten</span>
- <span class="opacity-50">Steile leercurve</span>
- <span class="opacity-50">Kennis bij weinig mensen</span>
- Incidenten en security zijn van ons

<img src="/aks-incidenten.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Incidenten en security zijn van ons: Problemen met nodes, netwerk of kwetsbaarheden lossen wij zelf op, zonder specialistisch team.
-->

---
layout: center
class: text-center
---

# De migratie

<img src="/migratie.svg" class="mx-auto mt-8 h-72 rounded-xl shadow-xl" />

<!--
Titelslide: De verhuizing van AKS naar Container Apps ging niet pijnloos. Vooral het netwerk (VNet, subnets, NSG's, DNS, private endpoints) kostte veel uitzoekwerk.
-->

---

# Een container app aanmaken

<div class="w-[85%]">

- Eén commando en je app draait. Wat kan er misgaan? 😇

</div>

<img src="/mig-makkelijk.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Eén commando: Een container app aanmaken is in het begin heel makkelijk. Voor een proof of concept ben je in minuten klaar.
-->

---

# Een container app aanmaken

<div class="w-[85%]">

- <span class="opacity-50">Eén commando en je app draait. Wat kan er misgaan? 😇</span>
- Publieke URL en HTTPS krijg je cadeau

</div>

<img src="/mig-https.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Publieke URL en HTTPS: Out of the box krijg je een publieke URL met certificaat. Prima voor een demo, niet voor ons.
-->

---

# Een container app aanmaken

<div class="w-[85%]">

- <span class="opacity-50">Eén commando en je app draait. Wat kan er misgaan? 😇</span>
- <span class="opacity-50">Publieke URL en HTTPS krijg je cadeau</span>
- Dan komt de eis: alleen via het VNet 🔒

</div>

<img src="/mig-eis.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
De eis: Bij ons mag een container app niet publiek bereikbaar zijn, alleen via een VNet. En daar begon het gedoe.
-->

---

# Alleen via het VNet

<div class="w-[52%]">

- Geen publiek endpoint

</div>

<img src="/aca-netwerk.svg" class="absolute right-8 bottom-8 w-[26rem] rounded-xl" />

<!--
Geen publiek endpoint: Security-eis. Alles moet via het BMW-netwerk lopen.
-->

---

# Alleen via het VNet

<div class="w-[52%]">

- <span class="opacity-50">Geen publiek endpoint</span>
- Dus: een Container App Environment in een VNet

</div>

<img src="/aca-netwerk.svg" class="absolute right-8 bottom-8 w-[26rem] rounded-xl" />

<!--
Container App Environment: De container apps leven in een environment, en die moet in een eigen VNet-subnet hangen (VNet-integratie).
-->

---

# Alleen via het VNet

<div class="w-[52%]">

- <span class="opacity-50">Geen publiek endpoint</span>
- <span class="opacity-50">Dus: een Container App Environment in een VNet</span>
- Dus: subnets met eisen (grootte, delegation)

</div>

<img src="/aca-netwerk.svg" class="absolute right-8 bottom-8 w-[26rem] rounded-xl" />

<!--
Subnets: Dat subnet moet voldoen aan eisen: voldoende groot (bij ons /27), gedelegeerd aan Microsoft.App/environments en met een NSG en route table.
-->

---

# Alleen via het VNet

<div class="w-[52%]">

- <span class="opacity-50">Geen publiek endpoint</span>
- <span class="opacity-50">Dus: een Container App Environment in een VNet</span>
- <span class="opacity-50">Dus: subnets met eisen (grootte, delegation)</span>
- En: private endpoints en private DNS

</div>

<img src="/aca-netwerk.svg" class="absolute right-8 bottom-8 w-[26rem] rounded-xl" />

<!--
Private endpoints en DNS: Alle diensten eromheen (Key Vault, Service Bus, Storage, registry) moeten ook privé bereikbaar zijn, en dat vraagt private endpoints en private DNS zones. Wat is dat allemaal? De volgende slides leggen de termen uit.
-->

---

# Azure-woordenboek 1: VNet en subnet

<div class="w-[52%] text-base">

- **VNet**: je eigen privé-LAN binnen Azure, het kantoornetwerk maar dan virtueel
- **Subnet**: een afgeschermd segment van dat LAN, zoals de serverruimte, met een eigen IP-bereik, bijv. `10.15.60.224/27`
- **Delegation**: subnet exclusief voor één dienst, bij ons `Microsoft.App/environments`
- **Bij ons**: "Create VNet" in de Azure Portal? Vergeet het. Dat gaat via het BMW Manage Portal 🙃

</div>

<img src="/term-vnet.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
VNet en subnet: Een VNet is een eigen privé-netwerk. Subnets verdelen dat netwerk. Het VNet maken we aan in het BMW Manage Portal (regio Frankfurt, grootte /25 wordt automatisch goedgekeurd) en het wordt gepeerd met de GEN2 firewall hub. Daarna verwijderen we de default subnet en NSG en maakt Terraform onze eigen subnets aan: private-endpoints, database, service-bus, container-apps, storage en web-app.
-->

---

# Hub en spoke: ons VNet is geen eiland

<div class="w-[52%] text-base">

- Ons VNet is een **spoke**: gepeerd met een centrale **firewall-hub** van BMW

</div>

<img src="/hub-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Uitleg begrippen: Hub-spoke is een manier om netwerken in Azure op te zetten. De hub is een centraal VNet met gedeelde voorzieningen: bij BMW de firewall, de VPN-verbinding naar het bedrijfsnetwerk en de toegang tot het internet. Een spoke is het VNet van een team of applicatie, zoals het onze, met alleen de eigen resources. Peering is de koppeling tussen twee VNets: ze zijn privé met elkaar verbonden, zonder het internet. Voordeel: beveiliging en verbindingen staan op één plek en elke spoke heeft geen eigen firewall of VPN nodig. Vergelijking: de hub is het hoofdkantoor met poort en receptie, de spokes zijn de afdelingen.

Hub en spoke: Ons VNet is een spoke en gepeerd met de nieuwe firewall-hub van BMW (Gen2). De oude hub had proxies zonder authenticatie en shared services stonden niet in de hub, daarom is er een nieuwe firewall-hub gebouwd. Je controleert de peering met az network vnet peering list.
-->

---

# Hub en spoke: ons VNet is geen eiland

<div class="w-[52%] text-base">

- <span class="opacity-50">Ons VNet is een **spoke**: gepeerd met een centrale **firewall-hub** van BMW</span>
- Elk subnet heeft een route table: `0.0.0.0/0` gaat naar de hub-firewall. Alles naar buiten komt langs de portier 🛂

</div>

<img src="/hub-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Route table: Aan alle subnets hangt een firewall route table met één default user-defined route: 0.0.0.0/0 naar de hub-firewall. Al het verkeer uit het VNet gaat dus eerst langs de firewall.
-->

---

# Hub en spoke: ons VNet is geen eiland

<div class="w-[52%] text-base">

- <span class="opacity-50">Ons VNet is een **spoke**: gepeerd met een centrale **firewall-hub** van BMW</span>
- <span class="opacity-50">Elk subnet heeft een route table: `0.0.0.0/0` gaat naar de hub-firewall. Alles naar buiten komt langs de portier 🛂</span>
- **Azure-diensten** (service tags van de regio's) staan standaard open

</div>

<img src="/hub-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Standaard open: In de hub-firewall staan de service tags van de Azure-regio's waar BMW hubs aanbiedt (en de geo-redundante failover-regio's) standaard op de whitelist, plus een lijst met standaard toegestane bestemmingen.
-->

---

# Hub en spoke: ons VNet is geen eiland

<div class="w-[52%] text-base">

- <span class="opacity-50">Ons VNet is een **spoke**: gepeerd met een centrale **firewall-hub** van BMW</span>
- <span class="opacity-50">Elk subnet heeft een route table: `0.0.0.0/0` gaat naar de hub-firewall. Alles naar buiten komt langs de portier 🛂</span>
- <span class="opacity-50">**Azure-diensten** (service tags van de regio's) staan standaard open</span>
- De rest is dicht. Extra URL's aanvragen via het Manage Portal, goedkeuren in Tufin door vier ogen, en dan ±15 minuten wachten ⏳

</div>

<img src="/hub-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Whitelisting: Extra bestemmingen vraag je aan in het Manage Portal (Network, Add URL Whitelisting). Alleen URL's, geen IP's of IP-ranges. Standaard poort 80 en 443. Requester en approver mogen niet dezelfde persoon zijn (vier-ogenprincipe), de goedkeuring gebeurt in Tufin en de status wordt maar elk kwartier bijgewerkt. Wat de firewall blokkeert kun je opvragen met een URL-blocking report. Bron: BMW developer docs, Gen2-Hub concept.
-->

---

# Azure-woordenboek 2: de NSG

<div class="w-[52%] text-base">

- **NSG**: de uitsmijker van je subnet. Een lijst met regels: wie mag erin en wie mag eruit
- **Per regel instellen**: richting, bron, bestemming, poort, protocol, allow of deny
- **Prioriteit**: laagste nummer wint. Zonder match gelden de standaardregels
- Filtert op IP en poort, niet op URL. Voor URL's heb je de firewall in de hub
- **Bij ons**: een NSG per subnet via een BMW-Terraformmodule. Container apps: inkomend 80, 443 en 8080, uitgaand alleen het nodige

</div>

<img src="/term-nsg.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
NSG (Network Security Group): Een lijst met firewallregels die je aan een subnet of een netwerkkaart hangt. Elke regel heeft een prioriteit (laagste nummer wint), richting (inbound of outbound), bron, bestemming, poort, protocol en actie (allow of deny). Bron en bestemming mogen IP-ranges zijn, maar ook service tags (zoals Internet, VirtualNetwork of AzureLoadBalancer) of application security groups. Azure voegt standaardregels toe: verkeer binnen het VNet is toegestaan, de Azure load balancer mag, inkomend vanaf het internet is dicht, en uitgaand is open. Een NSG is stateful: het antwoord op een toegestaan verzoek mag automatisch terug. Een NSG filtert alleen op laag 3 en 4 (IP en poort), niet op URL. Bij ons maakt Terraform (network-security.tf, BMW-NSG-module) een NSG per subnet: private-endpoints, database, service-bus, container-apps, storage en web-app. Het container-apps-subnet heeft inkomend 80, 443 en 8080 (prioriteit 190). Uitgaand staat open voor: Azure Monitor (443), HTTP/HTTPS (80, 443), NTP (UDP 123) en de Container Apps control plane (5671 en 5672). Twee regels heten nog AllowAKSControlPlane (UDP 1194, TCP 9000): een overblijfsel uit de AKS-tijd.
-->

---

# Azure-woordenboek 3: route table en Tufin

<div class="w-[52%] text-base">

- **Route table** (UDR): het verkeersbord. Alles naar buiten (`0.0.0.0/0`) moet eerst langs de firewall
- **Tufin**: het loket voor firewallregels. Reken op wachttijd ⏳

</div>

<img src="/term-nsg.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Route table en Tufin: Uitgaand verkeer gaat via een route table naar de hub-firewall. Wil je naar buiten (bijv. een externe URL), dan moet er een firewallregel via Tufin komen.
-->

---

# Azure-woordenboek 4: private endpoint

<div class="w-[52%] text-base">

- **Private endpoint**: een eigen achterdeur naar een Azure-dienst, met een **privé-IP** in jouw subnet
- Key Vault, Service Bus, Storage en Registry gaan van het internet af. Alleen jij hebt de sleutel 🔑
- **Private Link**: de techniek erachter. Ook Front Door gebruikt hem om bij onze environment te komen
- **Bij ons**: elk endpoint krijgt pas een naam als je in het BMW Manage Portal op "Create Missing" klikt

</div>

<img src="/term-pe.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Private endpoint: Een deur naar een PaaS-dienst die alleen binnen het VNet bestaat. De dienst zelf zet je dicht voor het publieke internet. In het BMW Manage Portal onder Manage Private Endpoint DNS Records moet je per endpoint de DNS-records laten aanmaken (Create of Create Missing).
-->

---

# Azure-woordenboek 5: private DNS zone

<div class="w-[52%] text-base">

- **DNS**: het telefoonboek van het internet
- **Private DNS zone**: je eigen geheime telefoonboek, alleen leesbaar binnen je VNet
- **Container App Environment**: eigen zone met een wildcard A-record (`*`) naar het interne IP
- **Bij ons**: Terraform maakt de zone, maar pas na *onboarden* in het BMW Manage Portal resolvet hij vanaf het intranet. "Het werkt op mijn machine" dus niet 😅

</div>

<img src="/term-dns.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Private DNS zone: Zonder DNS weet niemand welk IP bij portal-service.<default-domain> hoort. De zone voor de environment heeft een wildcard-record naar het static IP. Daarnaast hebben we een zone voor de registry (privatelink.azurecr.io), de database en de intranetsite 360fp-<env>.azure.cloud.bmw, die we in het BMW Manage Portal aanmaken.
-->

---

# Ingress: twee knoppen, één vraag: wie mag erin?

<div class="w-[52%] text-base">

- **Environment**: internal load balancer aan = alleen vanuit het VNet bereikbaar, met een eigen static IP
- **Container app**: eigen ingress met poort, HTTPS-only en traffic weights per revision
- `external_enabled = true` klinkt als "internet", maar in een interne environment betekent het "iedereen in het VNet". Ja, echt 🤯
- **Bij ons**: environment intern. Het internet komt via Front Door, collega's via VPN en private DNS

</div>

<img src="/term-ingress.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Ingress: Twee lagen. De environment bepaalt of er een internal of publieke load balancer is, en in welk VNet het staat. De container app heeft zijn eigen ingress-blok met target port 8080, allow_insecure uit en traffic weights voor revisions. Bij ons is de environment internal (internal_load_balancer_enabled) en hebben de apps ingress aan. Internetgebruikers komen via Front Door met Private Link, collega's via VPN en de private DNS zone.
-->

---

# Infrastructure as Code, behalve het deel dat je moet klikken

<div class="w-[52%] text-base">

- **Portal** 🖱️: VNet, service principal, tijdelijke rollen en DNS-zones aanvragen
- **Terraform** ⌨️: subnets, NSG's, environment, apps en private endpoints
- **Portal** 🖱️ (alweer): DNS-zones onboarden en endpoint-records aanmaken
- **Tufin** ⏳: firewallregels aanvragen en wachten

</div>

<img src="/term-pingpong.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Heen en weer: Niet alles kan in code. Het VNet, de service principal (met JIT Owner-rol) en de DNS-zones lopen via het BMW Manage Portal. Terraform doet de rest, maar halverwege de apply moeten private DNS zones en private endpoint-records weer handmatig in het portal. Het ging dus een paar keer heen en weer.
-->

---
layout: center
class: text-center
---

# Logging en monitoring

<img src="/ops-overwerkt.svg" class="mx-auto mt-8 h-72 rounded-xl shadow-xl" />

<!--
Volgend probleem: hoe kom je erachter wat je apps doen?
-->

---

# Log Analytics en log stream: "Wat zegt de app nu?"

<div class="w-3/5">

- Log Analytics: eerst KQL leren, dan 5 minuten wachten op je logregel 😴

</div>

<img src="/mon-pijn-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
De ingebouwde Azure-oplossing werkte voor ons niet prettig. Log Analytics vraagt KQL-kennis en heeft ingestie-vertraging, de log stream is live-only. Voor een Java-developer die snel een fout wil zoeken is dat te omslachtig.
-->

---

# Log Analytics en log stream: "Wat zegt de app nu?"

<div class="w-3/5">

- <span class="opacity-50">Log Analytics: eerst KQL leren, dan 5 minuten wachten op je logregel 😴</span>
- Log stream: blink en je mist de fout. Geen historie, geen zoeken

</div>

<img src="/mon-pijn-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
De ingebouwde Azure-oplossing werkte voor ons niet prettig. Log Analytics vraagt KQL-kennis en heeft ingestie-vertraging, de log stream is live-only. Voor een Java-developer die snel een fout wil zoeken is dat te omslachtig.
-->

---

# Log Analytics en log stream: "Wat zegt de app nu?"

<div class="w-3/5">

- <span class="opacity-50">Log Analytics: eerst KQL leren, dan 5 minuten wachten op je logregel 😴</span>
- <span class="opacity-50">Log stream: blink en je mist de fout. Geen historie, geen zoeken</span>
- Logs, traces en metrics: drie plekken, drie schermen, nul overzicht

</div>

<img src="/mon-pijn-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
De ingebouwde Azure-oplossing werkte voor ons niet prettig. Log Analytics vraagt KQL-kennis en heeft ingestie-vertraging, de log stream is live-only. Voor een Java-developer die snel een fout wil zoeken is dat te omslachtig.
-->

---

# Ons alternatief: de Grafana-stack 🔥

<div class="w-[58%] text-lg">

- **Alloy**: collector, ontvangt OpenTelemetry en scrapet `/q/metrics`

</div>

<img src="/mon-stack-1.svg" class="absolute right-8 bottom-12 w-[26rem] rounded-xl" />

<!--
Wij hosten Grafana, Loki, Tempo en Mimir zelf als container apps via Terraform (monitoring.tf). De Quarkus-apps sturen via de Quarkus OpenTelemetry-extensie logs en traces naar Alloy (http://alloy). Alloy routeert traces naar Tempo en logs naar Loki, en scrapet de Quarkus-metrics elke 30 seconden naar Mimir. Opslag is Azure Blob Storage met 31 dagen retentie. Er staat een Quarkus-dashboard klaar in Grafana. Let op: de OTEL-variabelen staan in de terraform bij sommige services nog uitgecommentarieerd tot de dependencies kloppen.
-->

---

# Ons alternatief: de Grafana-stack 🔥

<div class="w-[58%] text-lg">

- <span class="opacity-50">**Alloy**: collector, ontvangt OpenTelemetry en scrapet `/q/metrics`</span>
- **Loki**: logs, **Tempo**: traces, **Mimir**: metrics

</div>

<img src="/mon-stack-2.svg" class="absolute right-8 bottom-12 w-[26rem] rounded-xl" />

<!--
Wij hosten Grafana, Loki, Tempo en Mimir zelf als container apps via Terraform (monitoring.tf). De Quarkus-apps sturen via de Quarkus OpenTelemetry-extensie logs en traces naar Alloy (http://alloy). Alloy routeert traces naar Tempo en logs naar Loki, en scrapet de Quarkus-metrics elke 30 seconden naar Mimir. Opslag is Azure Blob Storage met 31 dagen retentie. Er staat een Quarkus-dashboard klaar in Grafana. Let op: de OTEL-variabelen staan in de terraform bij sommige services nog uitgecommentarieerd tot de dependencies kloppen.
-->

---

# Ons alternatief: de Grafana-stack 🔥

<div class="w-[58%] text-lg">

- <span class="opacity-50">**Alloy**: collector, ontvangt OpenTelemetry en scrapet `/q/metrics`</span>
- <span class="opacity-50">**Loki**: logs, **Tempo**: traces, **Mimir**: metrics</span>
- **Grafana**: dashboards en zoeken op één plek. Eindelijk 😍

</div>

<img src="/mon-stack-3.svg" class="absolute right-8 bottom-12 w-[26rem] rounded-xl" />

<!--
Wij hosten Grafana, Loki, Tempo en Mimir zelf als container apps via Terraform (monitoring.tf). De Quarkus-apps sturen via de Quarkus OpenTelemetry-extensie logs en traces naar Alloy (http://alloy). Alloy routeert traces naar Tempo en logs naar Loki, en scrapet de Quarkus-metrics elke 30 seconden naar Mimir. Opslag is Azure Blob Storage met 31 dagen retentie. Er staat een Quarkus-dashboard klaar in Grafana. Let op: de OTEL-variabelen staan in de terraform bij sommige services nog uitgecommentarieerd tot de dependencies kloppen.
-->

---

# Ons alternatief: de Grafana-stack 🔥

<div class="w-[58%] text-lg">

- <span class="opacity-50">**Alloy**: collector, ontvangt OpenTelemetry en scrapet `/q/metrics`</span>
- <span class="opacity-50">**Loki**: logs, **Tempo**: traces, **Mimir**: metrics</span>
- <span class="opacity-50">**Grafana**: dashboards en zoeken op één plek. Eindelijk 😍</span>
- Alles draait als container apps in dezelfde environment. Eigen data in Blob Storage, 31 dagen bewaard

</div>

<img src="/mon-stack-4.svg" class="absolute right-8 bottom-12 w-[26rem] rounded-xl" />

<!--
Wij hosten Grafana, Loki, Tempo en Mimir zelf als container apps via Terraform (monitoring.tf). De Quarkus-apps sturen via de Quarkus OpenTelemetry-extensie logs en traces naar Alloy (http://alloy). Alloy routeert traces naar Tempo en logs naar Loki, en scrapet de Quarkus-metrics elke 30 seconden naar Mimir. Opslag is Azure Blob Storage met 31 dagen retentie. Er staat een Quarkus-dashboard klaar in Grafana. Let op: de OTEL-variabelen staan in de terraform bij sommige services nog uitgecommentarieerd tot de dependencies kloppen.
-->

---
layout: center
class: text-center
---

# De app: een App Service

<img src="/app-titel.svg" class="mx-auto mt-8 h-72 rounded-xl shadow-xl" />

<!--
Naast de container apps bespreken we de frontend.
-->

---

# App Service: de nette nginx-server van Azure

<div class="w-[52%] text-base">

- Beheerd webhosting-platform: jij levert code of een container, Azure doet het saaie werk (OS, patches, schaling)

</div>

<img src="/app-service-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
App Service is een PaaS voor webapps. Wij draaien een nginx-container (web-app.tf) met public_network_access_enabled=false, VNet-integratie en een private endpoint. De frontend staat in Blob Storage die als mount in de container zit, net als de nginx-configuratie. Vanwege het bedrijfs-DNS gebruiken we WEBSITE_DNS_SERVER 168.63.129.16 zodat private zones resolven.
-->

---

# App Service: de nette nginx-server van Azure

<div class="w-[52%] text-base">

- <span class="opacity-50">Beheerd webhosting-platform: jij levert code of een container, Azure doet het saaie werk (OS, patches, schaling)</span>
- **VNet-integratie**: de app kan naar buiten het VNet in, dus ook bij alle private diensten

</div>

<img src="/app-service-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
App Service is een PaaS voor webapps. Wij draaien een nginx-container (web-app.tf) met public_network_access_enabled=false, VNet-integratie en een private endpoint. De frontend staat in Blob Storage die als mount in de container zit, net als de nginx-configuratie. Vanwege het bedrijfs-DNS gebruiken we WEBSITE_DNS_SERVER 168.63.129.16 zodat private zones resolven.
-->

---

# App Service: de nette nginx-server van Azure

<div class="w-[52%] text-base">

- <span class="opacity-50">Beheerd webhosting-platform: jij levert code of een container, Azure doet het saaie werk (OS, patches, schaling)</span>
- <span class="opacity-50">**VNet-integratie**: de app kan naar buiten het VNet in, dus ook bij alle private diensten</span>
- **Private endpoint**: inkomend alleen via een privé-IP. Het internet blijft buiten 🚪

</div>

<img src="/app-service-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
App Service is een PaaS voor webapps. Wij draaien een nginx-container (web-app.tf) met public_network_access_enabled=false, VNet-integratie en een private endpoint. De frontend staat in Blob Storage die als mount in de container zit, net als de nginx-configuratie. Vanwege het bedrijfs-DNS gebruiken we WEBSITE_DNS_SERVER 168.63.129.16 zodat private zones resolven.
-->

---

# App Service: de nette nginx-server van Azure

<div class="w-[52%] text-base">

- <span class="opacity-50">Beheerd webhosting-platform: jij levert code of een container, Azure doet het saaie werk (OS, patches, schaling)</span>
- <span class="opacity-50">**VNet-integratie**: de app kan naar buiten het VNet in, dus ook bij alle private diensten</span>
- <span class="opacity-50">**Private endpoint**: inkomend alleen via een privé-IP. Het internet blijft buiten 🚪</span>
- Custom domain, certificaten en container-registry-pull via het VNet

</div>

<img src="/app-service-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
App Service is een PaaS voor webapps. Wij draaien een nginx-container (web-app.tf) met public_network_access_enabled=false, VNet-integratie en een private endpoint. De frontend staat in Blob Storage die als mount in de container zit, net als de nginx-configuratie. Vanwege het bedrijfs-DNS gebruiken we WEBSITE_DNS_SERVER 168.63.129.16 zodat private zones resolven.
-->

---

# App Service: de nette nginx-server van Azure

<div class="w-[52%] text-base">

- <span class="opacity-50">Beheerd webhosting-platform: jij levert code of een container, Azure doet het saaie werk (OS, patches, schaling)</span>
- <span class="opacity-50">**VNet-integratie**: de app kan naar buiten het VNet in, dus ook bij alle private diensten</span>
- <span class="opacity-50">**Private endpoint**: inkomend alleen via een privé-IP. Het internet blijft buiten 🚪</span>
- <span class="opacity-50">Custom domain, certificaten en container-registry-pull via het VNet</span>
- **Bij ons**: nginx-container met de Angular-app, content en nginx-config via Blob Storage

</div>

<img src="/app-service-5.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
App Service is een PaaS voor webapps. Wij draaien een nginx-container (web-app.tf) met public_network_access_enabled=false, VNet-integratie en een private endpoint. De frontend staat in Blob Storage die als mount in de container zit, net als de nginx-configuratie. Vanwege het bedrijfs-DNS gebruiken we WEBSITE_DNS_SERVER 168.63.129.16 zodat private zones resolven.
-->

---

# De DNS-hack: een publieke CNAME voor een interne naam 🕵️

<div class="w-[52%] text-base">

- Een custom domain op een App Service vraagt **domeinvalidatie** in de *publieke* DNS

</div>

<img src="/app-dns-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Dit was de lastigste stap: App Service wil bewijs van eigenaarschap voor het domein. Dat kan alleen via TXT en CNAME in de publieke DNS, terwijl onze app volledig privé is. De records worden aangevraagd via het BMW-portal. Daarna binden we het domein en het certificaat uit de Key Vault (azurerm_app_service_certificate_binding). Zie README stap 23.
-->

---

# De DNS-hack: een publieke CNAME voor een interne naam 🕵️

<div class="w-[52%] text-base">

- <span class="opacity-50">Een custom domain op een App Service vraagt **domeinvalidatie** in de *publieke* DNS</span>
- Dus: een **TXT** (`asuid.…`) en een **CNAME** naar `…azurewebsites.net` in de publieke zone `cloud.bmw`

</div>

<img src="/app-dns-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Dit was de lastigste stap: App Service wil bewijs van eigenaarschap voor het domein. Dat kan alleen via TXT en CNAME in de publieke DNS, terwijl onze app volledig privé is. De records worden aangevraagd via het BMW-portal. Daarna binden we het domein en het certificaat uit de Key Vault (azurerm_app_service_certificate_binding). Zie README stap 23.
-->

---

# De DNS-hack: een publieke CNAME voor een interne naam 🕵️

<div class="w-[52%] text-base">

- <span class="opacity-50">Een custom domain op een App Service vraagt **domeinvalidatie** in de *publieke* DNS</span>
- <span class="opacity-50">Dus: een **TXT** (`asuid.…`) en een **CNAME** naar `…azurewebsites.net` in de publieke zone `cloud.bmw`</span>
- Voor ons interne adres `360fp-<env>.azure.cloud.bmw` moest dus de publieke CNAME worden aangepast

</div>

<img src="/app-dns-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Dit was de lastigste stap: App Service wil bewijs van eigenaarschap voor het domein. Dat kan alleen via TXT en CNAME in de publieke DNS, terwijl onze app volledig privé is. De records worden aangevraagd via het BMW-portal. Daarna binden we het domein en het certificaat uit de Key Vault (azurerm_app_service_certificate_binding). Zie README stap 23.
-->

---

# De DNS-hack: een publieke CNAME voor een interne naam 🕵️

<div class="w-[52%] text-base">

- <span class="opacity-50">Een custom domain op een App Service vraagt **domeinvalidatie** in de *publieke* DNS</span>
- <span class="opacity-50">Dus: een **TXT** (`asuid.…`) en een **CNAME** naar `…azurewebsites.net` in de publieke zone `cloud.bmw`</span>
- <span class="opacity-50">Voor ons interne adres `360fp-<env>.azure.cloud.bmw` moest dus de publieke CNAME worden aangepast</span>
- Het echte verkeer loopt daarna via de **private DNS zone** en het private endpoint

</div>

<img src="/app-dns-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Dit was de lastigste stap: App Service wil bewijs van eigenaarschap voor het domein. Dat kan alleen via TXT en CNAME in de publieke DNS, terwijl onze app volledig privé is. De records worden aangevraagd via het BMW-portal. Daarna binden we het domein en het certificaat uit de Key Vault (azurerm_app_service_certificate_binding). Zie README stap 23.
-->

---

# Front Door + nginx: de verkeersleiders 🚦

<div class="w-2/5 text-base">

- Publiek draait de applicatie via **Front Door**

</div>

<img src="/app-routes-1.svg" class="absolute right-8 bottom-12 w-[34rem] rounded-xl" />

<!--
In front-door.tf staan origin groups en routes: web_app voor / en /portal-app, en aparte routes voor portal-service, notification-service, external-notification-service en translation-service. Intern gebruikt nginx (config/<env>/web-app/nginx-location.conf) proxy_pass naar de container apps. De App Service heeft VNet-integratie met vnet_route_all, dus bereikt hij de interne environment.
-->

---

# Front Door + nginx: de verkeersleiders 🚦

<div class="w-2/5 text-base">

- <span class="opacity-50">Publiek draait de applicatie via **Front Door**</span>
- Routes: `/` en `/portal-app/*` naar de App Service, `/<service>/*` naar de container apps

</div>

<img src="/app-routes-2.svg" class="absolute right-8 bottom-12 w-[34rem] rounded-xl" />

<!--
In front-door.tf staan origin groups en routes: web_app voor / en /portal-app, en aparte routes voor portal-service, notification-service, external-notification-service en translation-service. Intern gebruikt nginx (config/<env>/web-app/nginx-location.conf) proxy_pass naar de container apps. De App Service heeft VNet-integratie met vnet_route_all, dus bereikt hij de interne environment.
-->

---

# Front Door + nginx: de verkeersleiders 🚦

<div class="w-2/5 text-base">

- <span class="opacity-50">Publiek draait de applicatie via **Front Door**</span>
- <span class="opacity-50">Routes: `/` en `/portal-app/*` naar de App Service, `/<service>/*` naar de container apps</span>
- Front Door gebruikt Private Link naar beide

</div>

<img src="/app-routes-3.svg" class="absolute right-8 bottom-12 w-[34rem] rounded-xl" />

<!--
In front-door.tf staan origin groups en routes: web_app voor / en /portal-app, en aparte routes voor portal-service, notification-service, external-notification-service en translation-service. Intern gebruikt nginx (config/<env>/web-app/nginx-location.conf) proxy_pass naar de container apps. De App Service heeft VNet-integratie met vnet_route_all, dus bereikt hij de interne environment.
-->

---

# Front Door + nginx: de verkeersleiders 🚦

<div class="w-2/5 text-base">

- <span class="opacity-50">Publiek draait de applicatie via **Front Door**</span>
- <span class="opacity-50">Routes: `/` en `/portal-app/*` naar de App Service, `/<service>/*` naar de container apps</span>
- <span class="opacity-50">Front Door gebruikt Private Link naar beide</span>
- **nginx** op de App Service proxyt API-calls naar de container apps.

</div>

<img src="/app-routes-4.svg" class="absolute right-8 bottom-12 w-[34rem] rounded-xl" />

<!--
In front-door.tf staan origin groups en routes: web_app voor / en /portal-app, en aparte routes voor portal-service, notification-service, external-notification-service en translation-service. Intern gebruikt nginx (config/<env>/web-app/nginx-location.conf) proxy_pass naar de container apps. De App Service heeft VNet-integratie met vnet_route_all, dus bereikt hij de interne environment.
-->

---
layout: center
class: text-center
---

# Wat werd het bij ons?

<img src="/aca-landschap.svg" class="mx-auto mt-4 h-[26rem] rounded-xl shadow-xl" />

<!--
Overzicht: Front Door met WAF en Private Link voor internet, VPN en firewall-hub voor intranet, een spoke VNet met per dienst een subnet met NSG, private endpoints voor Service Bus, Storage, Key Vault en registry, en private DNS zones.
-->

---

# Spiekbrief voor de borrel 🍺

<div class="text-sm leading-tight">

| Term | Betekenis |
|---|---|
| VNet / subnet | Eigen netwerk in Azure en een deel daarvan |
| NSG | Firewall op subnet- of netwerkkaartniveau |
| Route table (UDR) | Bepaalt via welke route verkeer gaat |
| Private endpoint | Privé-IP in ons subnet naar een Azure-dienst |
| Private Link | Techniek achter private endpoints |
| Private DNS zone | Naam naar IP, alleen binnen gelinkte VNets |
| Container App Environment | Grens rond apps: VNet, load balancer, logs |
| Ingress | Instellingen voor inkomend verkeer |

</div>

<!--
Spiekbrief om terug te kijken tijdens de Q&A.
-->
