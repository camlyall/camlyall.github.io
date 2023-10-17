---
layout: default
title: Nats Installation
parent: Linux
---

# How to Install Nats on Ubuntu
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
* [https://github.com/nats-io/nats-server/releases/](https://github.com/nats-io/nats-server/releases/)

---

## Installation
### Update
{: .no_toc }
{% include ubuntu_update.md %}


### Download Latest 
{: .no_toc }
```bash
nats_file_url=$(curl -s https://api.github.com/repos/nats-io/nats-server/releases/latest | awk -F'"' '/browser_download_url.*amd64.deb/ {print $4}')
nats_file_name=$(basename "$nats_file_url")
echo $nats_file_url
echo $nats_file_name

sudo rm /tmp/$nats_file_name
wget -q "$nats_file_url" -P /tmp
```

### Install
{: .no_toc }
```bash
sudo dpkg -i /tmp/$nats_file_name
rm /tmp/$nats_file_name
```

### Test
{: .no_toc }
```bash
nats-server
```

---

## Configuration
### Listen on Private IP
#### Get Private IP
{: .no_toc }
{% include private_ip.md %}

#### Create Configuration File
{: .no_toc }
```bash
# Create nats-server.conf
sudo tee /etc/nats-server.conf > /dev/null <<EOF
host: $PRIV_IP
EOF
```

#### Check
{: .no_toc }
```bash
cat /etc/nats-server.conf
```

#### Restart
{: .no_toc }
```bash
sudo systemctl restart nats
```

#### Check Ports
{: .no_toc }
{% include listened_ports.md %}

---

## Service
### User
{: .no_toc }
```bash
sudo adduser --system --group --no-create-home --shell /bin/false nats
sudo chown -R nats:nats /usr/bin/nats-server /etc/nats-server.conf
```

### Service File
{: .no_toc }
```bash
sudo tee /etc/systemd/system/nats.service > /dev/null <<EOF
[Unit]  
Description=NATS Server  
After=network.target ntp.service

[Service]  
PrivateTmp=true 
Type=simple 
ExecStart=/usr/bin/nats-server -c /etc/nats-server.conf  
ExecReload=/bin/kill -s HUP $MAINPID  
ExecStop=/bin/kill -s SIGINT $MAINPID  
User=nats  
Group=nats

[Install]  
WantedBy=multi-user.target
EOF
```

### Reload
{: .no_toc }
{% include ubuntu_enable.md content="nats"%}
