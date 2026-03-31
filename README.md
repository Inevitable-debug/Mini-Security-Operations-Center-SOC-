# Project Summary
A fully virtualised Security Operations Center (SOC) intended to simulate a real world enterprise Cybersecurity environment where Security Professionals can follow SOC Metholodigies, such as performing log aggregation, risk scoring, threat analysis and triage. In this project, a Windows and Ubuntu VM will be used. Elasticsearch and Kibana are installed on the Ubuntu VM. Sysmon and Winlogbeat are installed on the Windows VM.

Elasticsearch is a tool that can receive logging data and convert it into a datastore, as well as a vector database. Kibana is a data analytics and visualisation software that can represent this data from Elasticsearch. This data can be queried using the Kibana Query Language to parse out crucial information for threat hunting and can be analysed by Security Professionals. Sysmon expands regular log collection capabilities to include process creation, file changes, network connections and more. Winlogbeat feeds this log collection data enhanced by Sysmon into the Ubuntu VM, which is handled by Elasticsearch and is represented in Kibana.

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

###Elasticsearch installation

1. Install the Elastic Public key. This ensures that we can verify the digital signature of Elastic packages using the public key.
`wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg`

2. Download the apt-transport-https. This allows apt to communicate over HTTPs, not just HTTP. This is important, as Elasticsearch is hosted over HTTP, not HTTPs.
`sudo apt-get install apt-transport-https`

2a. Selects the key we will use to verify the Debian package and where the repository can be found (link provided)
`echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-9.x.list`

3. Install the Elastic Package itself. Enter this command to both update the existing packages Ubuntu can access, and to install the Debian package.
`sudo apt-get update && sudo apt-get install elasticsearch`

## Lessons Learned

## Limitations

## Future Improvements
