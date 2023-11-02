---
layout: default
title: Redis Installation
parent: Linux
---

# How to Install Redis on Ubuntu
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
* [https://redis.io/docs/getting-started/installation/install-redis-on-linux/](https://redis.io/docs/getting-started/installation/install-redis-on-linux/)

---

## Installation
### Update
{: .no_toc }
{% include ubuntu_update.md %}

### Install
{: .no_toc }
```bash
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg 
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list 
sudo apt-get update 
sudo apt install redis-server
```

---

## Configuration
### Listen on Private IP
#### Get Private IP
{: .no_toc }
{% include private_ip.md %}

#### Edit Configuration File
{: .no_toc }
```bash
sudo nano /etc/redis/redis.conf

# Find and change
bind 127.0.0.1 <Enter Private IP>
protected-mode no
supervised systemd
```

#### Reload
{: .no_toc }
```bash
sudo systemctl restart redis-server
sudo systemctl status redis-server
sudo systemctl enable redis-server
```

#### Check Ports
{: .no_toc }
{% include listened_ports.md %}

---
