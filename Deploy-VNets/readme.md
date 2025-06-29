### OVERVIEW
Consider the fictional organization Contoso Ltd, which is in the process of migrating infrastructure and applications to Azure. Imagining that my role entails planning and implementing three virtual networks and subnets to support resources in those virtual networks.

- The CoreServicesVnet virtual network is deployed in the East US region. This virtual network will have the largest number of resources. It will have connectivity to on-premises networks through a VPN connection. This network will have web services, databases, and other systems that are key to the operations of the business. Shared services, such as domain controllers and DNS also will be located here. A large amount of growth is anticipated, so a large address space is necessary for this virtual network.

- The ManufacturingVnet virtual network is deployed in the West Europe region, near the location of your organization’s manufacturing facilities. This virtual network will contain systems for the operations of the manufacturing facilities. The organization is anticipating a large number of internal connected devices for their systems to retrieve data from, such as temperature, and will need an IP address space that it can expand into.

- The ResearchVnet virtual network is deployed in the Southeast Asia region, near the location of the organization’s research and development team. The research and development team uses this virtual network. The team has a small, stable set of resources that is not expected to grow. The team needs a small number of IP addresses for a few virtual machines for their work.

![image](https://github.com/user-attachments/assets/0b6f8ec0-b323-47a4-b06c-388426b4b38c)

Let's get to work:
![image](https://github.com/user-attachments/assets/52e71e86-fdf9-4027-bcc3-077b5f0004d7)

