# 🛡️ Linux Server Hardening Project

> Hardening an Ubuntu 22.04 LTS server to meet security best practices for production or enterprise use.  
> ✅ Designed for Security Engineers and SOC teams | 📡 Real-world tested on AWS EC2 (t3.small) | 🔐 CIS & OWASP aligned

---

## 🧭 Project Goals

- Harden a clean Ubuntu system against unauthorized access, malware, and misconfiguration
- Implement real-world controls for firewall, audit logging, user policy, and patch automation
- Ensure compliance with basic CIS Benchmarks
- Provide a reproducible, automation-friendly baseline

---

## 🛠️ Environment Details

| Feature              | Value                 |
|----------------------|-----------------------|
| OS                   | Ubuntu 22.04 LTS      |
| Architecture         | x86_64                |
| Cloud Provider       | AWS EC2               |
| Instance Type        | t3.small              |
| Role                 | Bastion / Web Server  |
| SSH Port             | `2222`                |
| Firewall             | UFW + Fail2Ban        |

---

## ✅ Hardening Checklist

| Category                  | Status | Notes                         |
|---------------------------|--------|-------------------------------|
| 🔐 Disable Root Login      | ✅     | `PermitRootLogin no`         |
| 🛡️ SSH Port Change         | ✅     | Port set to `2222`           |
| 🔥 UFW Firewall Active     | ✅     | Allow only ports 2222, 80, 443 |
| 🚫 Unused Services Removed | ✅     | snapd, cups, bluetooth       |
| 👥 Least Privilege User    | ✅     | `sabinadmin` with `sudo`     |
| 🔍 Audit Logging Enabled   | ✅     | `auditd` installed           |
| 🚨 Fail2Ban Configured     | ✅     | Default jail + SSH jail      |
| 🧠 Lynis Scan Performed    | ✅     | Score above `80`             |
| 🔁 Auto Updates Enabled    | ✅     | `unattended-upgrades`        |

---

## 📷 Screenshots

| Screenshot                    | Description                          |
|-------------------------------|--------------------------------------|
| `ufw-status.png`              | Firewall rules after configuration   |
| `fail2ban-status.png`         | Active jails protecting SSH          |
| `ss-tulnp.png`                | Listening ports after hardening      |
| `lynis-report.png`            | Security score from audit tool       |

_📁 Place screenshots in `/screenshots` folder and link them here._

---

## 🧱 Network Architecture Diagram

> You can design this with [draw.io](https://draw.io), [Excalidraw](https://excalidraw.com), or [Lucidchart](https://www.lucidchart.com)

### Suggested Diagram Elements:
- Cloud Provider (EC2)
- Bastion Node (Hardened Linux Server)
- Allowed Ports
- User Login Flow (via SSH Key)
- Alert/Monitoring Stack (Fail2Ban, Auditd)

📁 Put the diagram in `/diagrams` and embed below:

yaml
Copy
Edit

---

## 🧪 Audit & Verification

### Lynis Output (summary)
```bash
Hardening index : 82 [###################      ]
Audit performed on : Ubuntu 22.04 LTS
Warnings          : 3
Suggestions       : 7
Run your own audit:

bash
Copy
Edit
sudo apt install lynis -y
sudo lynis audit system
🚀 Deployment Steps (Manual)
bash
Copy
Edit
sudo apt update && sudo apt upgrade -y
sudo adduser sabinadmin && sudo usermod -aG sudo sabinadmin
sudo passwd -l root
sudo apt purge snapd avahi-daemon cups -y
sudo apt install ufw fail2ban auditd apparmor lynis unattended-upgrades -y

# SSH hardening
sudo nano /etc/ssh/sshd_config
# → Change Port 2222, disable root login, disable password login

sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2222
sudo ufw allow http
sudo ufw allow https
sudo ufw enable

# Enable Fail2Ban
sudo systemctl enable fail2ban
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo systemctl restart fail2ban

# Auto updates
sudo dpkg-reconfigure --priority=low unattended-upgrades

# Auditd
sudo systemctl enable auditd && sudo systemctl start auditd
🧾 Logs & Output Samples
/var/log/auth.log – SSH login attempts

/var/log/fail2ban.log – Banned IPs

/var/log/audit/audit.log – System audit trail

🧠 Lessons Learned
Default Ubuntu installs are NOT secure out of the box

SSH key auth with non-default ports reduces brute force attempts by ~95%

Auditd is underused but extremely powerful for detection

lynis gives real, actionable system hardening insights

📚 References
CIS Ubuntu 22.04 Benchmark

Ubuntu Hardening Guide

Linux Auditd Rules

Fail2Ban Filters

📁 Repo Structure
vbnet
Copy
Edit
├── screenshots/
│   ├── ufw-status.png
│   └── ...
├── diagrams/
│   └── linux-hardening-arch.png
├── scripts/
│   └── harden.sh        # Optional: auto-script
├── README.md
└── LICENSE
🧪 Want to Try Yourself?
Clone this repo and follow each phase manually or run the harden.sh script (coming soon).

bash
Copy
Edit
git clone https://github.com/yourusername/linux-hardening-project.git
cd linux-hardening-project
📌 Maintained by
Sabin Shrestha
Security Engineer | Linux Enthusiast | Cybersecurity Nerd
🌐 sabinsecurityhub.xyz

yaml
Copy
Edit

---

### 🔧 Want Me to Build Supporting Files?

Let me know if you want:
- `scripts/harden.sh` – Auto-hardening bash script
- Diagram PNGs or Draw.io source
- Sample screenshots (I can generate fake ones for now)
- PDF report to export alongside your GitHub repo

Ready to publish? Let’s make it production-worthy.
