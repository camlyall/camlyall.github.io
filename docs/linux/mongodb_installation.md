---
layout: default
title: MongoDB Installation
parent: Linux
---

# How to Install MongoDB on Ubuntu
{: .no_toc }

Last Modified: {% last_modified_at %}

<details open markdown="block">
  <summary>
   Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

{: .no_toc .text-delta }

---

## Documentation
* [https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/)

---

## Useful Files
Data: /var/lib/mongodb  
Logs: /var/log/mongodb  
Config: /etc/mongod.conf  

---

## Installation
### Update
{: .no_toc }
{% include ubuntu_update.md %}

### Clean
{: .no_toc }
```bash
sudo apt remove mongodb mongodb-org
sudo apt purge mongodb mongodb-org
sudo apt autoremove
```

### Install
{: .no_toc }
```bash
sudo apt-get install gnupg
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg \
   --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

sudo apt update
sudo apt install mongodb-org
```

#### Check
{: .no_toc }
```bash
mongod --version
```

---

## Configuration
```bash
# Increase vm.max_map_count
sudo vim /etc/sysctl.conf

# Add:
vm.max_map_count=262144

# Check 
sudo sysctl -p
```

---

## Service
{% include ubuntu_enable.md content="mongod"%}

---

## Setup
### View Databases
```bash
mongosh
show dbs
```

### Create Users
#### Admin
{: .no_toc }
```bash
# Create admin user 
use admin 
db.createUser(   
  {   
    user: "admin",   
    pwd: passwordPrompt(),   
    roles: [ { role: "root", db: "admin" }] 
  }   
)
```

#### Standard
{: .no_toc }
```bash
# Create admin user 
use "<some database>"
db.createUser( 
  { 
    user: "<some user>", 
    pwd: passwordPrompt(), 
    roles: [ { role: "readWrite", db: "<some database>" } ] 
  } 
)
```

#### Check
{: .no_toc }
```bash
show users
exit

# Test admin
mongoh -u admin

# Test pydio
mongosh -u "<some user>" --authenticationDatabase cells
```

---

## Secure & Configure for Remote Access
```bash
# Get private IP
ip addr show ens5 | grep "inet\b" | awk '{print $2}' | cut -d/ -f1

# Modify config
sudo nano /etc/mongod.conf

# Configure remote access 
net: 
  port: 27017 
  bindIp: 127.0.0.1, <Enter Private IP>

# Enable Authentication
security: 
  authorization: enabled
```

```bash
sudo systemctl restart mongod
sudo systemctl status mongod
```

```bash
# Verify it's running on private IP
sudo lsof -i -P -n | grep LISTEN
```

---
