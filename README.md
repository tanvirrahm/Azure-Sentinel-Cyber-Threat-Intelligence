# Azure Security Lab Portfolio

## Part 1: Creating the Honey Pot (Azure Virtual Machine)
- **Virtual Machine (VM) Setup**:
  - Created a new Windows 10 virtual machine within the Azure portal.
  - Set up the Network Security Group (NSG) to allow inbound traffic from all sources.
  - Disabled the Windows firewall within the VM via `wf.msc` for logging purposes.

## Part 2: Logging into the VM and Inspecting Logs
- Simulated failed login attempts to create security logs.
- Used the Event Viewer to inspect security logs, focusing on event ID 4625 (Failed login attempts).
- The logs were essential in setting up a central log repository for monitoring.

## Part 3: Log Forwarding and KQL
- **Log Analytics Workspace (LAW)**:
  - Created a Log Analytics Workspace to centralize logs.
  - Configured a Microsoft Sentinel instance and connected it to the workspace.
  - Configured the “Windows Security Events via AMA” connector for log forwarding.
- **KQL Querying**:
  - Implemented Kusto Query Language (KQL) to query logs from both the LAW and Sentinel instance.
  - Executed queries like `SecurityEvent | where EventId == 4625` to analyze failed login events.

## Part 4: Log Enrichment and Location Data
- **IP Geolocation Enrichment**:
  - Imported the `geoip-summarized.csv` file as a Sentinel Watchlist to enrich the logs with geographic location data.
  - Used KQL to correlate IP addresses with geographical information, enabling location-based insights on failed login attempts.
  - Executed queries to identify the location of attacks by enriching the logs:
    ```kusto
    let GeoIPDB_FULL = _GetWatchlist("geoip");
    let WindowsEvents = SecurityEvent
        | where IpAddress == <attacker IP address>
        | where EventID == 4625
        | order by TimeGenerated desc
        | evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network);
    WindowsEvents
    ```

## Part 5: Attack Map Creation
- **Creating a Workbook**:
  - Designed an interactive attack map within Sentinel.
  - Customized the workbook by deleting prepopulated elements and adding a query element.
  - Configured the map using a JSON-based query to visualize the locations of failed login attempts.

## Conclusion
Successfully built a comprehensive log monitoring and analysis system using Azure and Microsoft Sentinel. The system involved creating a virtual machine as a honeypot, capturing and analyzing security event logs, enriching logs with geolocation data, and visualizing attack patterns through an interactive map.

---

## GitHub Repository

Here is the GitHub code repository that contains the scripts and configurations for the tasks mentioned above:  
[**Azure Security Lab Repository**](#)

- **Scripts**:  
  - KQL queries for log analysis
  - JSON configurations for workbooks

