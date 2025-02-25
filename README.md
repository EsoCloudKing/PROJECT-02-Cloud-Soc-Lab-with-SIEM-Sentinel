# PROJECT NAME
 
 **Cloud SOC Lab for Advanced Security: Threat Detection and Response with SIEM (Microsoft Sentinel) Implementation**.
 
 ## Objective
  This project is to design, implement, and showcase a comprehensive Security Operations Center (SOC) lab in the cloud. This SOC lab will leverage Microsoft Sentinel to provide advanced threat detection, real-time monitoring, and automated response capabilities. The project aims to demonstrate proficiency in setting up a cloud-based SOC environment, integrating various data sources, implementing Analytic rules for threat detection, workbook for threat visualization, Threat Intelligence Integration such as threat feed  and watch lists to track and monitor indicators of compromise (IOCs) and automating incident response workflows with Logic Apps in either real time or simulated alerts.

  ## Skills Learned
  - __Cloud Infrastructure Setup__: Proficiency in deploying and configuring virtual machines, storage accounts, and network security groups in Azure. Ability to manage and organize cloud resources effectively using Azure Resource Manager (ARM) templates and Azure portal.
  - __SIEM Implementation__:Expertise in setting up Microsoft Sentinel in a Log Analytics workspace. Integration of multiple data sources (e.g., Azure Activity Logs, SecurityEvent, network logs, storage logs) into Microsoft Sentinel for centralized monitoring.
  - __KQL__: Use of Kusto Query Language (KQL) to query, analyze, and visualize security logs and events.
  - __Security Monitoring and Incident Management__: Continuous monitoring of security alerts and incidents in Microsoft Sentinel and the Use of Sentinel's investigation tools to analyze and respond to security incidents.
    

  ### Tools Used For this Project
- __Microsoft Visio__ for diagram mapping.
- __Azure Portal:__ To manage and configure Azure resources, such as virtual machines, storage accounts, and virtual networks, NSGs, Storage Account.
- __Microsoft Sentinel:__ The primary SIEM solution for log collection, analysis, threat detection, and incident response. Watchlist, Analytic Rules, Azure Logic Apps were explored.
- __Log Analytics Workspace:__ For collecting and analyzing log data from various sources.
- __Kusto Query Language (KQL):__ To query, analyze, and visualize log data in the Log.
- __Powershell:__ To ping Virtual machine.
- __Diagnostic Settings__: To configure and send logs from Azure resources to the Log Analytics workspace.

## Steps
- **Step 1: Provisioning of resources that is to be monitored**

A virtual network was provisioned with Network security configured to allow all ports (RDP, SSH, HTTP, HTTPS) for inbound traffic.
An Azure virtual machine to be used somehow as a honey pot to observe and respond to alerts especially windows event 4625(Failed log on). All ports were opened for this VM (RDP, SSH, HTTP, HTTPS)after which, I logged in via RDP virtual machine and turned off the windows firewall (start -> wf.msc -> properties -> all off). Pinged the virtual machine with powershell to confirm its reachability from any network.
A storage account with security settings relaxed to allow for more security events. Anonymous access on blobs and files was enabled; permitted scope of copy operation from any storage account and cross tenant replication was enabled. Public network access from all networks was also enabled. These settings were configured for the purpose of this lab to generate more alerts for triage and response with a SIEM solution (Sentinel)
Provisioned Another Virtual machine in a different subnet and deployed ngix webserver which will be monitored too.

##IMAGE 1a: NSG rules 

![NSG4](https://github.com/user-attachments/assets/ba9e8cd3-17ef-4ed5-b490-61bb49548bc6)

##Image 1b: VM network test from public network

![powershell ping](https://github.com/user-attachments/assets/296e2974-015f-40ce-826d-362b30c7bbff)



- **Step 2: Log Forwarding and KQL**
Created Log Analytics Workspace, Created a Sentinel Instance and connected it to Log Analytics.
Configured the “Windows Security Events via AMA”, storage account and Azure activity connectors in sentinel.
Created the DCR within sentinel for the webserver and VM, configured diagnostic settings in storage account for log collection in sentinel
Then I Queried for logs within the log Analytics workspace to make sure it is receiving logs (SecurityEvent
| where EventId == 4625) and this returned some nuber of entries after delibrate failed log on to the Vms.

**Image2** query results of security event
![securityevent log](https://github.com/user-attachments/assets/adfd9be5-b534-4550-925e-ee5b59895349)




- **Step 3:** Analytics rule creation


- **Step 4: Created a Watchlist for Log Enrichment and Finding Location Data of any suspeted Attacker**

When I queried the SecurityEvent logs in the Log Analytics Workspace; there is no location data, only IP address, which we can use to derive the location data.
So i imported a pre-downloaded geoip spreadsheet (as a “Sentinel Watchlist”) which contains geographic information for each block of IP addresses which is about 54,000 rows.
A watchlist was created and this csv file uploaded to the watch list to enrich our query data.
In actual datacenter settings, this location data would come from a live source or it would be updated automatically on the back end by any service provider we decide to use.
Using this watchlist the query result will display the location, latitude, country etc of the concerned IP of interest as opposed to when the watchlist was not created as dipicted in the image below;
   
   **Image4** Watchlist KQL displaying location of IP addresses involved in the alerts or incedence.
   ![real geoip with my ip](https://github.com/user-attachments/assets/4290e154-b777-4e86-a944-588b9703abe6)
 

- **Step 5: Workbook (Attack Map Creation)**
Microsoft Sentinel workbooks provide powerful data visualization and monitoring capabilities, allowing for easy analysis and insight into security events and incidents.
In sentinel, a new workbook was created and   the advanced editor tab was prepopulated by a preconfigured KQL in JSON format to display the image below. Giving us the location of IPs and counts of alerts in Visual form which is a Map.

**Image5:** Attack Map

                                                            ##CONCLUSION
The "Cloud SOC Lab for Advanced Security: Threat Detection and Response with SIEM (Microsoft Sentinel) Implementation" project showcases my ability to design and implement a comprehensive Security Operations Center in the cloud. Through this project, I have demonstrated proficiency in cloud infrastructure setup, SIEM deployment, log analysis. By integrating multiple data sources and leveraging advanced security tools, I have developed a robust and intelligent threat detection and response system. This project not only highlights my technical expertise but also my commitment to enhancing cybersecurity through innovative solutions. My hands-on experience, capabilities and innovative approach in cloud security, threat intelligence, and real-time monitoring make me a valuable asset to any cybersecurity team.






 







