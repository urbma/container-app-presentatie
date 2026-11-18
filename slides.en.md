---
theme: seriph
background: /en/title-bg.svg
title: From AKS to ACA
colorSchema: dark
info: |
  ## From AKS to ACA
  Our migration project.
class: text-center
drawings:
  persist: false
transition: fade
comark: true
duration: 35min
---

# From AKS to ACA

Our migration project, or: what does it take to get an application into the cloud?

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press space for the next slide <carbon:arrow-right />
</div>

---
transition: fade
---

# What is this about?

<div class="w-[52%] text-base">

- The tests are green, the build passes. That's when deploying really begins

</div>

<img src="/en/frame-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Goal of the presentation: as a developer, deploying usually makes you think of a pipeline and a container. In practice, most of the work is in everything around your application. We show that through our migration journey. This is not an Azure training, but a story about what you run into.
-->

---
transition: fade
---

# What is this about?

<div class="w-[52%] text-base">

- <span class="opacity-50">The tests are green, the build passes. That's when deploying really begins</span>
- This presentation shows **what it takes** to put an application in the cloud

</div>

<img src="/en/frame-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
This presentation shows what it takes to put an application in the cloud
-->

---
transition: fade
---

# What is this about?

<div class="w-[52%] text-base">

- <span class="opacity-50">The tests are green, the build passes. That's when deploying really begins</span>
- <span class="opacity-50">This presentation shows **what it takes** to put an application in the cloud</span>
- Not the code, but everything around it: network, security, DNS, logging and a frontend

</div>

<img src="/en/frame-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Not the code, but everything around it: network, security, DNS, logging and a frontend
-->

---
transition: fade
---

# What is this about?

<div class="w-[52%] text-base">

- <span class="opacity-50">The tests are green, the build passes. That's when deploying really begins</span>
- <span class="opacity-50">This presentation shows **what it takes** to put an application in the cloud</span>
- <span class="opacity-50">Not the code, but everything around it: network, security, DNS, logging and a frontend</span>
- Through our migration from AKS to ACA. Scars included

</div>

<img src="/en/frame-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Through our migration from AKS to ACA. Scars included
-->

---
transition: fade
---

# What is an Azure Container App?

A **serverless container platform** from Azure for running containers without managing a cluster yourself.

<img src="/en/aca-schip.svg" class="mx-auto mt-4 h-72 rounded-xl shadow-xl" />

<!--
Key message: in terms of management effort, Container Apps sits between App Service and AKS.
- App Service: you hand Azure your code or container and the platform takes care of almost everything. Little work, but also little freedom (mostly one web app at a time).
- AKS: a full Kubernetes cluster. Maximum freedom, but you manage nodes, upgrades, ingress and manifests.
- Container Apps: you just run containers (multiple services, background jobs, scale to zero), but Azure manages the cluster underneath. More freedom than App Service, less work than AKS.
-->

---

# What do you get?

- No Kubernetes knowledge needed

<img src="/en/krijg-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
No Kubernetes knowledge needed: You run containers without worrying about nodes, upgrades or manifests.
-->

---

# What do you get?

- <span class="opacity-50">No Kubernetes knowledge needed</span>
- Automatic scaling, even to zero

<img src="/en/krijg-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Automatic scaling, even to zero: Scale on HTTP traffic, queues or events. No traffic? Then nothing runs. Scaling out takes seconds, even for GPU workloads.
-->

---

# What do you get?

- <span class="opacity-50">No Kubernetes knowledge needed</span>
- <span class="opacity-50">Automatic scaling, even to zero</span>
- Ingress, HTTPS and revisions

<img src="/en/krijg-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Ingress, HTTPS and revisions: built in, without an ingress controller or certificate management.
- Ingress = the front door of your app: the rules that determine how traffic from outside (or from other apps) reaches your container, on which port and with which hostname.
- Ingress controller: in Kubernetes (AKS) ingress is only a set of rules. You have to install a component yourself to enforce them, usually an nginx or Traefik controller or the Application Gateway variant. You also have to update and configure it. In Container Apps this is built in (Envoy), you just switch ingress on.
- Certificates: HTTPS means the front door needs a certificate. On AKS you often arrange that yourself, for example with cert-manager and Let's Encrypt, including renewal. Container Apps can issue and renew a managed certificate for your own domain; an Azure domain (*.azurecontainerapps.io) already has one anyway.
- Your own certificates (BYO): also possible. You upload a certificate (PFX/PEM) to the environment or refer to a certificate in Key Vault, and bind it to your custom domain. Then you are responsible for purchasing and renewing; with Key Vault and a managed identity, Azure can pick up a renewed version automatically. This is often the route for internal or private domains, because a managed certificate needs a publicly verifiable domain name.
- Revisions make phased rollouts and rollbacks easy.
-->

---

# What do you get?

- <span class="opacity-50">No Kubernetes knowledge needed</span>
- <span class="opacity-50">Automatic scaling, even to zero</span>
- <span class="opacity-50">Ingress, HTTPS and revisions</span>
- Pay per use

<img src="/en/krijg-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Pay per use: You pay for CPU and memory per second that your app actually runs. Idle costs nothing and the Consumption plan has a free monthly allowance.
- Consumption (workload profile): Container Apps has two kinds of environments. The older "Consumption-only" environment runs everything on shared, Azure-managed capacity. The newer "workload profiles" environment lets you choose per app: the Consumption profile (serverless, you pay per second of CPU and memory and scale to zero) or a dedicated profile (fixed VM size, reservation, also GPU, you pay for the VMs). You can combine them in one environment.
- Why a smaller network is possible: with Consumption-only, Azure asks for a large subnet (minimum /23, well over 500 addresses), because the scaling headroom comes out of your subnet. With workload profiles the subnet can be much smaller (minimum /27, 32 addresses), because the Consumption capacity is managed by Azure and not every replica needs its own IP from your subnet; only dedicated nodes count. Less IP space from the corporate network is a big advantage in a shared hub-spoke landscape. Check the exact subnet sizes and IP rules in the current Azure documentation.
-->

---

# What do you get?

- <span class="opacity-50">No Kubernetes knowledge needed</span>
- <span class="opacity-50">Automatic scaling, even to zero</span>
- <span class="opacity-50">Ingress, HTTPS and revisions</span>
- <span class="opacity-50">Pay per use</span>
- Secure and reliable

<img src="/en/krijg-5.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Secure and reliable: Managed identities, network isolation and access control are built in. Microsoft offers an SLA of 99.95% (according to the product page; check the current SLA for our configuration).

Managed identity: an identity that Azure itself gives to your app, comparable to a service employee with their own badge. With it, your app can sign in to other Azure services (Key Vault, Storage, Service Bus, the container registry) without you having to put passwords, connection strings or secrets in your configuration. Azure creates and rotates the credentials, you only grant the identity the rights it needs through a role assignment (RBAC), for example "read from this Key Vault". There are two kinds: system-assigned (belongs to a single app and disappears with the app) and user-assigned (created separately and shareable between multiple apps). Advantage: no secrets that can leak or expire.
-->

---

# What do you get?

- <span class="opacity-50">No Kubernetes knowledge needed</span>
- <span class="opacity-50">Automatic scaling, even to zero</span>
- <span class="opacity-50">Ingress, HTTPS and revisions</span>
- <span class="opacity-50">Pay per use</span>
- <span class="opacity-50">Secure and reliable</span>
- Network integration (VNet, private endpoints)

<img src="/en/krijg-6.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Network integration: Container Apps run in your own VNet, with private endpoints to databases, storage and other PaaS services. Handy, but exactly where we had to figure out the most (see later).
-->

---

# What do you get?

- <span class="opacity-50">No Kubernetes knowledge needed</span>
- <span class="opacity-50">Automatic scaling, even to zero</span>
- <span class="opacity-50">Ingress, HTTPS and revisions</span>
- <span class="opacity-50">Pay per use</span>
- <span class="opacity-50">Secure and reliable</span>
- <span class="opacity-50">Network integration (VNet, private endpoints)</span>
- Standard containers, no lock-in

<img src="/en/krijg-7.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Standard containers, no lock-in: Deploy from any container registry (Docker Hub, Azure Container Registry, your own registry). Open source building blocks (Dapr, KEDA, Envoy) and plain containers keep the door open to other platforms.
-->

---

# Under the hood

- Runs on Kubernetes

<img src="/en/motorkap-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Runs on Kubernetes: But that layer is completely hidden from you.
-->

---

# Under the hood

- <span class="opacity-50">Runs on Kubernetes</span>
- KEDA, Dapr and Envoy

<img src="/en/motorkap-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
KEDA, Dapr and Envoy: Event-driven autoscaling, microservice building blocks and ingress/traffic splitting are already built in.

- KEDA (Kubernetes Event-driven Autoscaling): scales your app based on events, such as HTTP traffic, the length of a queue (Service Bus, Kafka) or a cron schedule. Can go down to zero instances and back up again.
- Dapr (Distributed Application Runtime): a sidecar with ready-made building blocks for microservices: service-to-service calls, pub/sub, state store, secrets and bindings. Your app talks to the sidecar over HTTP/gRPC, and the sidecar handles the rest, regardless of the underlying technology. Optional in Container Apps and we don't use it (no Dapr in our terraform); it is part of what the platform offers, though.
  Would Dapr have given us anything? A little: Service Bus, Key Vault and Storage through fixed APIs instead of SDKs, plus mTLS and retries between services. But Quarkus already has good extensions for that, Dapr adds a sidecar and an extra abstraction layer, and our pain was in the network, which Dapr doesn't solve.
- Envoy: a proxy that handles incoming traffic: ingress, HTTPS/TLS termination, load balancing and traffic splitting between revisions (e.g. a 90/10 rollout).

All open source and managed by Azure, so we don't have to install or upgrade them ourselves.
-->

---

# Under the hood

- <span class="opacity-50">Runs on Kubernetes</span>
- <span class="opacity-50">KEDA, Dapr and Envoy</span>
- Container Apps Environment

<img src="/en/motorkap-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Container Apps Environment: Apps live together in an environment: a shared network and log boundary.

What else the environment offers beyond what we use:
- Workload profiles and GPU: dedicated capacity alongside Consumption.
- Dapr components and shared configuration: environment-wide building blocks (pub/sub, state, secrets).
- Environment-wide mTLS and peer-to-peer encryption between apps.
- Jobs: scheduled or one-off tasks alongside the apps.
- Managed certificates and your own certificates per environment, which can be bound to custom domains.
- Azure Files mounts as shared storage for multiple apps.
- Zone redundancy for higher availability, and more log destinations (Log Analytics, Event Hubs).

Certificates: in our setup Front Door and the App Service terminate the HTTPS traffic, so the environment doesn't need them. You need them on the environment as soon as clients talk directly to the container app through your own domain name, for example a public API without Front Door in front of it, or internal traffic that still has to go over HTTPS with a corporate domain. Also if you want end-to-end TLS all the way into the app, instead of encryption that ends at Front Door. Check this against the Azure documentation.
-->

---

# Under the hood

- <span class="opacity-50">Runs on Kubernetes</span>
- <span class="opacity-50">KEDA, Dapr and Envoy</span>
- <span class="opacity-50">Container Apps Environment</span>
- What is it good for?

<img src="/en/motorkap-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
What is it good for?: APIs, microservices, jobs and background processing.
-->

---
layout: center
class: text-center
---

# Why leave AKS?

<img src="/en/aks-fire.svg" class="mx-auto mt-8 h-72 rounded-xl shadow-xl" />

<!--
Title slide: AKS was stable, that is not the problem. But the cost (nodes 24/7) and the maintenance, especially the heavy cluster upgrades and migrations, are a mountain of work for only four workloads (notification, external notification, portal and translation service).
-->

---

# Why leave AKS?

- Maintaining the cluster ourselves

<img src="/en/aks-onderhoud.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Maintaining the cluster ourselves: Kubernetes version upgrades, node image updates and patches are our responsibility.
-->

---

# Why leave AKS?

- <span class="opacity-50">Maintaining the cluster ourselves</span>
- Cost for 4 workloads

<img src="/en/aks-kosten.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Cost for four workloads: Nodes run 24/7, even when nothing is happening. For four workloads we pay for a lot of capacity we don't use.
-->

---
dragPos:
  square: 691,32,167,_,-16
---

# Why leave AKS?

- <span class="opacity-50">Maintaining the cluster ourselves</span>
- <span class="opacity-50">Cost for 4 workloads</span>
- Lots of DIY setup

<img src="/en/aks-inrichten.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Lots of DIY setup: Ingress controller, certificates, monitoring and autoscaling we have to install and manage ourselves.
-->

---

# Why leave AKS?

- <span class="opacity-50">Maintaining the cluster ourselves</span>
- <span class="opacity-50">Cost for 4 workloads</span>
- <span class="opacity-50">Lots of DIY setup</span>
- Steep learning curve

<img src="/en/leercurve.svg" class="absolute right-10 bottom-10 w-96 rounded-xl shadow-xl" />

<!--
Steep learning curve: YAML, Helm, networking and RBAC: a lot of Kubernetes knowledge for a small project.
-->

---

# Why leave AKS?

- <span class="opacity-50">Maintaining the cluster ourselves</span>
- <span class="opacity-50">Cost for 4 workloads</span>
- <span class="opacity-50">Lots of DIY setup</span>
- <span class="opacity-50">Steep learning curve</span>
- Knowledge held by few people

<img src="/en/aks-buszijn.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Knowledge held by few people: Without an infra team, the cluster depends on one or two people (bus factor).
Bus factor: the number of people who can be hit by a bus (or fall ill, go on holiday or leave) before the project grinds to a halt. If the bus factor is 1, only one person can upgrade or repair the cluster. The lower it is, the more vulnerable the team.
-->

---

# Why leave AKS?

- <span class="opacity-50">Maintaining the cluster ourselves</span>
- <span class="opacity-50">Cost for 4 workloads</span>
- <span class="opacity-50">Lots of DIY setup</span>
- <span class="opacity-50">Steep learning curve</span>
- <span class="opacity-50">Knowledge held by few people</span>
- Incidents and security are on us

<img src="/en/aks-incidenten.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Incidents and security are on us: Problems with nodes, network or vulnerabilities we solve ourselves, without a specialist team.
-->

---
layout: center
class: text-center
---

# The migration

<img src="/en/migratie.svg" class="mx-auto mt-8 h-72 rounded-xl shadow-xl" />

<!--
Title slide: The move from AKS to Container Apps was not painless. The network in particular (VNet, subnets, NSGs, DNS, private endpoints) took a lot of figuring out.
-->

---

# Creating a container app

<div class="w-[85%]">

- One command and your app is running. What could possibly go wrong? 😇

</div>

<img src="/en/mig-makkelijk.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
One command: Creating a container app is really easy at first. For a proof of concept you're done in minutes.
-->

---

# Creating a container app

<div class="w-[85%]">

- <span class="opacity-50">One command and your app is running. What could possibly go wrong? 😇</span>
- A public URL and HTTPS come free of charge

</div>

<img src="/en/mig-https.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Public URL and HTTPS: Out of the box you get a public URL with a certificate. Fine for a demo, not for us.
-->

---

# Creating a container app

<div class="w-[85%]">

- <span class="opacity-50">One command and your app is running. What could possibly go wrong? 😇</span>
- <span class="opacity-50">A public URL and HTTPS come free of charge</span>
- Then comes the requirement: only via the VNet 🔒

</div>

<img src="/en/mig-eis.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
The requirement: At our company a container app must not be publicly reachable, only via a VNet. And that's where the trouble started.
-->

---

# Only via the VNet

<div class="w-[52%]">

- No public endpoint

</div>

<img src="/en/aca-netwerk.svg" class="absolute right-8 bottom-8 w-[26rem] rounded-xl" />

<!--
No public endpoint: Security requirement. Everything has to go through the BMW network.
-->

---

# Only via the VNet

<div class="w-[52%]">

- <span class="opacity-50">No public endpoint</span>
- So: a Container App Environment in a VNet

</div>

<img src="/en/aca-netwerk.svg" class="absolute right-8 bottom-8 w-[26rem] rounded-xl" />

<!--
Container App Environment: The container apps live in an environment, and that environment has to sit in its own VNet subnet (VNet integration).
-->

---

# Only via the VNet

<div class="w-[52%]">

- <span class="opacity-50">No public endpoint</span>
- <span class="opacity-50">So: a Container App Environment in a VNet</span>
- So: subnets with requirements (size, delegation)

</div>

<img src="/en/aca-netwerk.svg" class="absolute right-8 bottom-8 w-[26rem] rounded-xl" />

<!--
Subnets: That subnet has to meet requirements: large enough (a /27 in our case), delegated to Microsoft.App/environments, and with an NSG and route table.
-->

---

# Only via the VNet

<div class="w-[52%]">

- <span class="opacity-50">No public endpoint</span>
- <span class="opacity-50">So: a Container App Environment in a VNet</span>
- <span class="opacity-50">So: subnets with requirements (size, delegation)</span>
- And: private endpoints and private DNS

</div>

<img src="/en/aca-netwerk.svg" class="absolute right-8 bottom-8 w-[26rem] rounded-xl" />

<!--
Private endpoints and DNS: All the services around it (Key Vault, Service Bus, Storage, registry) must also be reachable privately, and that requires private endpoints and private DNS zones. What is all that? The next slides explain the terms.
-->

---

# Azure glossary 1: VNet and subnet

<div class="w-[52%] text-base">

- **VNet**: your own private LAN inside Azure, the office network but virtual
- **Subnet**: a walled-off segment of that LAN, like the server room, with its own IP range, e.g. `10.15.60.224/27`
- **Delegation**: a subnet exclusively for one service, in our case `Microsoft.App/environments`
- **In our case**: "Create VNet" in the Azure Portal? Forget it. That goes through the BMW Manage Portal 🙃

</div>

<img src="/en/term-vnet.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
VNet and subnet: A VNet is a private network of your own. Subnets divide that network. We create the VNet in the BMW Manage Portal (Frankfurt region, size /25 is approved automatically) and it is peered with the GEN2 firewall hub. After that we delete the default subnet and NSG, and Terraform creates our own subnets: private-endpoints, database, service-bus, container-apps, storage and web-app.
-->

---

# The Manage Portal: the front door to your network

<div class="w-[40%] text-base">

- **Network**: request a VNet and subnets
- **Private Endpoint** and **DNS**: onboard endpoints and DNS zones, request records
- **Technical Identities**: service principals for your pipeline
- **ITSM Ticket**: for everything that has to go through a service request
- **Cloudrooms**: four environments for us: TEST, INT, PROD and Shared

</div>

<img src="/manage-portal.png" class="absolute right-8 bottom-8 w-[34rem] rounded-xl shadow-xl" />

<!--
The BMW Manage Portal (manage.azure.bmw.cloud) is the self-service portal for the Azure platform. What you would normally do yourself in the Azure Portal goes through this portal here: requesting a VNet and subnets, onboarding private endpoints and DNS zones, requesting DNS records, creating technical identities (service principals) and opening ITSM tickets. The left-hand menu shows Network, DNS, Private Endpoint, Technical Identities and ITSM Ticket. The dashboard shows our cloudrooms: TEST, INT, PROD and Shared, each with status, Secure Score and billing. Why this slide: part of the infrastructure cannot be handled with Terraform, you have to click or request it here.
-->

---

# The Azure Portal: where you see what is actually running

<div class="w-[40%] text-base">

- **Resources**: NSGs, private endpoints, Service Bus, container apps, Front Door and the Log Analytics workspace, all in one place
- **Looking and debugging**: view log stream, revisions, NSG rules and settings
- **Approving**: this is where you approve private endpoint connections from Front Door and the registry
- **Creating** is not done here but in Terraform

</div>

<img src="/azure-portal.png" class="absolute right-8 bottom-8 w-[34rem] rounded-xl shadow-xl" />

<!--
The Azure Portal (portal.azure.com) is the regular Azure management interface, next to the BMW Manage Portal from the previous slide. You use the Manage Portal to request things the platform team takes care of (VNet, DNS, endpoints). You use the Azure Portal to see what is actually running: on the start page the Recent list shows our NSG, the container registry, the private endpoint and Service Bus, the container apps (portal-service, external-notification-service), the Log Analytics workspace and Front Door with the WAF policy. For us it is mostly a place to look and debug: log stream, revisions, NSG rules. During setup we also had to approve private endpoint connections (Front Door to the environment, the registry for the web app), otherwise Front Door returns a 502. We prefer creating resources with Terraform rather than clicking.
-->

---

# Hub and spoke: our VNet is not an island

<div class="w-[52%] text-base">

- Our VNet is a **spoke**: peered with a central BMW **firewall hub**

</div>

<img src="/en/hub-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Explaining the terms: Hub-spoke is a way of setting up networks in Azure. The hub is a central VNet with shared facilities: at BMW the firewall, the VPN connection to the corporate network and the access to the internet. A spoke is the VNet of a team or application, like ours, containing only its own resources. Peering is the link between two VNets: they are privately connected to each other, without the internet. Advantage: security and connectivity live in one place and no spoke needs its own firewall or VPN. Comparison: the hub is the head office with a gate and reception, the spokes are the departments.

Hub and spoke: Our VNet is a spoke and peered with the new BMW firewall hub (Gen2). The old hub had proxies without authentication and shared services were not in the hub, which is why a new firewall hub was built. You check the peering with az network vnet peering list.
-->

---

# Hub and spoke: our VNet is not an island

<div class="w-[52%] text-base">

- <span class="opacity-50">Our VNet is a **spoke**: peered with a central BMW **firewall hub**</span>
- Every subnet has a route table: `0.0.0.0/0` goes to the hub firewall. Everything heading out passes the doorman 🛂

</div>

<img src="/en/hub-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Route table: Every subnet has a firewall route table attached with a single default user-defined route: 0.0.0.0/0 to the hub firewall. All traffic leaving the VNet therefore passes the firewall first.
-->

---

# Hub and spoke: our VNet is not an island

<div class="w-[52%] text-base">

- <span class="opacity-50">Our VNet is a **spoke**: peered with a central BMW **firewall hub**</span>
- <span class="opacity-50">Every subnet has a route table: `0.0.0.0/0` goes to the hub firewall. Everything heading out passes the doorman 🛂</span>
- **Azure services** (service tags of the regions) are open by default

</div>

<img src="/en/hub-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Open by default: In the hub firewall, the service tags of the Azure regions where BMW offers hubs (and the geo-redundant failover regions) are whitelisted by default, plus a list of destinations allowed by default.
-->

---

# Hub and spoke: our VNet is not an island

<div class="w-[52%] text-base">

- <span class="opacity-50">Our VNet is a **spoke**: peered with a central BMW **firewall hub**</span>
- <span class="opacity-50">Every subnet has a route table: `0.0.0.0/0` goes to the hub firewall. Everything heading out passes the doorman 🛂</span>
- <span class="opacity-50">**Azure services** (service tags of the regions) are open by default</span>
- The rest is closed. Request extra URLs via the Manage Portal, get them approved in Tufin by four eyes, then wait ±15 minutes ⏳

</div>

<img src="/en/hub-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Whitelisting: You request extra destinations in the Manage Portal (Network, Add URL Whitelisting). URLs only, no IPs or IP ranges. Ports 80 and 443 by default. Requester and approver may not be the same person (four-eyes principle), approval happens in Tufin and the status is only updated every quarter of an hour. What the firewall blocks can be retrieved with a URL blocking report. Source: BMW developer docs, Gen2 Hub concept.
-->

---

# Azure glossary 2: the NSG

<div class="w-[52%] text-base">

- **NSG**: the bouncer of your subnet. A list of rules: who gets in and who gets out
- **Set per rule**: direction, source, destination, port, protocol, allow or deny
- **Priority**: lowest number wins. Without a match the default rules apply
- Filters on IP and port, not on URL. For URLs you have the firewall in the hub
- **In our case**: one NSG per subnet via a BMW Terraform module. Container apps: inbound 80, 443 and 8080, outbound only what's needed

</div>

<img src="/en/term-nsg.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
NSG (Network Security Group): A list of firewall rules that you attach to a subnet or a network card. Every rule has a priority (lowest number wins), direction (inbound or outbound), source, destination, port, protocol and action (allow or deny). Source and destination can be IP ranges, but also service tags (such as Internet, VirtualNetwork or AzureLoadBalancer) or application security groups. Azure adds default rules: traffic within the VNet is allowed, the Azure load balancer is allowed, inbound from the internet is closed, and outbound is open. An NSG is stateful: the reply to an allowed request is automatically allowed back. An NSG only filters on layers 3 and 4 (IP and port), not on URL. In our setup Terraform (network-security.tf, BMW NSG module) creates one NSG per subnet: private-endpoints, database, service-bus, container-apps, storage and web-app. The container-apps subnet has inbound 80, 443 and 8080 (priority 190). Outbound is open for: Azure Monitor (443), HTTP/HTTPS (80, 443), NTP (UDP 123) and the Container Apps control plane (5671 and 5672). Two rules are still called AllowAKSControlPlane (UDP 1194, TCP 9000): a leftover from the AKS days.
-->

---

# Azure glossary 3: route table and Tufin

<div class="w-[52%] text-base">

- **Route table** (UDR): the road sign. Everything heading out (`0.0.0.0/0`) has to pass the firewall first
- **Tufin**: the counter for firewall rules. Expect a wait ⏳

</div>

<img src="/en/term-nsg.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Route table and Tufin: Outbound traffic goes through a route table to the hub firewall. If you want to reach the outside (e.g. an external URL), a firewall rule has to be requested via Tufin.
-->

---

# Azure glossary 4: private endpoint

<div class="w-[52%] text-base">

- **Private endpoint**: your own back door to an Azure service, with a **private IP** in your subnet
- Key Vault, Service Bus, Storage and Registry come off the internet. Only you hold the key 🔑
- **Private Link**: the technology behind it. Front Door also uses it to reach our environment
- **In our case**: an endpoint only gets a name once you click "Create Missing" in the BMW Manage Portal

</div>

<img src="/en/term-pe.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Private endpoint: A door to a PaaS service that only exists inside the VNet. You close the service itself to the public internet. In the BMW Manage Portal, under Manage Private Endpoint DNS Records, you have to have the DNS records created per endpoint (Create or Create Missing).
-->

---

# Azure glossary 5: private DNS zone

<div class="w-[52%] text-base">

- **DNS**: the phone book of the internet
- **Private DNS zone**: your own secret phone book, only readable inside your VNet
- **Container App Environment**: its own zone with a wildcard A record (`*`) pointing to the internal IP
- **In our case**: Terraform creates the zone, but only after *onboarding* in the BMW Manage Portal does it resolve from the intranet. So much for "it works on my machine" 😅

</div>

<img src="/en/term-dns.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Private DNS zone: Without DNS nobody knows which IP belongs to portal-service.<default-domain>. The zone for the environment has a wildcard record pointing to the static IP. In addition we have a zone for the registry (privatelink.azurecr.io), the database and the intranet site 360fp-<env>.azure.cloud.bmw, which we create in the BMW Manage Portal.
-->

---

# Ingress: two knobs, one question: who gets in?

<div class="w-[52%] text-base">

- **Environment**: internal load balancer on = only reachable from the VNet, with its own static IP
- **Container app**: its own ingress with port, HTTPS-only and traffic weights per revision
- `external_enabled = true` sounds like "internet", but in an internal environment it means "everyone in the VNet". Yes, really 🤯
- **In our case**: environment internal. The internet comes in via Front Door, colleagues via VPN and private DNS

</div>

<img src="/en/term-ingress.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Ingress: Two layers. The environment determines whether there is an internal or public load balancer, and in which VNet it sits. The container app has its own ingress block with target port 8080, allow_insecure off and traffic weights for revisions. In our case the environment is internal (internal_load_balancer_enabled) and the apps have ingress enabled. Internet users come in via Front Door with Private Link, colleagues via VPN and the private DNS zone.
-->

---

# Infrastructure as Code, except the part where you have to click

<div class="w-[52%] text-base">

- **Portal** 🖱️: request the VNet, service principal, temporary roles and DNS zones
- **Terraform** ⌨️: subnets, NSGs, environment, apps and private endpoints
- **Portal** 🖱️ (again): onboard DNS zones and create endpoint records
- **Tufin** ⏳: request firewall rules and wait

</div>

<img src="/en/term-pingpong.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Back and forth: Not everything can be done in code. The VNet, the service principal (with JIT Owner role) and the DNS zones go through the BMW Manage Portal. Terraform does the rest, but halfway through the apply, private DNS zones and private endpoint records have to be done by hand in the portal again. So it went back and forth a few times.
-->

---
layout: center
class: text-center
---

# Our infra repo: what is in it?

<img src="/en/infra-repo.svg" class="mx-auto mt-4 h-[26rem] rounded-xl shadow-xl" />

<!--
The infra repo has three parts. Deployment: one Jenkins pipeline per environment (TEST, INT, PROD) plus one for the shared parts. azure-terraform-shared: what all environments share, such as the container registry, a key vault, a Log Analytics workspace and alerts. azure-terraform-environments: one codebase for all environments with around 4,600 lines of Terraform, split by topic (network, container apps, Front Door, data, security, monitoring). Per environment only the tfvars file and the config/ folder with nginx and service configuration differ. The README describes the full setup step by step, including the manual steps in the Manage Portal and Tufin.
-->

---
layout: center
class: text-center
---

# The app: an App Service

<img src="/en/app-titel.svg" class="mx-auto mt-8 h-72 rounded-xl shadow-xl" />

<!--
Besides the container apps, we cover the frontend.
-->

---

# App Service: Azure's well-behaved nginx server

<div class="w-[52%] text-base">

- Managed web hosting platform: you supply code or a container, Azure does the boring work (OS, patches, scaling)

</div>

<img src="/en/app-service-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
App Service is a PaaS for web apps: you supply code or a container, Azure manages the operating system, the patches and the scaling. For us it is where the frontend runs, an nginx container (web-app.tf). It is the 'boring' counterpart of the container apps: fewer knobs, fewer surprises.
-->

---

# App Service: Azure's well-behaved nginx server

<div class="w-[52%] text-base">

- <span class="opacity-50">Managed web hosting platform: you supply code or a container, Azure does the boring work (OS, patches, scaling)</span>
- **VNet integration**: the app can reach out into the VNet, so also to all private services

</div>

<img src="/en/app-service-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
VNet integration handles the outbound traffic: the App Service gets a foot in our VNet and can thereby reach private services such as the container apps, the storage and the registry. In our setup vnet_route_all_enabled is on, so all outbound traffic goes through the VNet. Note: this says nothing about inbound traffic, that is the private endpoint on the next slide. Because of the corporate DNS we also use WEBSITE_DNS_SERVER 168.63.129.16, otherwise the private zones don't resolve.
-->

---

# App Service: Azure's well-behaved nginx server

<div class="w-[52%] text-base">

- <span class="opacity-50">Managed web hosting platform: you bring code or a container, Azure does the boring work (OS, patches, scaling)</span>
- <span class="opacity-50">**VNet integration**: the app can reach out into the VNet, and so to all private services</span>
- **Private endpoint**: inbound only via a private IP. The internet stays outside 🚪

</div>

<img src="/en/app-service-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
The private endpoint handles inbound traffic.

What it is: a network card with a private IP address in our own VNet (subnet for private endpoints), connected to the App Service. From the network's point of view, the web app is simply a machine at an internal address.

Why: by default an App Service has a public address. We set public_network_access_enabled=false, so nobody gets in from the internet. The only way in is the private IP.

Who does get in:
- Front Door via Private Link: Front Door gets its own, approved connection to the web app (front-door.tf, private_link to the web app). That way internet users reach the app without the App Service itself being public.
- Intranet users via VPN or the corporate network, through the hub-spoke network to the private IP.

DNS: the name of the web app must point to that private IP, otherwise it resolves to the public address and the connection is refused. That is what the private DNS zone does (see the glossary slide).

Note: VNet integration (outbound, previous slide) and private endpoint (inbound) are two separate things. You usually need both, and it is a common mistake to assume that one covers the other.
-->

---

# App Service: Azure's well-behaved nginx server

<div class="w-[52%] text-base">

- <span class="opacity-50">Managed web hosting platform: you bring code or a container, Azure does the boring work (OS, patches, scaling)</span>
- <span class="opacity-50">**VNet integration**: the app can reach out into the VNet, and so to all private services</span>
- <span class="opacity-50">**Private endpoint**: inbound only via a private IP. The internet stays outside 🚪</span>
- Custom domain, certificates and container registry pull via the VNet

</div>

<img src="/en/app-service-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
Custom domain and certificate: for us the certificate comes from Key Vault and is attached to the App Service (azurerm_app_service_certificate and binding), so the intranet domain name works with HTTPS. We pull the container image from the shared ACR (in another subscription) via its own private endpoint on the registry, using the web app's user-assigned managed identity. So no public internet, no password.
-->

---

# App Service: Azure's well-behaved nginx server

<div class="w-[52%] text-base">

- <span class="opacity-50">Managed web hosting platform: you bring code or a container, Azure does the boring work (OS, patches, scaling)</span>
- <span class="opacity-50">**VNet integration**: the app can reach out into the VNet, and so to all private services</span>
- <span class="opacity-50">**Private endpoint**: inbound only via a private IP. The internet stays outside 🚪</span>
- <span class="opacity-50">Custom domain, certificates and container registry pull via the VNet</span>
- **In our case**: nginx container with the Angular app, content and nginx config via Blob Storage

</div>

<img src="/en/app-service-5.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
In our case: an nginx container with the Angular app. The frontend files and the nginx configuration live in Blob Storage and are mounted into the container (/usr/share/nginx/html and /etc/nginx/nginx-location). Rolling out a new version therefore means putting files in the storage, without building a new image. Nginx also proxies the API calls to the container apps (see the Front Door slides).
-->

---

# The DNS hack: a public CNAME for an internal name 🕵️

<div class="w-[52%] text-base">

- A custom domain on an App Service requires **domain validation** in *public* DNS

</div>

<img src="/en/app-dns-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
This was the trickiest step: App Service wants proof of ownership of the domain. That is only possible via TXT and CNAME in public DNS, while our app is completely private. You cannot create these records yourself: they can only be requested via an ITSM service request. Afterwards we bind the domain and the certificate from the Key Vault (azurerm_app_service_certificate_binding). See README step 23.
-->

---

# The DNS hack: a public CNAME for an internal name 🕵️

<div class="w-[52%] text-base">

- <span class="opacity-50">A custom domain on an App Service requires **domain validation** in *public* DNS</span>
- So: a **TXT** (`asuid.…`) and a **CNAME** to `…azurewebsites.net` in the public zone `cloud.bmw`

</div>

<img src="/en/app-dns-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
This was the trickiest step: App Service wants proof of ownership of the domain. That is only possible via TXT and CNAME in public DNS, while our app is completely private. You cannot create these records yourself: they can only be requested via an ITSM service request. Afterwards we bind the domain and the certificate from the Key Vault (azurerm_app_service_certificate_binding). See README step 23.
-->

---

# The DNS hack: a public CNAME for an internal name 🕵️

<div class="w-[52%] text-base">

- <span class="opacity-50">A custom domain on an App Service requires **domain validation** in *public* DNS</span>
- <span class="opacity-50">So: a **TXT** (`asuid.…`) and a **CNAME** to `…azurewebsites.net` in the public zone `cloud.bmw`</span>
- For our internal address `360fp-<env>.azure.cloud.bmw`, the public CNAME therefore had to be changed

</div>

<img src="/en/app-dns-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
This was the trickiest step: App Service wants proof of ownership of the domain. That is only possible via TXT and CNAME in public DNS, while our app is completely private. You cannot create these records yourself: they can only be requested via an ITSM service request. Afterwards we bind the domain and the certificate from the Key Vault (azurerm_app_service_certificate_binding). See README step 23.
-->

---

# The DNS hack: a public CNAME for an internal name 🕵️

<div class="w-[52%] text-base">

- <span class="opacity-50">A custom domain on an App Service requires **domain validation** in *public* DNS</span>
- <span class="opacity-50">So: a **TXT** (`asuid.…`) and a **CNAME** to `…azurewebsites.net` in the public zone `cloud.bmw`</span>
- <span class="opacity-50">For our internal address `360fp-<env>.azure.cloud.bmw`, the public CNAME therefore had to be changed</span>
- The real traffic then flows via the **private DNS zone** and the private endpoint

</div>

<img src="/en/app-dns-4.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
This was the trickiest step: App Service wants proof of ownership of the domain. That is only possible via TXT and CNAME in public DNS, while our app is completely private. You cannot create these records yourself: they can only be requested via an ITSM service request. Afterwards we bind the domain and the certificate from the Key Vault (azurerm_app_service_certificate_binding). See README step 23.
-->

---

# Front Door + nginx: the traffic controllers 🚦

<div class="w-2/5 text-base">

- Publicly, the application runs via **Front Door**

</div>

<img src="/en/app-routes-1.svg" class="absolute right-8 bottom-12 w-[34rem] rounded-xl" />

<!--
front-door.tf contains the origin groups and routes: web_app for / and /portal-app, and separate routes for portal-service, notification-service, external-notification-service and translation-service. Internally, nginx (config/<env>/web-app/nginx-location.conf) uses proxy_pass to the container apps. The App Service has VNet integration with vnet_route_all, so it reaches the internal environment.
-->

---

# Front Door + nginx: the traffic controllers 🚦

<div class="w-2/5 text-base">

- <span class="opacity-50">Publicly, the application runs via **Front Door**</span>
- Routes: `/` and `/portal-app/*` to the App Service, `/<service>/*` to the container apps

</div>

<img src="/en/app-routes-2.svg" class="absolute right-8 bottom-12 w-[34rem] rounded-xl" />

<!--
front-door.tf contains the origin groups and routes: web_app for / and /portal-app, and separate routes for portal-service, notification-service, external-notification-service and translation-service. Internally, nginx (config/<env>/web-app/nginx-location.conf) uses proxy_pass to the container apps. The App Service has VNet integration with vnet_route_all, so it reaches the internal environment.
-->

---

# Front Door + nginx: the traffic controllers 🚦

<div class="w-2/5 text-base">

- <span class="opacity-50">Publicly, the application runs via **Front Door**</span>
- <span class="opacity-50">Routes: `/` and `/portal-app/*` to the App Service, `/<service>/*` to the container apps</span>
- Front Door uses Private Link to both

</div>

<img src="/en/app-routes-3.svg" class="absolute right-8 bottom-12 w-[34rem] rounded-xl" />

<!--
front-door.tf contains the origin groups and routes: web_app for / and /portal-app, and separate routes for portal-service, notification-service, external-notification-service and translation-service. Internally, nginx (config/<env>/web-app/nginx-location.conf) uses proxy_pass to the container apps. The App Service has VNet integration with vnet_route_all, so it reaches the internal environment.
-->

---

# Front Door + nginx: the traffic controllers 🚦

<div class="w-2/5 text-base">

- <span class="opacity-50">Publicly, the application runs via **Front Door**</span>
- <span class="opacity-50">Routes: `/` and `/portal-app/*` to the App Service, `/<service>/*` to the container apps</span>
- <span class="opacity-50">Front Door uses Private Link to both</span>
- **nginx** on the App Service proxies API calls to the container apps.

</div>

<img src="/en/app-routes-4.svg" class="absolute right-8 bottom-12 w-[34rem] rounded-xl" />

<!--
front-door.tf contains the origin groups and routes: web_app for / and /portal-app, and separate routes for portal-service, notification-service, external-notification-service and translation-service. Internally, nginx (config/<env>/web-app/nginx-location.conf) uses proxy_pass to the container apps. The App Service has VNet integration with vnet_route_all, so it reaches the internal environment.
-->

---
layout: center
class: text-center
---

# Logging and monitoring

<img src="/en/ops-overwerkt.svg" class="mx-auto mt-8 h-72 rounded-xl shadow-xl" />

<!--
Next problem: how do you find out what your apps are doing?
-->

---

# Log Analytics and log stream: "What is the app saying right now?"

<div class="w-3/5">

- Log Analytics: first learn KQL, then wait 5 minutes for your log line 😴

</div>

<img src="/en/mon-pijn-1.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
The built-in Azure solution did not work well for us. Log Analytics requires KQL knowledge and has ingestion delay, and the log stream is live-only. For a Java developer who wants to track down an error quickly, that is too cumbersome.
-->

---

# Log Analytics and log stream: "What is the app saying right now?"

<div class="w-3/5">

- <span class="opacity-50">Log Analytics: first learn KQL, then wait 5 minutes for your log line 😴</span>
- Log stream: blink and you miss the error. No history, no search

</div>

<img src="/en/mon-pijn-2.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
The built-in Azure solution did not work well for us. Log Analytics requires KQL knowledge and has ingestion delay, and the log stream is live-only. For a Java developer who wants to track down an error quickly, that is too cumbersome.
-->

---

# Log Analytics and log stream: "What is the app saying right now?"

<div class="w-3/5">

- <span class="opacity-50">Log Analytics: first learn KQL, then wait 5 minutes for your log line 😴</span>
- <span class="opacity-50">Log stream: blink and you miss the error. No history, no search</span>
- Logs, traces and metrics: three places, three screens, zero overview

</div>

<img src="/en/mon-pijn-3.svg" class="absolute right-12 bottom-16 w-96 rounded-xl" />

<!--
The built-in Azure solution did not work well for us. Log Analytics requires KQL knowledge and has ingestion delay, and the log stream is live-only. For a Java developer who wants to track down an error quickly, that is too cumbersome.
-->

---

# Our alternative: the Grafana stack 🔥

<div class="w-[58%] text-lg">

- **Alloy**: collector, receives OpenTelemetry and scrapes `/q/metrics`

</div>

<img src="/en/mon-stack-1.svg" class="absolute right-8 bottom-12 w-[26rem] rounded-xl" />

<!--
We host Grafana, Loki, Tempo and Mimir ourselves as container apps via Terraform (monitoring.tf). The Quarkus apps send logs and traces to Alloy (http://alloy) via the Quarkus OpenTelemetry extension. Alloy routes traces to Tempo and logs to Loki, and scrapes the Quarkus metrics every 30 seconds into Mimir. Storage is Azure Blob Storage with 31 days of retention. A Quarkus dashboard is ready in Grafana. Note: in the Terraform, the OTEL variables for some services are still commented out until the dependencies are right.
-->

---

# Our alternative: the Grafana stack 🔥

<div class="w-[58%] text-lg">

- <span class="opacity-50">**Alloy**: collector, receives OpenTelemetry and scrapes `/q/metrics`</span>
- **Loki**: logs, **Tempo**: traces, **Mimir**: metrics

</div>

<img src="/en/mon-stack-2.svg" class="absolute right-8 bottom-12 w-[26rem] rounded-xl" />

<!--
We host Grafana, Loki, Tempo and Mimir ourselves as container apps via Terraform (monitoring.tf). The Quarkus apps send logs and traces to Alloy (http://alloy) via the Quarkus OpenTelemetry extension. Alloy routes traces to Tempo and logs to Loki, and scrapes the Quarkus metrics every 30 seconds into Mimir. Storage is Azure Blob Storage with 31 days of retention. A Quarkus dashboard is ready in Grafana. Note: in the Terraform, the OTEL variables for some services are still commented out until the dependencies are right.
-->

---

# Our alternative: the Grafana stack 🔥

<div class="w-[58%] text-lg">

- <span class="opacity-50">**Alloy**: collector, receives OpenTelemetry and scrapes `/q/metrics`</span>
- <span class="opacity-50">**Loki**: logs, **Tempo**: traces, **Mimir**: metrics</span>
- **Grafana**: dashboards and search in one place. Finally 😍

</div>

<img src="/en/mon-stack-3.svg" class="absolute right-8 bottom-12 w-[26rem] rounded-xl" />

<!--
We host Grafana, Loki, Tempo and Mimir ourselves as container apps via Terraform (monitoring.tf). The Quarkus apps send logs and traces to Alloy (http://alloy) via the Quarkus OpenTelemetry extension. Alloy routes traces to Tempo and logs to Loki, and scrapes the Quarkus metrics every 30 seconds into Mimir. Storage is Azure Blob Storage with 31 days of retention. A Quarkus dashboard is ready in Grafana. Note: in the Terraform, the OTEL variables for some services are still commented out until the dependencies are right.
-->

---

# Our alternative: the Grafana stack 🔥

<div class="w-[58%] text-lg">

- <span class="opacity-50">**Alloy**: collector, receives OpenTelemetry and scrapes `/q/metrics`</span>
- <span class="opacity-50">**Loki**: logs, **Tempo**: traces, **Mimir**: metrics</span>
- <span class="opacity-50">**Grafana**: dashboards and search in one place. Finally 😍</span>
- Everything runs as container apps in the same environment. Own data in Blob Storage, kept for 31 days

</div>

<img src="/en/mon-stack-4.svg" class="absolute right-8 bottom-12 w-[26rem] rounded-xl" />

<!--
We host Grafana, Loki, Tempo and Mimir ourselves as container apps via Terraform (monitoring.tf). The Quarkus apps send logs and traces to Alloy (http://alloy) via the Quarkus OpenTelemetry extension. Alloy routes traces to Tempo and logs to Loki, and scrapes the Quarkus metrics every 30 seconds into Mimir. Storage is Azure Blob Storage with 31 days of retention. A Quarkus dashboard is ready in Grafana. Note: in the Terraform, the OTEL variables for some services are still commented out until the dependencies are right.
-->

---
layout: center
class: text-center
---

# What did we end up with?

<img src="/en/aca-landschap.svg" class="mx-auto mt-4 h-[26rem] rounded-xl shadow-xl" />

<!--
Overview: Front Door with WAF and Private Link for the internet, VPN and firewall hub for the intranet, a spoke VNet with a subnet and NSG per service, private endpoints for Service Bus, Storage, Key Vault and registry, and private DNS zones.
-->

---

# What do we take away?

<div class="w-[85%] text-lg">

- Your application is only part of the story. The rest is **network, security and operations**
- Talk early to the people who manage the network and the firewalls. They hold the keys
- Capture everything as Infrastructure as Code, even the places where you would rather click
- "Serverless" doesn't mean there are no servers, but that they're no longer your problem. The network still is

</div>

<!--
Wrap-up. Key message: deploying to the cloud is more than starting a container. For developers, the surprise is usually in the network and security, not in the application itself.
-->

---

# Cheat sheet for the pub 🍺

<div class="text-sm leading-tight">

| Term | Meaning |
|---|---|
| VNet / subnet | Your own network in Azure and a part of it |
| NSG | Firewall at subnet or network card level |
| Route table (UDR) | Determines which route traffic takes |
| Private endpoint | Private IP in our subnet to an Azure service |
| Private Link | The technology behind private endpoints |
| Private DNS zone | Name to IP, only within linked VNets |
| Container App Environment | Boundary around apps: VNet, load balancer, logs |
| Ingress | Settings for inbound traffic |

</div>

<!--
Cheat sheet to look back at during the Q&A.
-->
