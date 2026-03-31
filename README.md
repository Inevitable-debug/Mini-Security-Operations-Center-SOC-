# Project Summary
A fully virtualised Security Operations Center (SOC) intended to simulate a real world enterprise Cybersecurity environment where Security Professionals can follow SOC Methodologies, such as performing log aggregation, risk scoring, threat analysis and triage. In this project, a Windows and Ubuntu VM will be used. Elasticsearch and Kibana are installed on the Ubuntu VM. Sysmon and Winlogbeat are installed on the Windows VM.

Elasticsearch is a tool that can receive logging data and convert it into a database. Kibana is a data analytics and visualisation software that can represent this data from Elasticsearch. This data can be queried using the Kibana Query Language (KQL) to parse out crucial information for threat hunting and can be analysed by Security Professionals. Sysmon expands regular log collection capabilities to include process creation, file changes, network connections and more. Winlogbeat feeds this log collection data enhanced by Sysmon into the Ubuntu VM, which is handled by Elasticsearch and is represented in Kibana.

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

3. Download the apt-transport-https. This allows apt to communicate over HTTPs, not just HTTP. This is important, as Elasticsearch is hosted over HTTP, not HTTPs.

`sudo apt-get install apt-transport-https`

2a. Selects the key we will use to verify the Debian package and where the repository can be found (link provided)

`echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-9.x.list`

3. Install the Elastic Package itself. Enter this command to both update the existing packages Ubuntu can access, and to install the Debian package.

`sudo apt-get update && sudo apt-get install elasticsearch`

4. Navigate to /etc/elasticsearch to set some parameters.
4a. Set `network-host: 0.0.0.0` to `network-host: your-ubuntu-vm-ipv4-address`
<img width="412.5" height="288" alt="cfd8463f803a2a6f3888a11ee50e587b" src="https://github.com/user-attachments/assets/82416de7-5b53-4f65-8b0d-209733971d2c" />
4b. Set `discovery.seed_hosts:["0.0.0.0"]` to your Ubuntu VM ip as well.
4c. Enable `xpack.security.enabled` to `true` and set `xpack.security.enrollment.enabled` to true

5. Start the Elasticsearch service
5a. Check whether the Elasticsearch status is inactive / disabled. It should be inactive, and we shall enable it.
`systemctl status elasticsearch`
5b. Enable Elasticsearch. Give it 1-2 minutes to start properly.
`systemctl start elasticsearch`
5c. Test Elasticsearch by navigating to `https://your-vm-ip:9200`
<img width="823" height="472.5" alt="2ef605d9a3e6bce4afe56dcc21918b8d" src="https://github.com/user-attachments/assets/5efbad53-21a2-443c-8fa9-676705cf31b5" />

Elasticsearch should now be fully installed!

Note: Just remember, Elasticsearch is HTTPs; Kibana is HTTP.

## Lessons Learned

## Limitations

## Future Improvements
