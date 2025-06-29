## Overview
I will be configuring DNS name resolution for Contoso Ltd. You will create a private DNS zone named contoso.com, link the VNets for registration and resolution, and then create two virtual machines and test the configuration.

##### In this exercise, I will:

- Task 1: Create a private DNS Zone
- Task 2: Link subnet for auto registration
- Task 3: Create Virtual Machines to test the configuration
- Task 4: Verify records are present in the DNS zone

#### Task 1: Create a private DNS Zone
- Go to [Azure Portal](https://portal.azure.com).
- On the Azure home page, in the search bar, enter dns, and then select Private DNS zones.

<img width="395" alt="image" src="https://github.com/user-attachments/assets/eaa65299-cfb3-4ecb-b2dc-9ae1f32c80a3" />

- In Private DNS zones, select + Create.
- Fill the appropriate information in the columns to create the private DNS zone.

![image](https://github.com/user-attachments/assets/b2f53023-5c65-4153-803b-d979be7cda60)

#### Task 2: Link subnet for auto registration
- In Contoso.com, under DNS Management, select Virtual network links.
- On Contoso.com >> Virtual network links >> select + Add.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/3d5b5b7b-e5c4-44f5-97aa-bbc056f0e11d" />

###### Verify that the CoreServicesVnetLink has been created, and that auto-registration is enabled.

![image](https://github.com/user-attachments/assets/70101b31-1a1a-4507-9ccb-b7d4e67f0acd)

#### Task 3: Create Virtual Machines to test the configuration

- In the Azure portal, select the Cloud Shell icon (top right). If necessary, configure the shell.
- Select PowerShell.
- On the toolbar of the Cloud Shell pane, select the Manage Files icon, in the drop-down menu, select Upload and upload the template files: azuredeploy.json and azuredeploy.parameters.json and run the command below (remeber to replace the RG with your resource group name if different)
  
###### PS: The template files used are both in this repo folder

#### Note: You will be prompted to provide an Admin password. You will need this password in a later step.

```
$RGName = "Az700-RG"
   
New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
```

- When the deployment is complete, go to the Azure portal home page, and then select Virtual Machines.
- Verify that both virtual machines have been created.

![image](https://github.com/user-attachments/assets/85d0221e-0f50-4936-a67a-b6058206a4ea)

#### Task 4: Verify records are present in the DNS zone

- On the Azure Portal home page, select Private DNS zones.
- On Private DNS zones, select contoso.com.
- Verify that host (A) records are listed for both VMs, as shown below:

<img width="890" alt="image" src="https://github.com/user-attachments/assets/f0de1961-c337-4c67-bc08-b6763eae93d0" />

PS: Make a note of the names and IP addresses of the VMs.

##### Conclusion
- Connect to a VM to test the name resolution
- On the Azure Portal home page, select Virtual Machines.
- Select TestVM1.
- On TestVM1, select Connect > Connect and download the RDP file. Ensure the file downloads successfully.
- Locate the RDP file and double-click to execute the file.
- Select Connect and provide the TestUser password you provided during the template deployment.
- Select Okay and then Yes at the warning page.
- On TestVM1, open a command prompt and enter the command ipconfig /all.

Notice the IP address is the same as the one in the DNS zone.

Enter the command ping TestVM2.contoso.com. This command will timeout because of the Windows Firewall that is enabled on the VMs.

Instead, use nslookup TestVM2.contoso.com command to verify that you receive a successful name resolution record for VM2. This demonstrates private zone name resolution.

![image](https://github.com/user-attachments/assets/a3183838-a9b4-43ff-8b82-7be8b44e75b2)


#### Here are the main takeaways for this lab.

Azure DNS is a cloud service that allows you to host and manage domain name system (DNS) domains, also known as DNS zones.
Azure DNS public zones host domain name zone data for records that you intend to be resolved by any host on the internet.
Azure Private DNS zones allow you to configure a private DNS zone namespace for private Azure resources.
A DNS zone is a collection of DNS records. DNS records provide information about the domain.
