---
layout: default
title: Docker Installation
parent: Linux
---

# How to Install Docker on Ubuntu
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
* [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

---

## Installation
### Update
{: .no_toc }
{% include ubuntu_update.md %}

### Uninstall Conflicts
{: .no_toc }
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

### Set Up Repository
{: .no_toc }
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

### Install Latest
{: .no_toc }
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## Post Installation
### Add User to Docker Group
{: .no_toc }
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
```

### Reload Group Membership
{: .no_toc }
Log out and log back in to re-evaluate group membership.

Alternatively, activate the changes to group:
```bash
newgrp docker
```

### Test
{: .no_toc }
```bash
docker version
docker compose version
```

---
