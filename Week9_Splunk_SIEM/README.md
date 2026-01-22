# Week 9 — SIEM Basics with Splunk

## Overview
This folder documents hands-on work with a SIEM using Splunk. The focus is on installing Splunk, ingesting Windows Event Logs, running searches, building dashboards, and creating alerts based on security-related activity. All work is supported with screenshots and log evidence.

## Learning Goals
- Understand how a SIEM is used for security monitoring  
- Install and configure Splunk in a lab environment  
- Ingest and analyze Windows Event Logs  
- Create searches, dashboards, and alerts  
- Practice filtering and reporting on log data  

---

## Splunk Installation and Setup
**What I did**
- Installed Splunk on a virtual machine  
- Completed initial setup and verified Splunk access  
- Confirmed Splunk services were running correctly  

**Artifacts added**
- Screenshot of Splunk web interface after login  

**Why this matters**
- A functioning SIEM is required before logs, alerts, and dashboards can be created.

---

## Windows Event Log Ingestion
**What I did**
- Configured Splunk to collect Windows Event Logs  
- Verified logs were successfully ingested  
- Reviewed key log fields such as event ID, user, source, and time  

**Artifacts added**
- Screenshot showing Windows Event Logs in Splunk  
- Screenshot highlighting important log fields  

**Why this matters**
- Windows Event Logs are essential for detecting login failures and system activity.

---

## Searches and Dashboards
**What I did**
- Created basic Splunk search queries  
- Focused searches on failed login events  
- Built a dashboard to visualize security data  

**Artifacts added**
- Screenshot of search queries  
- Screenshot of completed dashboard  

**Why this matters**
- Dashboards provide quick visibility into system and user activity.

---

## Alerts and Detection
**What I did**
- Created an alert for suspicious activity such as port scanning  
- Configured alert conditions and trigger thresholds  
- Reviewed alert behavior when conditions are met  

**Artifacts added**
- Screenshot of alert configuration  
- Screenshot showing alert trigger logic  

**Why this matters**
- Alerts allow security teams to respond quickly to potential threats.

---

## SIEM Filtering and Reporting
**What I did**
- Filtered logs by IP address and severity  
- Ran targeted searches to reduce noise  
- Documented example queries used for filtering  

**Artifacts added**
- Screenshot of filtered search results  
- Screenshot of query details  

**Why this matters**
- Filtering helps analysts focus on meaningful security events.

---

## Dashboard and Alert Integration
**What I did**
- Combined dashboards and alerts into a unified view  
- Verified that visualizations and alerts work together  

**Artifacts added**
- Screenshot showing dashboards and active alerts  

**Why this matters**
- Integrated monitoring improves investigation and response efficiency.

---

## Tools & Technologies
- Splunk  
- Windows Event Logs  
- Virtual Machines (Windows/Linux)  

## Repository Structure
Week_9_SIEM_Splunk/
├── dashboards/
├── alerts/
├── searches/
├── screenshots/
└── notes/
