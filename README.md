<a href="https://imgur.com/CucK6qJ"><img src="https://i.imgur.com//CucK6qJ.png" tB2TqFcLitle="source: imgur.com" /></a>

# NGFW-Azure-Bastion
This project focus on securing virtual machines (VMs) within the Azure cloud environment, apply Azure Bastion service to avoid assigning public IP addresses for our resources. We will achieve this by deploying a Fortinet Next-Generation Firewall (NGFW) on Microsoft Azure.  The NGFW will act as a central security barrier, inspecting and managing all network traffic flowing to and from the VMs.  Firewall rules will be applied to shield the VMs from cyber threats, To further enhance protection, Intrusion Prevention System (IPS) and Intrusion Detection System (IDS) functionalities within the NGFW will be configured to identify and block suspicious network activity in real-time. Finally, the project will explore the integration of the configured firewall rules with Microsoft Sentinel, a cloud-native security information and event management (SIEM) solution.

# Objectives
  - Create Resource group and Virtual machines without RDP access for public inbound ports (No public IP address).
  - Create a Virtual network and add three Subnets within the V-Net (WAN, DMZ and Bastion subnets).
  - Create Route tables, add two routes (Inbound and Outbound) associate them with the appropriate subnet.
  - Deploy Fortinet FortiGate Next-Generation Firewall within Azure and configure it's policy and routes.
  
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
 
 # We must acknowledge that having the management of our firewall open to the open internet is not a best security practice, it is setup this way for simplicity and educational purposes.

   - Double click each interface to edit > Rename Port1 and Port2, for Port1 WAN interface only allow HTTPS and PING traffic
   - For Port2 DMZ interface only allow SSH and PING traffic

     <a href="https://imgur.com/HaaaeQx"><img src="https://i.imgur.com//HaaaeQx.png" title="source: imgur.com" /></a>

# 9. Next, we create route tables for outbound traffic from our  servers to the internet and inbound traffic from the internet

   - Azure search bar > Route tables > + Create

  <a href="https://imgur.com/0vMODoX"><img src="https://i.imgur.com//0vMODoX.png" title="source: imgur.com" /></a>

   - Route tables > Settings > Routes > + Add
   - Route name = Outbound
   - Destination type = IP address
   - Destination IP address = 0.0.0.0/0 (Default internet address)
   - Next hop type = Virtual appliance
   - Next hop address = 10.10.10.10 (Internal firewall port)

  <a href="https://imgur.com/B2TqFcL"><img src="https://i.imgur.com//B2TqFcL.png" tB2TqFcLitle="source: imgur.com" /></a>

   - Now we need to associate the route we just created with a subnet
     - Route overview > Subnets > + Assoiciate > Select the V-Net and the DMZ subnet

     # Add Inbound route
      
   - Route tables > Settings > Routes > + Add
   - Route name = Inbound
   - Destination type = IP address
   - Destination IP address = 10.10.10.10/24 (DMZ Subnet)
   - Next hop type = Virtual appliance
   - Next hop address = 10.10.10.10 (External firewall port)

  <a href="https://imgur.com/DWR519y"><img src="https://i.imgur.com//DWR519y.png" tB2TqFcLitle="source: imgur.com" /></a>

   - Associate the route we just created with a subnet
     - Route overview > Subnets > + Assoiciate > Select the V-Net and the WAN subnet
     
# Now we can use PING command to test our connection within the DMZ and outbound PING request to Google.com

 - We are able to PING succesfully to our local machine in the DMZ and to Google.com
 - Also captured the following PING traffic on our NGFW
   - Fortinet-Fortiget Firewall > Log & Report > Forward Traffic

   <a href="https://imgur.com/qk0iZji"><img src="https://i.imgur.com//qk0iZji.png" tB2TqFcLitle="source: imgur.com" /></a>

# 10. Create a Firewall policy for outbound traffic on our NGFW

  - Incoming interface = DMZ (Port2)
  - Outgoing interface = WAN (Port1)
  - Service = PING,DNS and HTTPS
  - Action = Accept
  - NAT = Enabled
  - Policy = Enabled

    <a href="https://imgur.com/3GvSbMa"><img src="https://i.imgur.com//3GvSbMa.png" tB2TqFcLitle="source: imgur.com" /></a>

# Now we need to create a virtual IP for Network Address Transilation for inbound policy allowing only RDP traffic

 - Policy & Objects > Virtual IP > + Create new
   - External Ip address = External port on Firewall
   - Map to IPV4 address = IP for Windows machine in the DMZ
   - Port forwarding > External IP & Map to IPV4 = 3389 (RDP)

<a href="https://imgur.com/QE9nD1z"><img src="https://i.imgur.com//QE9nD1z.png" tB2TqFcLitle="source: imgur.com" /></a>

# Create additional incoming Firewall policy for RDP traffic

  - Source = All
  - Destination = Virtual IP we created previous step
  - Service = RDP
  - NAT = Disabled

<a href="https://imgur.com/MotK6tj"><img src="https://i.imgur.com//MotK6tj.png" tB2TqFcLitle="source: imgur.com" /></a>
    




