# Project Summary
A fully virtualised Security Operations Center (SOC) intended to simulate a Cybersecurity environment that Security Professionals can leverage to perform log aggregation, risk scoring, threat analysis and triage escalation. In this project, a Windows and Ubuntu VM will be used. Elasticsearch and Kibana are installed on the Ubuntu VM. Sysmon and Winlogbeat are installed on the Windows VM.

Elasticsearch is a tool that can receive unstructured logging and organise it into a database like structure with indexing. Kibana is a data analytics and visualisation software that can represent this structured data from Elasticsearch. This data can then be queried using the Kibana Query Language to parse out crucial information, analysed and visualised. Sysmon expands regular log collection capabilities to include process creation, file changes, network connections and more. Winlogbeat feeds this log collection data enhanced by Sysmon into the Ubuntu VM, which is handled by Elasticsearch and is represented in Kibana.

Elastic Security is built into Kibana and can create detection rules. In this lab, detection rules have been configured to detect any suspicious system behaviour, such as data exfiltration, process injections and registry tampering.

## Architecture Diagram

## Technological Requirements
### Hardware / VM requirements
- VirtualBox VM
   ○ Ubuntu/Debian Virtual Disk Image
      ▪ 4 GB of ram and 50 GB of HDD space
   ○ Windows 8/10/11 Virtual Disk Image
      ▪ 8 GB of ram and 80 GB of HDD space
  ### Software Requirements
  - Sysmon and Winlogbeat (Windows VM)
  - Kibana and Elasticsearch (Ubuntu VM)

## Deployment Instructions

## Lessons Learned

## Limitations

## Future Improvements
