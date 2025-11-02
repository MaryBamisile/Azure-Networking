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

Creation takes time, between 20 - 30mins....one of the longest azure service that takes long time to create (you can jump to next part while waiting)


##### Generate a self-signed certificate. 
Open PowerShell as Admin and Run the command below 
```
#This will create a root cert and install it under the current user cert store.
$cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject “CN=SLP2SRootCert” -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation “Cert:\CurrentUser\My” -KeyUsageProperty Sign -KeyUsage CertSign
```

##### Generating Client Certificates from Root Certificate
```
Get-ChildItem -Path “Cert:\CurrentUser\My”
```

Take note of the Thumbrint and as it will required in the next command.

```$cert = Get-ChildItem -Path “Cert:\CurrentUser\My\THUMBPRINT”```

Run the following command to generate the client certificate
```
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject “CN=SLP2SClientCert” -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(1) `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation “Cert:\CurrentUser\My” `
-Signer $cert -TextExtension @(“2.5.29.37={text}1.3.6.1.5.5.7.3.2”)
```

Proceed to export the root certificate public key (.cer) so that we can use it to configure the P2S configuration

- Click Windows Key + R, to open the Run dialog box and type in “certmgr.msc”. 
- When the management console opens, goto “Current User\Personal\Certificates”.
- Right-click on the newly created cert and go to All Tasks > Export.
- Click Next >> Select No, do not export the private key, and then click Next.
- On the Export File Format page, select Base-64 encoded X.509 (.CER)., and then click Next.
- Browse to the location to which you want to export the certificate. Specify the file name.  Then, click Next >> Finish

<img width="457" height="350" alt="image" src="https://github.com/user-attachments/assets/a800996c-ac08-417d-b742-7c86a56402e7" />

<img width="717" height="722" alt="image" src="https://github.com/user-attachments/assets/87171a67-7665-46a7-9f2c-7d69d005ae07" />

<img width="753" height="750" alt="image" src="https://github.com/user-attachments/assets/6dee1b64-6638-4e29-b384-5d3b44008efa" />

<img width="1137" height="579" alt="image" src="https://github.com/user-attachments/assets/be5e6639-8b49-4f10-af5f-c9fa616e9c64" />


### Configuring our VPN client configurtation

- Go to the created Virtual network gateway resource and click on Point-to-site configuration
- Click on configure now

<img width="275" height="203" alt="image" src="https://github.com/user-attachments/assets/efe2dce9-f6cb-46e9-92fa-ab46bd04dbab" />

<img width="1240" height="779" alt="image" src="https://github.com/user-attachments/assets/763ade4d-f47f-4fa3-9124-8a38fe9d1235" />


###### Under root certificate name, type the cert name >> under public certificate data, paste the root certificate data (Open the cert in notepad to get this).

Note: when you paste certificate data, do not copy —–BEGIN CERTIFICATE—– & —–END CERTIFICATE—– text.
- Then click on Save to complete the process.

Now, download Azure VPN client and also the VPN configuration file

On the Azure VPM client application, click + and import and save

<img width="678" height="691" alt="image" src="https://github.com/user-attachments/assets/478855d1-1737-49f6-972c-b738bc792173" />

Now, it is connected:

<img width="473" height="595" alt="image" src="https://github.com/user-attachments/assets/c1cff780-63cc-40e5-a99e-cb681cd3a211" />


## Creation of Virtual Machine and connecting via RDP (using private IP)

<img width="582" height="458" alt="image" src="https://github.com/user-attachments/assets/59802538-4246-4bf3-8b18-1c416f996658" />

<img width="819" height="466" alt="image" src="https://github.com/user-attachments/assets/5af59f89-dc74-4173-bc8c-2fdbfdf2a84c" />

Yayyy, we are in

<img width="801" height="549" alt="image" src="https://github.com/user-attachments/assets/d33d1ea3-7ecf-47d4-b05e-1bceb5341260" />


### CURIOSITY Section:
I was curious how I'm able to access the internet from the VM withour having public IP.

##### Simple answer to that curiosity: The NSG associated with the VMs NIC/Subnet has the default Internet Outbound rule. The outbound internet connections will use a random IP belonging to Azure. (This IP might change depending upon availability)

- This is documented here : https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/default-outbound-access

#### Plot twist: After March 31, 2026, this method will no longer work without using NAT Gateway,

<img width="890" height="242" alt="image" src="https://github.com/user-attachments/assets/f0d9c188-9ffa-452d-8cef-5428eed2359a" />

#### Hence, start transitioning by adding an explicit outbound method:
- Associate a NAT gateway to the subnet of your virtual machine. Note this is the recommended method for most scenarios.
- Associate a standard load balancer configured with outbound rules.
- Associate a Standard public IP to any of the virtual machine's network interfaces.
- Add a Firewall or Network Virtual Appliance (NVA) to your virtual network and point traffic to it using a User Defined Route (UDR)


