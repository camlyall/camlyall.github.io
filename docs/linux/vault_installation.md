---
layout: default
title: Vault Installation
parent: Linux
---

# How to Install Vault on Ubuntu
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
* [https://developer.hashicorp.com/vault/docs/install](https://developer.hashicorp.com/vault/docs/install){:target="_blank"}
* [https://developer.hashicorp.com/vault/downloads](https://developer.hashicorp.com/vault/downloads){:target="_blank"}

---

## Installation
### Update
{: .no_toc }
{% include ubuntu_update.md %}

### Install
{: .no_toc }
```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vault

# Check
vault version
```

---

## Configure
### Listen on Private IP
#### Get Private IP
{: .no_toc }
{% include private_ip.md %}

#### Edit Configuration File
{: .no_toc }
```bash
sudo nano /etc/vault.d/vault.hcl

# HTTP listener 
listener "tcp" { 
  address = "vault:8200" 
  tls_disable = 1 
} 
# HTTPS listener 
#listener "tcp" { 
#  address       = "vault:8200" 
#  tls_cert_file = "/opt/vault/tls/tls.crt" 
#  tls_key_file  = "/opt/vault/tls/tls.key" 
#}
```

#### Reload
{: .no_toc }
{% include ubuntu_enable.md content="vault"%}

#### Check Ports
{: .no_toc }
{% include listened_ports.md %}

---

## Initialise
### Store Vault Address
{: .no_toc }
#### Temporary
{: .no_toc }
```bash
export VAULT_ADDR=http://$(ip addr show ens5 | grep "inet\b" | awk '{print $2}' | cut -d/ -f1):8200
```

#### Stored
{: .no_toc }
```bash
echo "export VAULT_ADDR=http://$(ip addr show ens5 | grep "inet\b" | awk '{print $2}' | cut -d/ -f1):8200" | sudo tee -a /etc/environment
```

### Initialise
{: .no_toc }
```bash
vault operator init -key-shares=3 -key-threshold=2
```
Remember to store the output.

---

## Create Vaults
### Store Token
{: .no_toc }
```bash
export VAULT_TOKEN=<Vault Token>
```
### Create Key Value Vault
{: .no_toc }
```bash
vault secrets enable -version=2 -path=kv kv
```

---

## Configure AWS Auto Unseal
### Requires
{: .no_toc }
* AWS KMS Key
* AWS IAM Role Assigned to Instance

### Configure Vault
{: .no_toc }
```bash
sudo vim /etc/vault.d/vault.hcl

# Example AWS KMS auto unseal
seal "awskms" {
  region = "eu-west-2"
  kms_key_id = "01a23bc4-d5e6-78f9-g012-hi345678j9kk"
}
```

### Seal Vault
{: .no_toc }
```bash
vault operator seal
```

### Migrate
{: .no_toc }
```bash
vault operator unseal -migrate
```

### Check
{: .no_toc }
The vault should unseal automatically on restart.
```bash
sudo systemctl restart vault
vault status
```

---
