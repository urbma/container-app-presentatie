---
theme: seriph
background: /title-bg.svg
title: Van AKS naar Azure Container Apps
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

# Wat is Azure Container Apps?

Een **serverless containerplatform** van Azure om containers te draaien zonder zelf een cluster te beheren.

<!--
Kernboodschap: Container Apps zit tussen App Service en AKS in. Je krijgt de flexibiliteit van containers, zonder de operationele last van een Kubernetes-cluster.
-->

---

# Wat krijg je?

- Geen Kubernetes-kennis nodig

<!--
Geen Kubernetes-kennis nodig: Je draait containers zonder je zorgen te maken over nodes, upgrades of manifests.
-->

---

# Wat krijg je?

- <span class="opacity-50">Geen Kubernetes-kennis nodig</span>
- Automatisch schalen, ook naar nul

<!--
Automatisch schalen, ook naar nul: Schaal op HTTP-verkeer, queues of events. Geen verkeer? Dan draait er niets.
-->

---

# Wat krijg je?

- <span class="opacity-50">Geen Kubernetes-kennis nodig</span>
- <span class="opacity-50">Automatisch schalen, ook naar nul</span>
- Ingress, HTTPS en revisions

<!--
Ingress, HTTPS en revisions: Ingebouwd, zonder ingress controller of certificaatbeheer. Revisions maken gefaseerde uitrol en rollback eenvoudig.
-->

---

# Wat krijg je?

- <span class="opacity-50">Geen Kubernetes-kennis nodig</span>
- <span class="opacity-50">Automatisch schalen, ook naar nul</span>
- <span class="opacity-50">Ingress, HTTPS en revisions</span>
- Betalen per gebruik

<!--
Betalen per gebruik: Je betaalt voor CPU en geheugen per seconde dat je app echt draait.
-->

---

# Onder de motorkap

- Draait op Kubernetes

<!--
Draait op Kubernetes: Maar die laag is volledig voor je verborgen.
-->

---

# Onder de motorkap

- <span class="opacity-50">Draait op Kubernetes</span>
- KEDA, Dapr en Envoy

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

<!--
Container Apps Environment: Apps leven samen in een environment: een gedeeld netwerk- en logboundary.
-->

---

# Onder de motorkap

- <span class="opacity-50">Draait op Kubernetes</span>
- <span class="opacity-50">KEDA, Dapr en Envoy</span>
- <span class="opacity-50">Container Apps Environment</span>
- Waarvoor geschikt?

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

<!--
Zelf het cluster onderhouden: Kubernetes-versie upgrades, node image updates en patches zijn onze verantwoordelijkheid.
-->

---

# Waarom weg van AKS?

- <span class="opacity-50">Zelf het cluster onderhouden</span>
- Kosten voor 2 microservices en 1 app

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

<!--
Incidenten en security zijn van ons: Problemen met nodes, netwerk of kwetsbaarheden lossen wij zelf op, zonder specialistisch team.
-->
