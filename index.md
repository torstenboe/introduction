In recent years cloud has become a very popular and evolved into an alternative to traditional data center. An increasing number of enterprises outsource their infrastructure to a cloud provider. Adopting a service delivery framework on top of a cloud controller helps IT departments to keppt their services aligned with the needs of the business. Usually, business value is measured in reduced cost, decreased time-to-market or increased revenue. Cloud infrastructure platforms increase operational efficiency, expedite the development of new services and prevents  investment risks. Global infrastructure pools provide access to resources on-deamnd. But while launching a cloud server has become a matter of pressing a button, building a hosting platform for intra- and extranet services remains challenging. Operators need to comply with security and compliance guidelines before provisioning the first server. This framework accelerates this process. Operators start with default configuration and refine their platform in small incremental steps. ["Infrastructure as Code" (IaC)](https://en.wikipedia.org/wiki/Infrastructure_as_code) is employed to extract configuration parameters from service assets and to inherit operator controls during the provisioning process. [Oracle's Resource Manager (ORM)](https://docs.oracle.com/en-us/iaas/Content/ResourceManager/Concepts/resourcemanager.htm) is used to generate graphical user interfaces and REST-API from code variables. This enables operations engineers to build templates that trigger provisioning processes with a service deployment. Service manager benefit from on-demand infrastructure, service operators keep control and effectuate security and comliance guidlines. 

### Architecture

The ocloud framework supports the adoption of [Oracle Cloud Infrastructure (OCI)](https://www.oracle.com/cloud) with a baseline configuration for service deployments. Rather than separating service design and infrastructure automation, service configurations include provisioning instructions for service assets. The framework defines a [service resident](assets/resident) isolating service assets in a network segment and creates administrator domains to reflect operational responsibilities. An administrator domain combines OCI resources like compartments, groups, policies, identity tags and notifications into a reusable asset that separates system operators from network-, database-, and security operators.

![Baseline Configuration](https://raw.githubusercontent.com/ocilabs/images/main/base_config.drawio.png)

A segment represents various network ressources in OCI. It combines a Virtual Cloud Network (VCN) with a dynamic routing gateway (DRG), a service gateway for the Oracle Service Network, an internet- and a NAT gateway. It contains an adjustable number of subnets, defined using Terraform's  [cidrsubnet()](https://www.terraform.io/language/functions/cidrsubnet) function and secures IP spaces through firewalls. A firewall represents port filter, implemented using security lists. In the default configuration egress traffic is unconstrained, ingress traffic is limited to a minimum set of application profiles and will need adjustments, launching a service. Routing destinations trigger the definition of route rules. The DRG is obligatory and proiveds on-prem connectivity as an edge router. Adding network segments, additional routers can be defined and associated with VCN, naming both equally. Internet and NAT gateways are activated per default, but can be deactivated in the resource manager interface. 

### Recommendations
Provisioning templates extract configuration parameter from the deployment code for service assets. This enables operators to start with a recommendet set of resources and refine the configuration in incremental steps. The state-aware provisioning suggests an iterative approach. A SCRUM team is made up of system administrators with expertise in application management, network-, database- and security operations. They can adjust settings without learning HCL or Terraform. Service configurations are captured in JSON files and can be edited without touching the deployment code.

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ocilabs/introduction/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
