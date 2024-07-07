# NGFW-Azure-Bastion

This project focus on securing virtual machines (VMs) within the Azure cloud environment, apply Azure Bastion service to avoid assigning public IP addresses for our resources. We will achieve this by deploying a Fortinet Next-Generation Firewall (NGFW) on Microsoft Azure.  The NGFW will act as a central security barrier, inspecting and managing all network traffic flowing to and from the VMs.  Firewall rules will be applied to shield the VMs from cyber threats, including unauthorized access attempts, malware infiltration, and malicious data exfiltration.  To further enhance protection, Intrusion Prevention System (IPS) and Intrusion Detection System (IDS) functionalities within the NGFW will be configured to identify and block suspicious network activity in real-time. Additionally, a simulated brute-force attack will be conducted to showcase the NGFW's ability to detect and thwart such attempts. Finally, the project will explore the integration of the configured firewall rules with Microsoft Sentinel, a cloud-native security information and event management (SIEM) solution. This integration will allow for centralized logging, analysis, and visualization of security events, providing valuable insights for proactive threat detection and response.

  <a href="https://imgur.com/3yK2AMT "><img src="https://i.imgur.com//3yK2AMT.png" title="source: imgur.com" /></a> 

# 1. Azure.portal.com > Resource group > Create Resource group
# 2. Create Windows-10-Pro and Linux-Ubuntu-Server

   - Basics > select 'None' for Inbound Rules

     <a href="https://imgur.com/M3YBYn7"><img src="https://i.imgur.com//M3YBYn7.png" title="source: imgur.com" /></a>  

   - Networking > select 'None' for NIC network security group
  
     <a href="https://imgur.com/mHbMGfb"><img src="https://i.imgur.com//mHbMGfb.png" title="source: imgur.com" /></a>
     
# 3. Create a Virtual Network  

   - Virtual network > Create > Security > Enable Azure Bastion

     <a href="https://imgur.com/DH93PIE"><img src="https://i.imgur.com//DH93PIE.png" title="source: imgur.com" /></a>

# 4. Virtual network > Open the created V-Net > Settings > Subnets > + Subnet

   - Create the following three subnets with their own IP address ranges

     <a href="https://imgur.com/RbQVkK4"><img src="https://i.imgur.com//RbQVkK4.png" title="source: imgur.com" /></a>

# 5. Next, we create our Fortinet FortiGate Next-Generation Firewall 
  
   - Search bar > Market place > Fortinet > Fortinet FortiGate Next-Generation Firewall > Single VM > Create

     <a href="https://imgur.com/wleC0HB"><img src="https://i.imgur.com//wleC0HB.png" title="source: imgur.com" /></a>
 
   - Configure Networking > Select the External and Internal subnets as follows

     <a href="https://imgur.com/EpyyYhK"><img src="https://i.imgur.com//EpyyYhK.png" title="source: imgur.com" /></a>

# 6. Open our Fortinet FortiGate Next-Generation Firewall > Copy Public IP that is assigned to our Firewall resource

  <a href="https://imgur.com/WiuxWm1"><img src="https://i.imgur.com//WiuxWm1.png" title="source: imgur.com" /></a>  

# 7. Paste Public IP into any browser > Advanced > Proceed > Login with the credential that we created earlier

  <a href="https://imgur.com/NvM5YXm"><img src="https://i.imgur.com//NvM5YXm.png" title="source: imgur.com" /></a>  

   - Once we login we can see the Status under Dashboard, we have AntiVirus, IPS and Web filter enabled

     <a href="https://imgur.com/GWNivpj"><img src="https://i.imgur.com//GWNivpj.png" title="source: imgur.com" /></a>

# 8. FortiGet-FW > Network > Interfaces
 
 # We must acknowledge that having the management of our firewall open to the open internet is not a best security practice, it is setup this way for simplicity and educational purpose.

   - Double click each interface to edit > Rename Port1 and Port2, for Port1 WAN interface only allow HHTPS and PING traffic
   - For Port2 DMZ interface only allow SSH and PING traffic

     <a href="https://imgur.com/HaaaeQx"><img src="https://i.imgur.com//HaaaeQx.png" title="source: imgur.com" /></a>

 


    




