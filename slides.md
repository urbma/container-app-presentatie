---
theme: seriph
background: /title-bg.svg
title: Van AKS naar Azure Container Apps
colorSchema: dark
info: |
  ## Van AKS naar Azure Container Apps
  Ons migratieproject.
class: text-center
drawings:
  persist: false
transition: fade
comark: true
duration: 35min
---

# Van AKS naar Azure Container Apps

Ons migratieproject

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Druk op spatie voor de volgende slide <carbon:arrow-right />
</div>

---
transition: fade
---

# Wat zijn Azure Container Apps?

Een **serverless containerplatform** van Azure om containers te draaien zonder zelf een cluster te beheren.

<img src="/aca-vracht.svg" class="mx-auto mt-4 h-72 rounded-xl shadow-xl" />

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

<div class="w-3/5">

- Eén commando en je app draait

</div>

<img src="/mig-makkelijk.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Eén commando: Een container app aanmaken is in het begin heel makkelijk. Voor een proof of concept ben je in minuten klaar.
-->

---

# Een container app aanmaken

<div class="w-3/5">

- <span class="opacity-50">Eén commando en je app draait</span>
- Publieke URL en HTTPS krijg je cadeau

</div>

<img src="/mig-makkelijk.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Publieke URL en HTTPS: Out of the box krijg je een publieke URL met certificaat. Prima voor een demo, niet voor ons.
-->

---

# Een container app aanmaken

<div class="w-3/5">

- <span class="opacity-50">Eén commando en je app draait</span>
- <span class="opacity-50">Publieke URL en HTTPS krijg je cadeau</span>
- Dan komt de eis: alleen via het VNet bereikbaar

</div>

<img src="/mig-eis.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
De eis: Bij ons mag een container app niet publiek bereikbaar zijn, alleen via een VNet. En daar begon het gedoe.
-->

---

# De eis: alleen via het VNet

<div class="w-3/5">

- Geen publiek endpoint toegestaan

</div>

<img src="/aca-netwerk.svg" class="absolute right-10 bottom-12 w-[28rem] rounded-xl" />

<!--
Geen publiek endpoint: Security-eis. Alles moet via het BMW-netwerk lopen.
-->

---

# De eis: alleen via het VNet

<div class="w-3/5">

- <span class="opacity-50">Geen publiek endpoint toegestaan</span>
- Dus: een Container App Environment in een VNet

</div>

<img src="/aca-netwerk.svg" class="absolute right-10 bottom-12 w-[28rem] rounded-xl" />

<!--
Container App Environment: De container apps leven in een environment, en die moet in een eigen VNet-subnet hangen (VNet-integratie).
-->

---

# De eis: alleen via het VNet

<div class="w-3/5">

- <span class="opacity-50">Geen publiek endpoint toegestaan</span>
- <span class="opacity-50">Dus: een Container App Environment in een VNet</span>
- Dus: subnets die aan eisen voldoen

</div>

<img src="/aca-netwerk.svg" class="absolute right-10 bottom-12 w-[28rem] rounded-xl" />

<!--
Subnets: Dat subnet moet voldoen aan eisen: voldoende groot (bij ons /27), gedelegeerd aan Microsoft.App/environments en met een NSG en route table.
-->

---

# De eis: alleen via het VNet

<div class="w-3/5">

- <span class="opacity-50">Geen publiek endpoint toegestaan</span>
- <span class="opacity-50">Dus: een Container App Environment in een VNet</span>
- <span class="opacity-50">Dus: subnets die aan eisen voldoen</span>
- En: private endpoints en private DNS eromheen

</div>

<img src="/aca-netwerk.svg" class="absolute right-10 bottom-12 w-[28rem] rounded-xl" />

<!--
Private endpoints en DNS: Alle diensten eromheen (Key Vault, Service Bus, Storage, registry) moeten ook privé bereikbaar zijn, en dat vraagt private endpoints en private DNS zones. Wat is dat allemaal? De volgende slides leggen de termen uit.
-->

---

# Azure-termen: VNet en subnet

<div class="w-[52%] text-base">

- **VNet** (virtual network): jouw eigen, afgeschermde netwerk in Azure
- **Subnet**: een deel van dat netwerk met een eigen IP-bereik, bijv. `10.15.60.224/27`
- **Delegation**: subnet exclusief voor één dienst, bij ons `Microsoft.App/environments`
- **Bij ons**: VNet aanmaken kan niet zomaar in de Azure Portal, maar via het BMW Manage Portal (`/25`, Frankfurt, gekoppeld aan een firewall-hub)

</div>

<img src="/term-vnet.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
VNet en subnet: Een VNet is een eigen privé-netwerk. Subnets verdelen dat netwerk. Het VNet maken we aan in het BMW Manage Portal (regio Frankfurt, grootte /25 wordt automatisch goedgekeurd) en het wordt gepeerd met de GEN2 firewall hub. Daarna verwijderen we de default subnet en NSG en maakt Terraform onze eigen subnets aan: private-endpoints, database, service-bus, container-apps, storage en web-app.
-->

---

# Azure-termen: NSG, route table en firewall

<div class="w-[52%] text-base">

- **NSG** (network security group): poortwachter op een subnet, wie mag op welke poort erin en eruit
- **Route table** (UDR): stuurt uitgaand verkeer (`0.0.0.0/0`) via de firewall in de hub
- **Hub-spoke / peering**: ons VNet (spoke) is gekoppeld aan het BMW-hub met VPN en firewall
- **Tufin**: hiermee vragen we firewallregels aan voor verkeer dat erdoor moet

</div>

<img src="/term-nsg.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
NSG en route table: Elk subnet krijgt een NSG. Uitgaand verkeer gaat via een route table naar de hub-firewall. Wil je naar buiten (bijv. een externe URL), dan moet er een firewallregel via Tufin komen.
-->

---

# Azure-termen: private endpoint

<div class="w-[52%] text-base">

- **Private endpoint**: een netwerkkaart met een **privé-IP** in jouw subnet, die naar een Azure-dienst wijst
- Key Vault, Service Bus, Storage en Registry zijn zo niet meer publiek bereikbaar
- **Private Link**: de techniek erachter. Front Door gebruikt die ook om bij onze Container App Environment te komen
- **Bij ons**: de DNS-records van elke private endpoint moeten we aanmaken via het BMW Manage Portal

</div>

<img src="/term-pe.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Private endpoint: Een deur naar een PaaS-dienst die alleen binnen het VNet bestaat. De dienst zelf zet je dicht voor het publieke internet. In het BMW Manage Portal onder Manage Private Endpoint DNS Records moet je per endpoint de DNS-records laten aanmaken (Create of Create Missing).
-->

---

# Azure-termen: private DNS zone

<div class="w-[52%] text-base">

- **DNS**: vertaalt een naam naar een IP-adres
- **Private DNS zone**: zo'n vertaaltabel die alleen geldt binnen een gelinkt VNet
- **Container App Environment**: zone met de default domain als naam en een wildcard A-record naar het interne IP
- **Bij ons**: zones via Terraform, daarna *onboarden* in het BMW Manage Portal. Pas dan resolven ze vanaf het intranet

</div>

<img src="/term-dns.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Private DNS zone: Zonder DNS weet niemand welk IP bij portal-service.<default-domain> hoort. De zone voor de environment heeft een wildcard-record naar het static IP. Daarnaast hebben we een zone voor de registry (privatelink.azurecr.io), de database en de intranetsite 360fp-<env>.azure.cloud.bmw, die we in het BMW Manage Portal aanmaken.
-->

---

# Ingress: wat zit waar?

<div class="w-[52%] text-base">

- **Environment**: internal load balancer aan = alleen bereikbaar vanuit het VNet, met een eigen static IP
- **Container app**: ingress-instellingen per app: poort, alleen HTTPS, verkeer per revision
- `external_enabled` op de app bepaalt wie erbij mag, binnen wat de environment toestaat
- **Bij ons**: environment intern. Internet via Front Door en Private Link, intranet via VPN en private DNS

</div>

<img src="/term-ingress.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Ingress: Twee lagen. De environment bepaalt of er een internal of publieke load balancer is, en in welk VNet het staat. De container app heeft zijn eigen ingress-blok met target port 8080, allow_insecure uit en traffic weights voor revisions. Bij ons is de environment internal (internal_load_balancer_enabled) en hebben de apps ingress aan. Internetgebruikers komen via Front Door met Private Link, collega's via VPN en de private DNS zone.
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

# Portal en Terraform: heen en weer

<div class="w-[52%] text-base">

- **Portal**: VNet, service principal, tijdelijke rollen en DNS-zones aanvragen
- **Terraform**: subnets, NSG's, environment, apps en private endpoints
- **Portal**: private DNS zones onboarden en endpoint-DNS-records aanmaken
- **Tufin**: firewallregels aanvragen

</div>

<img src="/term-pingpong.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Heen en weer: Niet alles kan in code. Het VNet, de service principal (met JIT Owner-rol) en de DNS-zones lopen via het BMW Manage Portal. Terraform doet de rest, maar halverwege de apply moeten private DNS zones en private endpoint-records weer handmatig in het portal. Het ging dus een paar keer heen en weer.
-->

---

# Spiekbrief: Azure-termen

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

---
layout: center
class: text-center
---

# Logging en monitoring

<!--
Volgend probleem: hoe kom je erachter wat je apps doen?
-->

---

# Log Analytics en log stream: pijnlijk

<div class="w-3/5">

- Log Analytics: KQL leren, en logs verschijnen met vertraging
- Log stream: alleen live, geen historie, geen zoeken
- Logs, traces en metrics zitten op verschillende plekken

</div>

<img src="/mon-pijn.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
De ingebouwde Azure-oplossing werkte voor ons niet prettig. Log Analytics vraagt KQL-kennis en heeft ingestie-vertraging, de log stream is live-only. Voor een Java-developer die snel een fout wil zoeken is dat te omslachtig.
-->

---

# Ons alternatief: Grafana-stack

<div class="w-2/5 text-base">

- **Alloy**: collector, ontvangt OpenTelemetry en scrapet `/q/metrics`
- **Loki**: logs, **Tempo**: traces, **Mimir**: metrics
- **Grafana**: dashboards en zoeken op één plek
- Alles draait zelf als container app in dezelfde environment, data in Blob Storage (31 dagen)

</div>

<img src="/mon-stack.svg" class="absolute right-8 bottom-12 w-[34rem] rounded-xl" />

<!--
Wij hosten Grafana, Loki, Tempo en Mimir zelf als container apps via Terraform (monitoring.tf). De Quarkus-apps sturen via de Quarkus OpenTelemetry-extensie logs en traces naar Alloy (http://alloy). Alloy routeert traces naar Tempo en logs naar Loki, en scrapet de Quarkus-metrics elke 30 seconden naar Mimir. Opslag is Azure Blob Storage met 31 dagen retentie. Er staat een Quarkus-dashboard klaar in Grafana. Let op: de OTEL-variabelen staan in de terraform bij sommige services nog uitgecommentarieerd tot de dependencies kloppen.
-->
