# 🛡️ Mini Security Operations Center Project 🛡️

## Table of Contents
1. [Project Summary](#project-summary)
2. [Architecture Diagram](#architecture-diagram)
3. [Prerequisites](#prerequisites)
4. [Deployment Instructions On Ubuntu](#deployment-instructions-on-ubuntu)
5. [Deployment Instructions On Windows](#deployment-instructions-on-windows)
6. [Lessons learned](#lessons-learned)
7. [Limitations](#limitations)
8. [Future Improvements](#future-improvements)

---

## Project Summary
A fully virtualised Security Operations Center (SOC) intended to simulate a real world enterprise Cybersecurity environment where Security Professionals can follow SOC Methodologies, such as performing log aggregation, risk scoring, threat analysis and triage. In this project, a Windows and Ubuntu VM will be used. Elasticsearch and Kibana are installed on the Ubuntu VM. Sysmon and Winlogbeat are installed on the Windows VM.

Elasticsearch is a tool that can receive logging data and convert it into a database. Kibana is a data analytics and visualisation software that can represent this data from Elasticsearch. This data can be queried using the Kibana Query Language (KQL) to parse out crucial information for threat hunting and can be analysed by Security Professionals. Sysmon expands regular log collection capabilities to include process creation, file changes, network connections and more. Winlogbeat feeds this log collection data enhanced by Sysmon into the Ubuntu VM, which is handled by Elasticsearch and is represented in Kibana.

Elastic Security is built into Kibana and can create detection rules. In this lab, detection rules have been configured to detect any suspicious system behaviour, such as data exfiltration, process injections and registry tampering.

---

## Architecture Diagram

## Prerequisites
| Requirement                  | Description                                      |
|------------------------------|--------------------------------------------------|
| **RAM**                      | At least **12 GB** spare (to run the VMs)        |
| **HDD Space**                | At least **130 GB** spare (storage on VMs)       |
| **Virtualization Software**  | Virtualbox                                       |
| **Virtual Disk Images**      | Windows (8/10/11) ISO and a Ubuntu (Debian) file |


## Deployment Instructions on Ubuntu
### Elasticsearch Installation

1. Install the Elastic Public key. This ensures that we can verify the digital signature of Elastic packages using the public key.
   
```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg`
```

2. Download the apt-transport-https for HTTPS communication (Elasticsearch is hosted on HTTPs) and retrieve key to verify Debian package

```bash
sudo apt-get install apt-transport-https
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-9.x.list
```

3. Install the Elastic Package itself. Enter this command to both update the existing packages Ubuntu can access, and to install the Debian package.

```bash
sudo apt-get update && sudo apt-get install elasticsearch
```

4. Navigate to /etc/elasticsearch to set `network-host` and `discovery.seed_hosts` to your ubuntu VM IP.
  <img width="536.25" height="374.4" alt="cfd8463f803a2a6f3888a11ee50e587b" src="https://github.com/user-attachments/assets/82416de7-5b53-4f65-8b0d-209733971d2c" />
  
5. Enable `xpack.security.enabled` to `true` and set `xpack.security.enrollment.enabled` to true

6. Check the status of Elasticsearch service. If it is inactive, we must enable it
```bash
systemctl status elasticsearch
```

7. Enable Elasticsearch if inactive.
```bash
systemctl start elasticsearch
```

8. Test Elasticsearch by navigating to `https://your-vm-ip:9200`
  <img width="536.25" height="374.4" alt="2ef605d9a3e6bce4afe56dcc21918b8d" src="https://github.com/user-attachments/assets/5efbad53-21a2-443c-8fa9-676705cf31b5" />
  
  Elasticsearch should now be fully installed!

  Note: Just remember, Elasticsearch is HTTPs; Kibana is HTTP.

### Kibana Installation
1. Install Kibana
```bash
sudo apt-get update && sudo apt-get install kibana
```
2. Navigate to /etc/kibana. Open the kibana.yml file. Replace `server.host` and `server.publicBaseUrl` to your Ubuntu VM IP address
<img width="536.25" height="374.4" alt="f98c1d1f9348f651c60c97b5605e7d5c" src="https://github.com/user-attachments/assets/d838dabd-1132-439d-970e-b74d080e96f0" />

3. Navigate to /usr/share/kibana. Generate encryption keys so Kibana can securely connect to Elasticsearch
```bash
bin/kibana-encryption-keys generate
```

4. Use the values generated from the kibana-encryption-keys command to fill the commands shown in the screenshot in /etc/kibana.
<img width="536.25" height="374.4" alt="cfd9671b663679be1cf4a6027d2169d4" src="https://github.com/user-attachments/assets/feb2469a-e7ae-4f6b-b017-a335800c6ad4" />

5. Integrate Kibana with Elasticsearch by creating and provisioning an Enrollment Token. Navigate to /usr/share/kibana and use this command in your terminal
  ```bash
  bin/elasticsearch-create-enrollment-token -s kibana
  ```
  This will create an enrollment token which you can use to connect to and incorporate Kibana's visualisation interface for Elasticsearch's backend

6. Configure Kibana to start automatically upon boot, and enable it.
  ```bash
  sudo /bin/systemctl daemon-reload
  sudo /bin/systemctl enable kibana.service
  sudo systemctl start kibana.service
  ```
To stop Kibana, simply replace 'start' with 'stop'.

7. Configure Kibana with Elasticsearch. Follow the link that appears in the status logs to start integrating Kibana with Elasticsearch.
  ```bash
  sudo systemctl status kibana.service
  ```

8. Copy paste the enrollment token you generated before into the web interface and click Configure. Optionally, provide the verfication code from the status command.
  
9. Upon arriving at the "Welcome to Elastic" page, configure username as elastic and paste in the password that was generated when setting up Elasticsearch. Login

## Detection Rules Configuration
1. 

## Deployment Instructions on Windows
### Sysmon Installation
1. Download [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
2. Navigate to this [Github Repository](https://github.com/SwiftOnSecurity/sysmon-config) and download the config file (XML)
3. Move Sysmon and sysmonconfig-export.xml into the same directory
4. Install Sysmon with the configuration file
```powershell
sysmon.exe -accepteula -i sysmonconfig-export.xml
```

### Winlogbeat installation
1. Download [Winlogbeat](https://www.elastic.co/downloads/beats/winlogbeat)
2. Open the winlogbeat.yml file and change the hosts attribute under Elasticsearch Output to the Ubuntu VM IP
<img width="536.25" height="374.4" alt="efcd570c359d7e8f0797283e015ec85a" src="https://github.com/user-attachments/assets/2cae0300-c791-4b88-bdb9-f5b73dd93dee" />

3. Go back to the Ubuntu VM: navigate to /etc/elasticsearch/certs. Take the http_ca.crt file (certificate authority) and email it to yourself so you can retrieve the file on the Windows VM
4. Set `ssl.certificate.authorities` to the folder that contains the certificate authority. This prevents any unknown certificate issue when winlogbeat
tries to send logging data to the Ubuntu VM
<img width="536.25" height="374.4" alt="b880c23758f3025569b919d8e5d2b63a" src="https://github.com/user-attachments/assets/2e070d32-49d0-4e1a-b488-8828ce80cb26" />

5. Finally, send logging data to the Ubuntu VM instance
```powershell
winlogbeat.exe -c winlogbeat.yml
```

### Atomic Red Team
1. Navigate to the directory you wish to store the atomic tests. Install the Atomic Red Team execution framework and the atomics folder in one line
```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics
```
2. Test to see if Atomic Red Team works by running a penetration test on your system
```powershell
Invoke-AtomicTest T1059.001
```
3. If the test is successful, be sure to perform cleanup to revert your system state back to normal
```powershell
Invoke-AtomicTest T1059.001 --Cleanup
```


## Lessons Learned

## Limitations

## Future Improvements
