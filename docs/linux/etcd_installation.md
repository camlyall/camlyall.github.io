---
layout: default
title: ETCD Installation
parent: Linux
---

# How to Install ETCD on Ubuntu
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
* [https://etcd.io/docs/v3.5/](https://etcd.io/docs/v3.5/)

---

## Installation
### Update
{: .no_toc }
{% include ubuntu_update.md %}

### Download latest
{: .no_toc }
```bash
etcd_file_url=$(curl -s https://api.github.com/repos/etcd-io/etcd/releases/latest | awk -F'"' '/browser_download_url.*linux-amd64.tar.gz/ {print $4}')
etcd_file_name=$(basename "$etcd_file_url")
echo $etcd_file_url
echo $etcd_file_name

sudo rm /tmp/$etcd_file_name
wget -q "$etcd_file_url" -P /tmp
```

### Install
{: .no_toc }
#### Extract
{: .no_toc }
```bash
sudo rm -r /tmp/etcd_extracted
mkdir /tmp/etcd_extracted
tar xzvf /tmp/$etcd_file_name -C /tmp/etcd_extracted --strip-components=1
```

#### Check
{: .no_toc }
```bash
/tmp/etcd_extracted/etcd --version
/tmp/etcd_extracted/etcdctl version
/tmp/etcd_extracted/etcdutl version
```

#### Install Gloablly
{: .no_toc }
```bash
sudo cp /tmp/etcd_extracted/etcd* /usr/local/bin/
etcd --version
etcdctl version
etcdutl version
```

---

## Service
### User
{: .no_toc }
```bash
sudo mkdir -p /etc/etcd /var/lib/etcd 
sudo groupadd -f -g 1501 etcd 
sudo useradd -c "etcd user" -d /var/lib/etcd -s /bin/false -g etcd -u 1501 etcd 
sudo chown -R etcd:etcd /var/lib/etcd
```

### Variables
{: .no_toc }
```bash
sudo -i

ETCD_HOST_IP=$(ip addr show ens5 | grep "inet\b" | awk '{print $2}' | cut -d/ -f1)
ETCD_NAME=$(hostname -s)

echo $ETCD_HOST_IP && echo $ETCD_NAME
# Example: 
# 172.31.47.178
# ip-172-31-47-178
```

### Service File
{: .no_toc }
```bash
cat << EOF > /lib/systemd/system/etcd.service  
[Unit]
Description=etcd service
Documentation=https://github.com/etcd-io/etcd
[Service]
User=etcd
Type=notify
ExecStart=/usr/local/bin/etcd \\
 --name ${ETCD_NAME} \\
 --data-dir /var/lib/etcd \\
 --initial-advertise-peer-urls http://${ETCD_HOST_IP}:2380 \\
 --listen-peer-urls http://${ETCD_HOST_IP}:2380 \\
 --listen-client-urls http://${ETCD_HOST_IP}:2379,http://127.0.0.1:2379 \\
 --advertise-client-urls http://${ETCD_HOST_IP}:2379 \\
 --initial-cluster-token etcd-cluster-token \\
 --initial-cluster ${ETCD_NAME}=http://${ETCD_HOST_IP}:2380 \\
 --initial-cluster-state new \\
 --heartbeat-interval 1000 \\
 --election-timeout 5000
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

### Enable
{: .no_toc }
{% include ubuntu_enable.md content="etcd"%}

### Test
{: .no_toc }
```bash
etcdctl put foo bar
# OK
etcdctl get foo
# foo
# bar
etcdctl del foo
# 1
```

---

## Monitor
```bash
ETCDCTL_API=3

etcdctl member list
etcdctl endpoint health
etcdctl endpoint status

# Check performance
etcdctl check perf
```
