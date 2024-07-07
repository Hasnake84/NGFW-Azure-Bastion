# NGFW-Azure-Bastion

This project focus on securing virtual machines (VMs) within the Azure cloud environment, apply Azure Bastion service to avoid assigning public IP addresses for our resources. We will achieve this by deploying a Fortinet Next-Generation Firewall (NGFW) on Microsoft Azure.  The NGFW will act as a central security barrier, inspecting and managing all network traffic flowing to and from the VMs.  Firewall rules will be applied to shield the VMs from cyber threats, including unauthorized access attempts, malware infiltration, and malicious data exfiltration.  To further enhance protection, Intrusion Prevention System (IPS) and Intrusion Detection System (IDS) functionalities within the NGFW will be configured to identify and block suspicious network activity in real-time. Additionally, a simulated brute-force attack will be conducted to showcase the NGFW's ability to detect and thwart such attempts. Finally, the project will explore the integration of the configured firewall rules with Microsoft Sentinel, a cloud-native security information and event management (SIEM) solution. This integration will allow for centralized logging, analysis, and visualization of security events, providing valuable insights for proactive threat detection and response.

  <a href="https://imgur.com/HgTstYR"><img src="https://i.imgur.com//HgTstYR.png" title="source: imgur.com" /></a>  

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

     

     




