With a new generation of cloud infrastructure, hyperscale offerings have taken a leap and become managed infrastructure platforms, capable of running enterprise workloads. While launching a cloud server is a matter of pressing a button, building a hosting platform for intra- and extranet services, operators need to comply with strict security and compliance guidelines. [Oracle Cloud Infrastructure (OCI)](https://www.oracle.com/cloud/) addresses the need for more control with dedicated resources and isolated networks. But while OCI allows operators to increase operational efficiency with ["Infrastructure as Code" (IaC)](https://developer.oracle.com/infrastructure-as-code), expedites the development of solutions with [predefined services](https://www.oracle.com/cloud/networking/service-gateway/service-gateway-supported-services) and prevents investment risks with [flexible pricing](https://docs.oracle.com/en/cloud/get-started/subscriptions-cloud/ocpib/billing-models-offered.html), a [continous architecture pracitice](https://continuousarchitecture.com/continuous-architecture-principles) is required to translate operational benefits to reduced cost, decreased time-to-market and increased revenue. This framework aims at IT departments looking for a service delivery that keeps services aligned with the needs of the business while functional and non-functional requirements evolve. Service residents enable "multi-tenant" deployments with shared services, service assets facilitate a modular approach and service configurations extracted from deployment plans allow for small, incremental development steps. The [Oracle’s Resource Manager (ORM)](https://docs.oracle.com/en-us/iaas/Content/ResourceManager/Concepts/resourcemanager.htm) is employed to generate graphical user interfaces and REST-API from code variables. This enables operations engineers to build self-service portals that trigger provisioning processes rather than launching infrastructure resources prior to adopting a CI/CD practice.

## Architecture

Architecture deals with transition planning activities that lead to the successful implementation of a cloud service. The architecture of a cloud service addresses security, scalability, performance, and resiliency requirements. It defines the guiding principles and standards for a services and helps to develop service assets. The framework enables architects to abstract at an appropriate level to make business and technical decisions, and to facilitate the analysis of platform capbilities and to compare design options. Meanwhile operations engineers focus on building reusable service assets, and system administrator provide the appropriate service configuration that will be merged into deployment plans. The framework provides a baseline configuration to support ad-hoc deployments, but separates service design and infrastructure automation with provisioning instructions for reusable assets. The framework defines a [service resident](assets/resident) isolating service assets in a network segment and creates administrator domains to reflect operational responsibilities. An administrator domain combines OCI resources like compartments, groups, policies, identity tags and notifications into a reusable asset that separates system operators from network-, database-, and security operators.

![Baseline Configuration](https://raw.githubusercontent.com/ocilabs/images/main/base_config.drawio.png)

A segment represents various network ressources in OCI. It combines a Virtual Cloud Network (VCN) with a dynamic routing gateway (DRG), a service gateway for the Oracle Service Network, an internet- and a NAT gateway. It contains an adjustable number of subnets, defined using Terraform’s [cidrsubnet()](https://www.terraform.io/language/functions/cidrsubnet) function and secures IP spaces through firewalls. A firewall represents port filter, implemented using security lists. In the default configuration egress traffic is unconstrained, ingress traffic is limited to a minimum set of application profiles and will need adjustments, launching a service. Routing destinations trigger the definition of route rules. The DRG is obligatory and proiveds on-prem connectivity as an edge router. Adding network segments, additional routers can be defined and associated with VCN, naming both equally. Internet and NAT gateways are activated per default, but can be deactivated in the resource manager interface.

## Recommendations

Continous architecture principles help operation engineers to develop a secure, resilient delivery platform for enterprise cloud services that is operated efficiently. Working in incremental, evolving steps towards a service design supports agile and DevOps methods that improve the quality and keep cost and complexity under control.

-   **Architect services, not solutions** A service delivers business value by facilitating the desired outcome without transfering the ownership for operational costs and risks to the user. Initially services are assembled using monitored and managed infrastructure components with PaaS and SaaS attachments. When functional requirements evolve, operators consider self-managed modules as replacement for public cloud services.

-   **Facilitate context-aware deployments** The way teams are organized drives the architecture and design of the systems they are working on. The commercial and the operational context for a service changes over time. Leveraging the “power of small” allows to move fast. In order to support a continuous delivery, tenancies are classified commercially and the lifecycle status is captured in the service configuration.

-   **Innovate without disruption** Cloud services are usually designed as blueprints that represent the entire footprint in a single automation script. Operational models for IT departments have incorporated shared infrastructure services in some shape or form. E.g. operational responsibilities for database or network infrastructure is reflected in a separation of duties. Extracting configuration parameters from the deployment code, the framework provides an automated provisioning process that collects input from existing competence center.

-   **Build services on a common foundation** Platforms evolve, there is no point in launching resources that may never be used. Modern CI/CD pipelines are developed around the capabilites of highly abstracted infrastrucure components. Reducing cost with agile development confilcts with optimizing cost for operated infrastructure. Dedicated resources have to be launched prior to supporting an on-demand model. With this framework, provisioning scripts for service assets ensure secure and compliant application deployments without prebuild resources. IaC libraries can be invoked to launch dedicated infrastructure together with a deployment script.

-   **Leverage network segmentation for hybrid services** Terms, like "cloud native" or "enterprise applications" describe the fact that different application designs rely on different orchestration capabilites, implemented on the resource layer. Different system requirements and communication pattern for host-, node- or container based services require a separation on network level. Rather than building hybrid clouds, the framework allows to build hybrid services that launch orchestrators in subnets of the network domain and eases the modernization of existing applications.

![Code Delivery](https://raw.githubusercontent.com/ocilabs/images/main/code_delivery.drawio.png)

Provisioning templates extract configuration parameter from the deployment code for service assets. This enables operators to start with a recommendet set of resources and refine the configuration in incremental steps. The state-aware provisioning suggests an iterative approach. A SCRUM team is made up of system administrators with expertise in application management, network-, database- and security operations. They can adjust settings without learning HCL or Terraform. Service configurations are captured in JSON files and can be edited without touching the deployment code.

## Considerations

The framework aims to provide more flexibility for infrastructure operators. Storing the code in a git repository and connecting the Resource Manager (ORM) with the source allows administrators to implement a [continuous delivery](https://en.wikipedia.org/wiki/Continuous_delivery) process. Operation engineers "borrow" tools and workflows, invented for software developer. But instead of using these tools for application code, operators leverage them for infrastructure automation. Git becomes the single source of truth for both, application and infrastructure code. A service template describes the desired state for the assets and is versioned in the repository. Operators introduce self-service following in three major steps:

### Service Residency

Default settings enable initial deployments for a new services. Operators evaluate the infrastructure platform, delivering a first service. System administrators and service owners explore service options that help to bootstrap operational tasks. [Oracle’s Resource Manager (ORM)](https://docs.oracle.com/en-us/iaas/Content/ResourceManager/Concepts/resourcemanager.htm) exposes assets through proteced user- and application interfaces, keeping service owners in charge to determine when and where a service will be launched. A schema file extends the console and facilitates variable entries in the `Create Stack` page. The default schema offers the following of options:

-   **Tenancy Classification**: The class parameter helps to constraint resource deployments according to the service limits associated with contract for a tenancy.

-   **Resident Configuration**: In the second section meta data assciated with a resident is collected. Names should be unique for a tenancy to avoid conflicts for resources, defined on root level. The lifecycle stage helps engineers to avoid over provisioning of resources that take a long time to be destroyed.

-   **Topology Options**: Executing the default deployment plan, the resource manager offers a choice of topologies that enable common use cases like enterprise applications, big-data and cloud-native services. These options can also be combined to build hybrid services. The opology selection triggers the subnet topology for a segment.

-   **Network Settings**: Generic network parameter trigger the level of exposure to the public internet. Activating the internet gateway will expose a frontend, and/or load balancer network to the public internet.

-   **Domain Protection**: The default protection setting is triggered by the lifecycle setting, however can be changed, should that be required.

### Toplogy Refinement

Default settings are stored in configuration files, containing parameters as lists of objects. Combining mulitple resources into assets, the number of resource arguments is significantly reduced. Dependencies are modelled referencing resource names. Subject matter experts refine these parameter to add, change or delete resources from a template. JSON files represent the design, scale and scope of a service. In the refinement phase teams collect input from practitioners to adjust the default parameter that allow operators to controll demand and optimize capacity utilization.

-   [Administrator Domains](https://github.com/ocilabs/default-configuration/blob/main/default/resident/domains.json) : Domains organize the stewardship for service assets like network, storage or compute. Domain names must be unique for a service resident.

-   [Administrator Roles](https://github.com/ocilabs/default-configuration/blob/main/default/resident/roles.json) : Roles reflect a series of policies to ensure a seprartion of duties between operators. Each role allows to manage administrator priviledges and policies independently.

-   [Operator Controls](https://github.com/ocilabs/default-configuration/blob/main/default/resident/controls.json) : Controls enable operators to constrain resource access and retrieve alarms or notifications in case of an event. Controls can also trigger scripts to apply predefined measures.

-   [Resource Tags](https://github.com/ocilabs/default-configuration/blob/main/default/resident/tags.json) : Resource tags identify groups of resources, enable cost tracking and allow to define cross-domain policies.

-   [Notification Channels](https://github.com/ocilabs/default-configuration/blob/main/default/resident/channels.json) : Channels utilize the messaging services for notifications generated by an event or an operator control like budget or service limits.

-   [Network Segments](https://github.com/ocilabs/default-configuration/blob/main/default/network/segments.json) : Segments provide private IP networks for a resident. OCI provides a native layer three network, tenancies can be considered as isolated, virtual data centers.

-   [Subnets](https://github.com/ocilabs/default-configuration/blob/main/default/network/subnets.json) : Subnets divide network segments into smaller parts. The purpose is to improve security and avoid address conflicts, when deploying autoscaling workloads.

-   [Edge Router](https://github.com/ocilabs/default-configuration/blob/main/default/network/routers.json) : Router are located at the cloud network boundary, the edge router represents an [DRG](https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/managingDRGs.htm) that connects network segments in the cloud with on-prem networks, allows for transit routing and for the implementation of a Hub-and-Spoke topology with multiple VCN.

-   [Routing Destinations](https://github.com/ocilabs/default-configuration/blob/main/default/network/routes.json) : Destinations translate the name of network zones into cidr ranges that can be reached using gateways. The route is defined as a pair between a destination and a gateway.

-   [Firewalls](https://github.com/ocilabs/default-configuration/blob/main/default/network/firewalls.json) : Firewalls represent port filter that either allow or block network packets based on their port number. The port.json files contains a list of predefined ports according to [RFC6335](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.txt) but can be extended with individual profiles.

-   [Zones](https://github.com/ocilabs/default-configuration/blob/main/default/network/destinations.json) : Security zones descirbe portions of a network with a security requirements set. Each zone consists of a single interface, to which a security policy is applied.

-   [Application Profiles](https://github.com/ocilabs/default-configuration/blob/main/default/network/ports.json) : Application Port Profiles include a combination of a protocol and a port, or a group of ports, that is used for firewalls and NAT gateways.

### Service Composition

The objective of every adoption project is the deployment of a service. Beside refining the topology, servers need to be configured and applications need to be installed. Configuration scripts are are triggerdd from a host configuration, and services hosted in the Oracle Service Network can be attached to a network segment. Cloud solutions are assembled using service assets. The framework provides predefined components that abstract provider specific APIs. Using ORM, services are deployed into existing residents. Predefined modules can be invoked referring to OCI modules in the [terraform registry](https://registry.terraform.io/browse/modules?provider=oci) or to a git repository, containing infrastructure code. A great starting point are the [cloudbricks](https://registry.terraform.io/search/modules?q=oci%20cloud%20bricks) components. Depending on the level of standardization, service components are introduced using the following methods:

-   **Service Assets** - Platform assets are defined as Terraform modules. Platform assets complement the initial set of resources, examples are hypervisors, container orchstrator or network appliances. Ideally, third party assets can be invoked using an own [Terraform provider](https://registry.terraform.io/browse/providers).

-   **Service Attachments** - The Oracle Service Network offers a variety of public cloud services that can be attached to a private service through the service gateway. Attachments don’t need customization, resource blocks can be added to the main.tf file.

-   **Service Modules** - Service Modules represent ORM modules with an own schema file. This allows to use the same modules accross multiple residents. Examples are application and database hosts or container cluster.

## Deployment

The resources manager comes with a number of [service provider](https://docs.oracle.com/en-us/iaas/Content/ResourceManager/Concepts/providers.htm) preinstalled, additional can be pulled form the [Terraform registry](https://registry.terraform.io/browse/providers), using the [provider block](https://www.terraform.io/docs/language/providers/configuration.html). The configuration module is the first out of three obligatory modules. It translates generic input paramerts into a baseline configuration. Operators adjust the service configuration when requirements evolve. For one-time deployments, the [Deploy to the Oracle Cloud](https://cloud.oracle.com/resourcemanager/stacks/create?zipUrl=https://github.com/oracle-devrel/terraform-oci-ocloud-landing-zone/archive/refs/heads/main.zip) button creates a zip archive that is pushed to the resource manager directly, to enable continuous changes the code should be cloned into a private repository and be connected as a source provider.

![Service Configuration](https://raw.githubusercontent.com/ocilabs/images/main/service_configuration.drawio.png)

An optional operator node is employed to execute cron jobs and runbooks that help to manage service availability, schedule resource consumption and fix problems for container workloads and functions. In addition service configurations enable service manager to adopt Oracle Cloud Services as alternative to shared intranet services and to benefit from [blueprints](https://github.com/oracle-quickstart) for services like utility computing, web- and mobile backbone services.

## Prerequisites

Code is written in HashiCorp Configuration Language (HCL), includes data stored in JSON format and cloud init scripts. The OCI Resource Manager executes Terraform and deploys Service Assets into a tenancy. Engineers should familerize themselfes with the following topics:

-   [Oracle Cloud Infrastructure (OCI) Account](https://www.oracle.com/cloud/free/)

-   [Oracle Resource Manager](https://docs.oracle.com/en-us/iaas/Content/ResourceManager/Concepts/resourcemanager.htm)

-   [HashiCorp Terraform](https://www.terraform.io)

-   [Terraform Service Provider for OCI](https://registry.terraform.io/providers/hashicorp/oci/latest)

-   [Terraform Time Service Provider](https://registry.terraform.io/providers/hashicorp/time/latest)

-   [Cloud Init](https://cloudinit.readthedocs.io/en/latest/)

## Notes/Issues

-   Destroying compartments and tag namespaces can take some time and will fail in some cases. Repeat the destroy command will continue the process.

## URLs

This repository is intended to be used with the Oracle Resource Manager. Using the "Deploy to Oracle Cloud" button requires users to [sign in](https://www.oracle.com/cloud/sign-in.html).

## Contributing

This project is a community project the code is open source. Please submit your contributions by forking this repository and submitting a pull request! Oracle appreciates any contributions that are made by the open source community.

## License

Copyright (c) 2021 Oracle and/or its affiliates.

Licensed under the Universal Permissive License (UPL), Version 1.0.

See [LICENSE](LICENSE) for more details.

ORACLE AND ITS AFFILIATES DO NOT PROVIDE ANY WARRANTY WHATSOEVER, EXPRESS OR IMPLIED, FOR ANY SOFTWARE, MATERIAL OR CONTENT OF ANY KIND CONTAINED OR PRODUCED WITHIN THIS REPOSITORY, AND IN PARTICULAR SPECIFICALLY DISCLAIM ANY AND ALL IMPLIED WARRANTIES OF TITLE, NON-INFRINGEMENT, MERCHANTABILITY, AND FITNESS FOR A PARTICULAR PURPOSE. FURTHERMORE, ORACLE AND ITS AFFILIATES DO NOT REPRESENT THAT ANY CUSTOMARY SECURITY REVIEW HAS BEEN PERFORMED WITH RESPECT TO ANY SOFTWARE, MATERIAL OR CONTENT CONTAINED OR PRODUCED WITHIN THIS REPOSITORY. IN ADDITION, AND WITHOUT LIMITING THE FOREGOING, THIRD PARTIES MAY HAVE POSTED SOFTWARE, MATERIAL OR CONTENT TO THIS REPOSITORY WITHOUT ANY REVIEW. USE AT YOUR OWN RISK.
