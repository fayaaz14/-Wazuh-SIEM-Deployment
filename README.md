# -Wazuh-SIEM-Deployment
 This project demonstrates the deployment of **Wazuh SIEM** in a virtualized lab environment, connecting an agent from **Kali Linux**, and detecting a simulated **SSH brute-force attack** in real time. 
The goal of this project was to:  
- Understand **SIEM deployment & configuration**  
- Monitor logs from a Linux endpoint (Kali agent)  
- Simulate an **attack scenario (SSH brute force)**  
- Detect and analyze alerts in the **Wazuh Dashboard**  

---

## ⚙️ Lab Setup  
- **Host Machine**: Windows 11
- **Virtualization**: VirtualBox (Bridged Network Mode)  
- **VMs Used**:  
  - **Ubuntu Server** → Wazuh Manager + Dashboard  
  - **Kali Linux** → Wazuh Agent + Attack simulation  

---

## 🏗️ Installation Steps  

### 1. Install Wazuh Server (Ubuntu)  
```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

Dashboard runs on:  
```
https://<Ubuntu_IP>:443
```

---

### 2. Install Wazuh Agent (Kali)  
```bash
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.12.0-1_amd64.deb
sudo WAZUH_MANAGER='YOUR IP' WAZUH_AGENT_NAME='AGENT NAME' dpkg -i ./wazuh-agent_4.12.0-1_amd64.deb
```

Enable and start the agent:  
```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

✅ Now Kali agent appears in Wazuh Dashboard.  

---

## 🔐 Attack Simulation  

From Kali (attacker), simulate SSH brute force against Ubuntu server:  

```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://YOUR IP
```

(or multiple failed SSH attempts using `ssh root@YOUR IP`)  

---

## 📊 Detection in Wazuh  

- Wazuh collects logs from `/var/log/auth.log` on Kali  
- Failed SSH login attempts generate **security alerts**  
- Alerts visible in **Wazuh Dashboard → Security Events**  

📸 *[Insert screenshot: Wazuh showing SSH brute force detection]*  

---

## 📌 Key Learnings  
- Deploying **SIEM from scratch** (Wazuh setup + agents)  
- Configuring **networking (bridged mode, HTTPS access)**  
- Simulating **realistic attack (SSH brute force)**  
- Analyzing **alerts & log sources**  

---
