---
layout: default
title: Lets Encrypt
parent: Linux
---

# How to Install Lets Encrypt on Ubuntu
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
* [https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal)
* [https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-ubuntu-20-04)

---

## Installation
### Update
{: .no_toc }
{% include ubuntu_update.md %}

### Install
{: .no_toc }
```bash
sudo snap install core; sudo snap refresh core
sudo apt remove certbot
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

---

## Usage
### Certificate Only
#### Requires
{: .no_toc }
* FQDN
* DNS A-Record pointing to instance pubic ip
* Port 80 open

#### Store FQDN
{: .no_toc }
```bash
FQDN=www.example.com
```

#### Generate Certificate
{: .no_toc }
```bash
sudo certbot certonly --standalone -d ${FQDN}
```

#### Output:
{: .no_toc }
```bash
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/www.example.com/fullchain.pem
Key is saved at: /etc/letsencrypt/live/www.example.com/privkey.pem
```

---

## Renew Certificate
```bash
sudo certbot renew
```
```bash
FQDN=www.example.com
sudo certbot certonly -d ${FQDN} -n --standalone 
```
