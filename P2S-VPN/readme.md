# Azure Point to Site VPN
### Description: Configuring Point to Site VPN Connection using Azure Certificate Authentication

New to deployment of virtual networks? check it out here: https://github.com/MaryBamisile/Azure-Networking/tree/main/Deploy-VNets

- I created a new Vnet and also GatewaySubnet as seen below:
  
<img width="1115" height="398" alt="image" src="https://github.com/user-attachments/assets/e362245e-fd7d-4c48-92b3-4de0aebb7675" />

  ##### Note: Ensure you use the subnet purpose specified for 'virtual network gateway'
- Go to Virtual Network Gateways > Create.
- Set the parameters
- Click Review + Create.

<img width="394" height="438" alt="image" src="https://github.com/user-attachments/assets/1206268f-3383-4472-b0b6-c0bfeb4d7006" />

Creation takes time, between 20 - 30mins....one of the longest azure service that takes long time to create

Afer creating, go to the created resource and configure

<img width="275" height="203" alt="image" src="https://github.com/user-attachments/assets/efe2dce9-f6cb-46e9-92fa-ab46bd04dbab" />


Conituation of VPN confirguration........

Now, download Azure VPN client and also the VPN configuration file

On the Azure VPM client application, click + and import and save

<img width="678" height="691" alt="image" src="https://github.com/user-attachments/assets/478855d1-1737-49f6-972c-b738bc792173" />

Now, it is connected:

<img width="473" height="595" alt="image" src="https://github.com/user-attachments/assets/c1cff780-63cc-40e5-a99e-cb681cd3a211" />


## Creation of Virtual Machine

<img width="582" height="458" alt="image" src="https://github.com/user-attachments/assets/59802538-4246-4bf3-8b18-1c416f996658" />
