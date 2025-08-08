---
layout: single
title: "Preparing Tricks for Attackers (Part 2): Hands-On Honeypot Deployment"
toc: true
excerpt: "Get your hands dirty with practical honeypot deployment using open-source tools. This Halloween special covers Cowrie SSH honeypots, Dionaea malware collection, and integration strategies."
header:
  overlay_image: /assets/images/post-honeypots-tricks-part2.jpg
  teaser: /assets/images/post-honeypots-tricks-part2.jpg
  overlay_filter: 0.5
---

Welcome back to our Halloween deception series! Last week we covered the strategic foundations of honeypot deployment. This week, we're rolling up our sleeves and building real honeypots using popular open-source tools. By the end of this post, you'll have working honeypots detecting and analyzing attacker behavior in your lab environment.

We'll focus on three battle-tested tools that provide different deception capabilities: Cowrie for SSH/Telnet deception, Dionaea for malware collection, and HoneyD for network service simulation.

## Lab Environment Setup

Before deploying honeypots in production, establish a safe lab environment for testing and validation. This prevents accidental exposure of production systems and allows experimentation with configurations.

### Network Isolation

Create an isolated network segment that can communicate with your monitoring infrastructure but remains separate from production systems:

```bash
# Create dedicated VLAN or subnet for honeypot testing
# Example network: 192.168.100.0/24
# Gateway: 192.168.100.1 (firewall with restricted routing)
# Honeypots: 192.168.100.10-50
# Monitoring: 192.168.100.100
```

### Base System Configuration

Use Ubuntu Server 20.04 LTS or CentOS 8 as your honeypot base. These distributions provide good package support and security update management:

```bash
# Update system and install essential packages
sudo apt update && sudo apt upgrade -y
sudo apt install -y git python3 python3-pip python3-venv build-essential
sudo apt install -y tcpdump wireshark-common tshark rsyslog

# Create dedicated honeypot user
sudo useradd -m -s /bin/bash honeypot
sudo usermod -aG sudo honeypot
```

## Cowrie SSH/Telnet Honeypot

Cowrie is a medium-to-high interaction SSH and Telnet honeypot designed to log brute force attacks and shell interaction performed by attackers.

### Installation and Basic Configuration

```bash
# Switch to honeypot user
sudo su - honeypot

# Create Python virtual environment
python3 -m venv cowrie-env
source cowrie-env/bin/activate

# Clone and install Cowrie
git clone https://github.com/cowrie/cowrie.git
cd cowrie
pip install --upgrade pip
pip install --upgrade -r requirements.txt

# Copy and customize configuration
cp etc/cowrie.cfg.dist etc/cowrie.cfg
```

### Strategic Configuration

Edit `etc/cowrie.cfg` to create realistic and attractive honeypot characteristics:

```ini
[honeypot]
hostname = web-server-01
log_path = var/log/cowrie
download_path = var/lib/cowrie/downloads
ttylog_path = var/lib/cowrie/tty
interactive_timeout = 180
authentication_timeout = 120

[ssh]
version = SSH-2.0-OpenSSH_7.4
listen_endpoints = tcp:2222:interface=0.0.0.0
sftp_enabled = true
forwarding = true

[telnet]
listen_endpoints = tcp:2323:interface=0.0.0.0
enabled = true

[output_jsonlog]
logfile = var/log/cowrie/cowrie.json

[output_syslog]
enabled = true
facility = daemon
priority = info
```

### User Account Configuration

Create believable user accounts that attackers might attempt to compromise:

```bash
# Edit etc/userdb.txt to add realistic accounts
echo "admin:x:123456:admin" >> etc/userdb.txt
echo "root:x:*:root" >> etc/userdb.txt
echo "ubuntu:x:ubuntu:ubuntu" >> etc/userdb.txt
echo "backup:x:backup123:backup" >> etc/userdb.txt
echo "oracle:x:oracle:oracle" >> etc/userdb.txt
```

### File System Deception

Cowrie includes a fake file system, but customization makes it more believable:

```bash
# Navigate to file system directory
cd honeyfs

# Create believable directory structure
mkdir -p home/admin/Documents
mkdir -p var/www/html
mkdir -p opt/database/backups
mkdir -p etc/ssl/private

# Add honey files with tempting names
echo "Database backup scheduled for midnight" > opt/database/backups/readme.txt
echo "Customer_Database_Backup_2025.sql" > opt/database/backups/customer_backup.sql
echo "SSL Certificate for production web server" > etc/ssl/private/server.key
```

### Starting and Monitoring Cowrie

```bash
# Start Cowrie
bin/cowrie start

# Monitor logs in real-time
tail -f var/log/cowrie/cowrie.json

# View interactive sessions
tail -f var/log/cowrie/cowrie.log
```

## Dionaea Malware Collection Platform

Dionaea is a low-interaction honeypot designed to trap malware. It emulates several services commonly targeted for malware deployment.

### Installation

```bash
# Install dependencies
sudo apt install -y python3-dev python3-pip git cmake build-essential
sudo apt install -y libglib2.0-dev libssl-dev libcurl4-openssl-dev
sudo apt install -y libnl-3-dev libnl-genl-3-dev libnl-nf-3-dev
sudo apt install -y libgc-dev libpcap-dev

# Create dionaea user and directory
sudo useradd -r -s /bin/false dionaea
sudo mkdir -p /opt/dionaea
sudo chown dionaea:dionaea /opt/dionaea

# Clone and build Dionaea
cd /tmp
git clone https://github.com/DinoTools/dionaea.git
cd dionaea
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/opt/dionaea ..
make
sudo make install
```

### Configuration

```bash
# Create main configuration
sudo tee /opt/dionaea/etc/dionaea/dionaea.cfg > /dev/null << 'EOF'
[dionaea]
download.dir=/opt/dionaea/var/lib/dionaea/binaries/
modules.python.dir=/opt/dionaea/lib/dionaea/python/
listen.mode=getifaddrs

[logging]
default.filename=/opt/dionaea/var/log/dionaea/dionaea.log
default.levels=info,warning,error
default.domains=*

[module.python]
sys_paths=default
service_configs=/opt/dionaea/etc/dionaea/services-enabled/*.yaml
ihandler_configs=/opt/dionaea/etc/dionaea/ihandlers-enabled/*.yaml
EOF

# Enable FTP service
sudo mkdir -p /opt/dionaea/etc/dionaea/services-enabled
sudo ln -s /opt/dionaea/etc/dionaea/services-available/ftp.yaml \
  /opt/dionaea/etc/dionaea/services-enabled/
```

### Service Configuration

Configure realistic service banners and responses:

```yaml
# /opt/dionaea/etc/dionaea/services-available/ftp.yaml
- name: ftp
  config:
    root: /opt/dionaea/var/lib/dionaea/root/ftp
    welcome_message: "Welcome to Corporate File Server FTP"
    max_attempts: 3
    timeout: 30
```

### Integration with Central Logging

```bash
# Configure rsyslog to forward Dionaea logs
sudo tee /etc/rsyslog.d/49-dionaea.conf > /dev/null << 'EOF'
# Dionaea honeypot logs
if $programname contains 'dionaea' then @@logserver.local:514
if $programname contains 'dionaea' then stop
EOF

sudo systemctl restart rsyslog
```

## HoneyD Network Service Simulation

HoneyD creates virtual hosts on a network and simulates TCP and UDP services on those hosts.

### Installation and Basic Setup

```bash
# Install HoneyD
sudo apt install -y honeyd

# Create configuration directory
sudo mkdir -p /etc/honeyd
sudo chown honeypot:honeypot /etc/honeyd
```

### Network Service Template

Create realistic service profiles that match your network environment:

```bash
# Create honeyd.conf
tee /etc/honeyd/honeyd.conf > /dev/null << 'EOF'
# Web server template
create webserver
set webserver personality "Linux 2.4.20"
set webserver default tcp action block
set webserver default udp action block
add webserver tcp port 80 "scripts/web.sh"
add webserver tcp port 443 "scripts/web.sh"
add webserver tcp port 22 "scripts/ssh.sh"

# File server template  
create fileserver
set fileserver personality "Windows XP SP2"
set fileserver default tcp action block
set fileserver default udp action block
add fileserver tcp port 445 "scripts/smb.sh"
add fileserver tcp port 139 "scripts/smb.sh"
add fileserver tcp port 21 "scripts/ftp.sh"

# Bind templates to IP addresses
bind 192.168.100.20 webserver
bind 192.168.100.21 fileserver
EOF
```

### Service Simulation Scripts

Create simple scripts that provide realistic service responses:

```bash
# Create scripts directory
sudo mkdir -p /usr/share/honeyd/scripts
cd /usr/share/honeyd/scripts

# Web server script
sudo tee web.sh > /dev/null << 'EOF'
#!/bin/bash
echo "HTTP/1.0 200 OK"
echo "Server: Apache/2.4.41 (Ubuntu)"
echo "Content-Type: text/html"
echo ""
echo "<html><head><title>Corporate Web Server</title></head>"
echo "<body><h1>Welcome to Corporate Intranet</h1></body></html>"
EOF

# SSH banner script
sudo tee ssh.sh > /dev/null << 'EOF'
#!/bin/bash
echo "SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.5"
sleep 2
EOF

sudo chmod +x *.sh
```

## Centralized Monitoring and Analysis

Integrate all honeypots with centralized logging and analysis systems.

### Elastic Stack Integration

```bash
# Install Filebeat for log forwarding
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.15.0-amd64.deb
sudo dpkg -i filebeat-7.15.0-amd64.deb

# Configure Filebeat for honeypot logs
sudo tee /etc/filebeat/filebeat.yml > /dev/null << 'EOF'
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /home/honeypot/cowrie/var/log/cowrie/cowrie.json
  json.keys_under_root: true
  json.add_error_key: true
  fields:
    honeypot_type: cowrie
    
- type: log
  enabled: true  
  paths:
    - /opt/dionaea/var/log/dionaea/dionaea.log
  fields:
    honeypot_type: dionaea

output.elasticsearch:
  hosts: ["elasticsearch.local:9200"]
  index: "honeypot-logs-%{+yyyy.MM.dd}"

processors:
  - add_host_metadata:
      when.not.contains.tags: forwarded
EOF

sudo systemctl enable filebeat
sudo systemctl start filebeat
```

### SIEM Integration

Create correlation rules for common attack patterns:

```json
{
  "rule_name": "Honeypot_Brute_Force_Attack",
  "description": "Multiple failed login attempts on honeypot",
  "query": "honeypot_type:cowrie AND eventid:cowrie.login.failed",
  "threshold": 5,
  "timeframe": "5m",
  "alert_severity": "medium"
}
```

## Operational Considerations

### Maintenance and Updates

```bash
# Create update script for all honeypots
tee /home/honeypot/update_honeypots.sh > /dev/null << 'EOF'
#!/bin/bash
# Update Cowrie
cd /home/honeypot/cowrie
source ../cowrie-env/bin/activate
git pull
pip install --upgrade -r requirements.txt

# Update Dionaea (requires rebuild)
echo "Dionaea updates require manual rebuild process"

# Restart services
bin/cowrie restart
sudo systemctl restart honeyd
EOF

chmod +x /home/honeypot/update_honeypots.sh
```

### Log Rotation and Storage

```bash
# Configure logrotate for honeypot logs
sudo tee /etc/logrotate.d/honeypots > /dev/null << 'EOF'
/home/honeypot/cowrie/var/log/cowrie/*.log {
    weekly
    rotate 12
    compress
    delaycompress
    missingok
    notifempty
    create 644 honeypot honeypot
}

/opt/dionaea/var/log/dionaea/*.log {
    weekly
    rotate 12
    compress
    delaycompress
    missingok
    notifempty  
    create 644 dionaea dionaea
}
EOF
```

## Testing Your Honeypots

Validate honeypot functionality with controlled testing:

```bash
# Test Cowrie SSH honeypot
ssh -p 2222 admin@honeypot-server
# Try common passwords: admin, password, 123456

# Test service simulation
nmap -sS -O honeypot-server
telnet honeypot-server 80

# Monitor logs for test activities
tail -f /home/honeypot/cowrie/var/log/cowrie/cowrie.json
```

## Next Week: Advanced Deception Techniques

In our final Halloween installment, we'll explore advanced deception techniques including:

- Distributed honeypot networks with centralized management
- Dynamic honeypot deployment based on threat intelligence  
- Automated response and threat hunting integration
- Legal and compliance considerations for deception technology

Your honeypots are now set and ready to catch some tricks! Remember to monitor them closely and tune configurations based on the types of attacks you observe.

*Need help integrating honeypots into your existing security infrastructure? Seguri's team has extensive experience deploying and managing deception technology that provides both detection capabilities and actionable threat intelligence.*