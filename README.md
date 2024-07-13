<a href="https://imgur.com/CucK6qJ"><img src="https://i.imgur.com//CucK6qJ.png" tB2TqFcLitle="source: imgur.com" /></a>

# NGFW-Azure-Bastion
This project focus on securing virtual machines (VMs) within the Azure cloud environment, apply Azure Bastion service to avoid assigning public IP addresses for our resources. We will achieve this by deploying a Fortinet Next-Generation Firewall (NGFW) on Microsoft Azure.  The NGFW will act as a central security barrier, inspecting and managing all network traffic flowing to and from the VMs.  Firewall rules will be applied to shield the VMs from cyber threats, To further enhance protection, Intrusion Prevention System (IPS) and Intrusion Detection System (IDS) functionalities within the NGFW will be configured to identify and block suspicious network activity in real-time. Finally, the project will explore the integration of the configured firewall rules with Microsoft Sentinel, a cloud-native security information and event management (SIEM) solution.

# Objectives
  - Create Resource group and Virtual machines without RDP access for public inbound ports (No public IP address).  
  - Create a Virtual network and add three Subnets within the V-Net (WAN, DMZ and Bastion subnets).
  - Create Route tables, add two routes (Inbound and Outbound) associate them with the appropriate subnet.
  - Deploy and Configure a Next-Gen FortiGate Firewall, set up firewall rules, and implement Intrusion Prevention Systems (IPS) for enhanced security.
  - Create a Log Analytics Workspace on Azure and connect our firewall to this workspace for efficient log management.
  - Utilize Azure's Sentinel(SIEM) tool in creating alerts, conducting incident response, and ensuring your network's integrity.

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
 
 # Note: We must acknowledge that having the management of our firewall open to the open internet is not a best security practice, it is setup this way for simplicity and educational purposes.

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
   - FortiGate WebUI > Log & Report > Forward Traffic

   <a href="https://imgur.com/qk0iZji"><img src="https://i.imgur.com//qk0iZji.png" tB2TqFcLitle="source: imgur.com" /></a>

# 10. Create a Firewall policy for outbound traffic on our NGFW

  - FortiGate WebUI > Policy & Objects > Firewall Policy > = Create new
  - Incoming interface = DMZ (Port2)
  - Outgoing interface = WAN (Port1)
  - Service = PING,DNS and HTTPS
  - Action = Accept
  - NAT = Enabled
  - Policy = Enabled

    <a href="https://imgur.com/3GvSbMa"><img src="https://i.imgur.com//3GvSbMa.png" tB2TqFcLitle="source: imgur.com" /></a>

# Now we need to create a virtual IP for Network Address Transilation for inbound policy allowing only RDP traffic

 - FortiGate WebUI > Policy & Objects > Virtual IP > + Create new
   - External Ip address = External port on Firewall
   - Map to IPV4 address = IP for Windows machine in the DMZ
   - Port forwarding > External IP & Map to IPV4 = 3389 (RDP)

<a href="https://imgur.com/QE9nD1z"><img src="https://i.imgur.com//QE9nD1z.png" tB2TqFcLitle="source: imgur.com" /></a>

# Create additional incoming Firewall policy for RDP traffic

  - FortiGate WebUI > Policy & Objects > Port1 (WAN to DMZ)
  - Incoming interface = WAN (port1)
  - Outgoing interface = DMZ (port2)
  - Source = All
  - Destination = Virtual IP we created previous step
  - Service = RDP
  - NAT = Disabled
  - Logging options
    - Log allowed traffic > On > All sessions
    - Generate logs when session starts > On
    
 <a href="https://imgur.com/MotK6tj"><img src="https://i.imgur.com//MotK6tj.png" tB2TqFcLitle="source: imgur.com" /></a>

# 11. Test our connection between our VMs in the DMZ and to the internet using PING command.
  
  - Successfull PING request
     
  <a href="https://imgur.com/69h5XC7"><img src="https://i.imgur.com//69h5XC7.png" tB2TqFcLitle="source: imgur.com" /></a>

  - See the captured ICMP and PING traffic by our NGFW

  <a href="https://imgur.com/qk0iZji"><img src="https://i.imgur.com//qk0iZji.png" tB2TqFcLitle="source: imgur.com" /></a>

  - While working on this project, our Next-Gen Firewall detected a number of login attempts into our enviroment using RDP session, which we left intentionally open to all incoming traffic on our firewall policy, but one malicious IP has attempted 636 times within one hour, which indicate a bruteforce attack, screeshot below along with other malcious IP from different geographical locations.

  <a href="https://imgur.com/qgdimsH"><img src="https://i.imgur.com//qgdimsH.png" tB2TqFcLitle="source: imgur.com" /></a>

  - Additional malicious IPs
    
  <a href="https://imgur.com/o6m07F0"><img src="https://i.imgur.com//o6m07F0.png" tB2TqFcLitle="source: imgur.com" /></a>

  <a href="https://imgur.com/OTtkd3I"><img src="https://i.imgur.com//OTtkd3I.png" tB2TqFcLitle="source: imgur.com" /></a>

# 12. Enable IPS (Intrusion Prevention System) on our NGFW

  - FortiGate WebUI > Intrusion Prevention > + Create new
  - Botnet C&C = Blocked
  - Create new
  - Type = Signature
  - Action = Monitor
  - Search bar > Type 'RDP' > Select 'MS.RDP.Connection.Brute.Force' > Ok

    <a href="https://imgur.com/RhrZlZY"><img src="https://i.imgur.com//RhrZlZY.png" tB2TqFcLitle="source: imgur.com" /></a>

  - We can see the IPS signature rules and filters in the details section
    
    <a href="https://imgur.com/MotK6tj"><img src="https://i.imgur.com//MotK6tj.png" tB2TqFcLitle="source: imgur.com" /></a>

  - FortiGate WebUI > Policy & Objects > Firewall Policy
  - Double click Port1-WAN interface
  - Security Profiles > IPS toggle On > from drop down select the IPS signature that we just created
  - Logging options
    - Log allowed traffic > On > All sessions
    - Generate logs when session starts > On

    <a href="https://imgur.com/XO2mYJe"><img src="https://i.imgur.com//XO2mYJe.png" tB2TqFcLitle="source: imgur.com" /></a>

# 13. Next we create a custom IPS signature that will enable us to block a malicious bruteforce attack

  - FortiGate WebUI > Security Profiles > IPS Signatures > Create new
  - Name it > Paste the following custom signature > Ok
  - "F-SBID( --attack_id 7170; --name "MS.RDP.Connection.Brute.Force."; --protocol TCP; --dst_port 3389; --flow from_client; --seq 1, relative; --pattern "|e0|"; --distance 5,packet; 
    within 1,packet; --rate 5,20; --track SRC_IP ; )"
  - Security Profiles > Intrusion Prevention > Create new > Select custom signature
  - Type = Signature > Action = Block Status = Enable > Add selected

    <a href="https://imgur.com/wAfub3s"><img src="https://i.imgur.com//wAfub3s.png" tB2TqFcLitle="source: imgur.com" /></a>

# 14. Generate an Action = Deny (Blocked) by UTM (Unified threat management) log for RDP connection brute force attack

  - Open RDP (Remote Desktop Connection) from local machine (Outside DMZ)
  - Attempt to login with incorrect credentials (Username & Password) > 5X

    <a href="https://imgur.com/VOeb3L0"><img src="https://i.imgur.com//VOeb3L0.png" tB2TqFcLitle="source: imgur.com" /></a>

   Log Details
   
    <a href="https://imgur.com/dzUh16P"><img src="https://i.imgur.com//dzUh16P.png" tB2TqFcLitle="source: imgur.com" /></a>





