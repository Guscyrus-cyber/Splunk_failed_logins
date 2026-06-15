
**Splunk Enterprise Tier 1 SOC 20 Security Event Monitoring Lab**

**Overview**

This lab was designed to simulate the daily responsibilities of a Tier 1 Security Operations Center (SOC) Analyst using Splunk Enterprise. The project consists of twenty security event datasets representing common enterprise security telemetry sources, including authentication logs, firewall events, web server activity, DNS queries, VPN access logs, Windows security events, endpoint security alerts, and network traffic data.

To create a realistic SOC environment, all datasets were custom-generated using Python scripts. The Python-based log generator was used to produce structured security events that simulate real-world enterprise activity and common security monitoring scenarios. Generating the datasets programmatically provided complete control over the event content while ensuring that each dataset contained sufficient data for meaningful Splunk analysis and investigation.

After generating the datasets, each log file was ingested into Splunk Enterprise and assigned to its appropriate index based on the security domain it represented. The datasets were then analyzed using Splunk Search Processing Language (SPL) to perform statistical investigations and event analysis.

The primary objective of this lab is to develop practical experience with log ingestion, indexing, event monitoring, and statistical analysis using Splunk Enterprise. Unlike dashboard-focused projects, this lab intentionally emphasizes raw event analysis and SPL-based investigations. No dashboards, visualizations, panels, reports, alerts, lookups, or correlation searches were used. Instead, the focus remained on understanding security events, identifying patterns and trends, and developing the search and analysis skills commonly used by Tier 1 SOC analysts during daily monitoring and triage activities.

For example, the Failed Login Attempts dataset was ingested into the authentication index and analyzed using SPL to count failed password attempts by source IP address and user account. Similar statistical investigations were performed across all twenty security event datasets, allowing the analyst to practice working with multiple security data sources commonly found in enterprise environments.\
\
**\
Security Event Categories**

This lab covers the following twenty security monitoring scenarios:

1.  Failed Login Attempts

2.  Successful Logins

3.  Top Source IP Addresses

4.  Top Destination IP Addresses

5.  Firewall Blocked Traffic

6.  Firewall Allowed Traffic

7.  HTTP 404 Errors

8.  HTTP 500 Errors

9.  DNS Query Monitoring

10. VPN Login Activity

11. After-Hours Login Detection

12. New User Account Creation

13. User Account Lockouts

14. Disabled Account Usage

15. Antivirus Detections

16. Top Talkers by Bandwidth

17. Critical Security Events

18. Most Active Hosts

19. Login Failures by User

20. Event Counts by Sourcetype

**Skills Practiced**

Throughout this lab, the following skills were developed and reinforced:

- Python log generation and dataset creation

- Security data engineering fundamentals

- Splunk Enterprise administration

- Log ingestion and indexing

- Search Processing Language (SPL)

- Statistical analysis using SPL commands

- Security event monitoring

- Authentication analysis

- Firewall traffic analysis

- DNS activity investigation

- VPN monitoring and access analysis

- Windows security event analysis

- Endpoint security monitoring

- Network traffic analysis

- Security operations workflows

- Event triage and investigation

- Threat detection fundamentals

- SOC Tier 1 analyst methodologies

- Security telemetry analysis

- Log source management and organization

**Outcome**

Upon completion of this lab, the analyst gained hands-on experience creating realistic security datasets using Python, ingesting multiple log sources into Splunk Enterprise, and performing statistical investigations using SPL. The project provided practical exposure to authentication monitoring, firewall analysis, web log investigation, DNS monitoring, VPN activity analysis, Windows security events, endpoint security alerts, and network traffic analysis.

By working directly with raw security events rather than dashboards or visualizations, the lab strengthened the analyst's ability to interpret log data, identify security-relevant activity, and perform the types of investigations commonly conducted within a Security Operations Center. The completed project serves as a foundational SOC monitoring portfolio piece and prepares the analyst for more advanced Tier 2 activities such as threat hunting, incident response, malware analysis, persistence detection, and adversary behavior investigations.\
\
**1. Python script file** (**generate_tier1_soc_datasets.py)**

**2. Terminal execution showing the datasets being created**

**3. The folder containing the 20 generated datasets**

**4. Splunk ingestion**

**5. Splunk searches and results**

Please refer to images #1, 2, and 3 in the repository.

| **\#** | **Dataset File**           | **Splunk Index** |
|--------|----------------------------|------------------|
| 1      | Failed_logins.log          | auth             |
| 2      | successful_logins.log      | auth             |
| 3      | top_source_ips.log         | network          |
| 4      | top_destination_ips.log    | network          |
| 5      | firewall_blocked.log       | firewall         |
| 6      | Firewall_allowed.log       | Firewall         |
| 7      | http_404.log               | web              |
| 8      | http_500.log               | web              |
| 9      | dns_queries.log            | dns              |
| 10     | vpn_logins.log             | vpn              |
| 11     | after_hours_logins.log     | auth             |
| 12     | new_users.log              | windows          |
| 13     | account_lockouts.log       | windows          |
| 14     | disabled_account_usage.log | windows          |
| 15     | antivirus_detections.log   | endpoint         |
| 16     | bandwidth_top_talkers.log  | network          |
| 17     | critical_events.log        | security         |
| 18     | active_hosts.log           | security         |
| 19     | login_failures_by_user.log | auth             |
| 20     | sourcetype_count.log       | security         |

**Note:** This repository contains one security event investigation from the Splunk Enterprise Tier 1 SOC 20 Security Event Monitoring Lab series. For the remaining security event investigations, please refer to the corresponding repositories in this project collection.

## Failed Logins.log

Dataset: failed_logins.log\
Index: auth\
Description: Simulated failed login security events for Splunk Tier 1 SOC monitoring.

I will treat failed_logins.log as the first Tier 1 security event and build several SOC-style statistical searches around it: basic validation, top users, top source IPs, brute-force patterns, and host/user targeting.

Below are useful SOC Tier 1 SPL queries for failed_logins.log.

Please refer to image # 4 in the repository.

**1. Confirm Dataset Ingestion**

index=auth source="\*failed_logins.log"\
\| stats count by source index sourcetype

Purpose: Confirms the dataset was ingested into the correct index and sourcetype.

Please refer to image # 5 in the repository.

**2. View Raw Failed Login Events**

index=auth source="\*failed_logins.log"\
\| table \_time host user src_ip action message

Purpose: Shows the raw failed login activity in table format.

Please refer to image # 6 in the repository.

**3. Count Failed Logins by Source IP and User**

index=auth source="\*failed_logins.log" "failed password"\
\| stats count by src_ip,user\
\| sort -count

Purpose: Identifies which source IPs are failing against which user accounts.

Please refer to image # 7 in the repository.

**4. Top Source IPs Causing Failed Logins**

index=auth source="\*failed_logins.log"\
\| stats count by src_ip\
\| sort -count

Purpose: Finds the most active source IP addresses generating failed logins.

Please refer to image # 5 in the repository.

**5. Top Targeted User Accounts**

index=auth source="\*failed_logins.log"\
\| stats count by user\
\| sort -count

Purpose: Shows which user accounts are receiving the most failed login attempts.

Please refer to image # 9 in the repository.

**6. Possible Brute Force by Source IP**

index=auth source="\*failed_logins.log"\
\| stats count by src_ip\
\| where count \>= 5\
\| sort -count

Purpose: Detects source IPs with repeated failed login attempts.

Please refer to image # 10 in the repository.

**7. Failed Logins by Host**

index=auth source="\*failed_logins.log"\
\| stats count by host\
\| sort -count

Purpose: Shows which systems are receiving the failed login attempts.

Please refer to images # 11 in the repository.

**8. Failed Logins Over Time**
\
index=auth source="\*failed_logins.log"\
\| timechart span=10m count

Purpose: Shows when failed login activity increased or decreased over time.

Please refer to image # 12 in the repository.

**9. Failed Logins by User and Host**

index=auth source="\*failed_logins.log"\
\| stats count by user,host\
\| sort -count

Purpose: Identifies which users are failing on which systems.

Please refer to image # 13 in the repository.

**10. Unique Users Targeted by Each Source IP**

index=auth source="\*failed_logins.log"\
\| stats dc(user) as unique_users values(user) as targeted_users by src_ip\
\| sort -unique_users

Purpose: Helps detect password spraying, where one source IP tries multiple users.

Please refer to images # 14 and 15 in the repository.

**11. Unique Source IPs per User**

index=auth source="\*failed_logins.log"\
\| stats dc(src_ip) as unique_src_ips values(src_ip) as source_ips by user\
\| sort -unique_src_ips

Purpose: Shows whether one user account is being attacked from multiple IP addresses.

Please refer to images # 16 and 17 in the repository.


**12. Statistical Summary**

index=auth source="\*failed_logins.log"\
\| stats count as total_failed_logins dc(src_ip) as unique_source_ips dc(user) as affected_users

Or

index=auth source="\*failed_logins.log" "failed password"\
\| stats count by src_ip,user\
\| sort -count

Purpose: Gives a clean statistical summary for documentation.

Please refer to images # 18 and 19 in the repository.



