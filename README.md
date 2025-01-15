# Project Report: Cybersecurity - Network Attack Defense System  

---

## 1. Agenda  
1. Overview  
2. Scope of Work  
3. Approach  
4. System Implementation  
5. Demonstrations  

---

## 2. Overview  

![image](https://gist.github.com/user-attachments/assets/de6f77db-81d3-4aa6-ab42-2625b7d5e728)

- The project focuses on detecting and mitigating **Advanced Persistent Threats (APTs)** using a hybrid honeypot architecture.  
- Combines **external** and **internal honeypots** to deceive attackers and monitor their activities.  
- Includes additional protection layers like **Firewalls** and **Web Application Firewalls (WAFs)** to block malicious traffic.  

---

## 3. Scope of Work  
- Build a **multi-layered honeypot system**:  
  - **External honeypots**: High-interaction honeypots (SSH, Telnet).  
  - **Internal honeypots**: Web apps behind firewalls (e.g., Django-based honeypots).  
  - **Local network honeypots**: Simulating internal attacks.  
- Simulate attack techniques using **MITRE ATT&CK framework**.  

---

## 4. Approach  
### Honeypots  
- **External honeypots**:  
  - Use **Cowrie** for SSH/Telnet.  
  - Use **Dionaea** for HTTP, FTP, and SMB attacks.  
- **Web honeypots**:  
  - Employ **Django Honeypot** for brute-force attack simulation.  
- **Local honeypots**:  
  - Use **KFSensor** for medium-interaction honeypots in internal networks.  

### System Infrastructure  
- **Kubernetes**: Deploy honeypots and services in containers.  
- **WAF**: Use Nginx Ingress with ModSecurity for traffic filtering and domain management.  
- **Firewall**: Deploy **Pfsense** for port scan detection and IP/domain filtering.  
- **Log Analysis**: Utilize ELK Stack (Elasticsearch, Logstash, Kibana) for log collection and analysis.  

---

## 5. System Implementation  
### Honeypot Details  
- **Cowrie**: Logs SSH/Telnet activity, records credentials, and tracks executed commands.  
- **Dionaea**: Focused on HTTP, FTP, SMB attacks.  
- **KFSensor**: Simulates mail servers and monitors port scans on Windows.  

### Firewall  
- Detects port scans, blocks malicious IPs/domains.  
- Provides first-line defense.  

### WAF  
- Filters malicious requests using **OWASP ModSecurity Core Rule Set**.  
- Tested using **DVWA (Damn Vulnerable Web App)**.  

### Log Collection  
- **Cowrie** and **Dionaea** generate JSON logs.  
- Logs are forwarded to Elasticsearch via Filebeat.  
- Kibana dashboards provide insights into attack behavior and system vulnerabilities.  

---

## 6. Demonstrations and Scenarios  
### 1. Cowrie Honeypot  
- Attracts attackers with simulated filesystems.  
- Records all attacker actions, such as credentials used, commands executed, and file modifications.  

### 2. Django Web Honeypot  
- Replaces real login pages with fake ones to capture brute-force attempts.  
- Logs include IP addresses, User-Agent, and login behavior.  

### 3. ICMP Exfiltration via SSH  
- Exploit downloaded via `curl` executed to exfiltrate sensitive data over ICMP.  
- Scripts like `download.sh` automate the attack.  

### 4. Local Network Honeypots  
- KFSensor simulates a mail server.  
- Monitors attacker actions during port scans and captures related activity.  

---

## 7. Tools and Technologies  
- **Honeypots**: Cowrie, Dionaea, KFSensor.  
- **Firewall**: Pfsense.  
- **WAF**: ModSecurity with Nginx Ingress.  
- **Log Analysis**: ELK Stack (Filebeat, Elasticsearch, Kibana).  
- **Simulation Environment**: Kubernetes, DVWA, custom vulnerable web apps.  

---

