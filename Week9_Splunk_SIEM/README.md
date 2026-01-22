# Week 9 — SIEM Basics with Splunk

This folder documents hands-on work with a SIEM platform, focusing on log collection, searching, alerts, and basic reporting. The goal is to show how security data is ingested, reviewed, and used to detect activity in a Windows environment.

## Learning Goals
- Understand how a SIEM collects and stores logs
- Practice writing basic search queries
- Build simple dashboards for visibility
- Create alerts for suspicious activity
- Review and filter logs for investigation

## SIEM Setup and Access
**What I did:**  
Set up the SIEM platform and confirmed it was running and accessible on the system.

**Artifacts added:**  
- Setup confirmation screenshots

**Why this matters:**  
A SIEM must be properly running before logs, searches, and alerts can work correctly.

## Log Ingestion (Windows Event Logs)
**What I did:**  
Ingested Windows Event Logs into the SIEM to collect real system activity.

**Artifacts added:**  
- Screenshot showing event logs being received

**Why this matters:**  
Event logs are a key data source for detecting logins, errors, and security events.

## Searches and Dashboards
**What I did:**  
Created basic search queries and built dashboards to display important events, such as failed login attempts.

**Artifacts added:**  
- Dashboard screenshots
- Search result screenshots

**Why this matters:**  
Searches and dashboards help analysts quickly see patterns and potential issues.

## Alerts and Monitoring
**What I did:**  
Set up alerts to trigger when suspicious activity, such as port scans, is detected.

**Artifacts added:**  
- Screenshot of alert configuration
- Alert trigger evidence

**Why this matters:**  
Alerts allow faster response by notifying analysts when something needs attention.

## Filtering and Reporting
**What I did:**  
Practiced filtering logs by IP address and severity to narrow down results and review specific activity.

**Artifacts added:**  
- Filtered search screenshots

**Why this matters:**  
Filtering helps reduce noise and focus on relevant security events.

## Repository Structure

Week_9_SIEM_Splunk/
├── configs/ # Configuration files related to SIEM and logs
├── searches/ # Saved search queries and filters
├── dashboards/ # Dashboard views and layouts
├── alerts/ # Alert rules and related details
└── screenshots/ # Visual proof of setup, searches, and alerts
