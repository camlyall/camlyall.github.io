---
layout: default
title: Go Installation
parent: Linux
---

# How to Install Go on Ubuntu
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
* [https://go.dev/doc/install](https://go.dev/doc/install)

---

## Installation
### Update
{: .no_toc }
{% include ubuntu_update.md %}

### Install
{: .no_toc }
```bash
GO_VERSION=1.21.3

wget https://go.dev/dl/go${GO_VERSION}.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go${GO_VERSION}.linux-amd64.tar.gz
```

### Enable for Current User
{: .no_toc }
```bash
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.profile
source ~/.profile
```

### Test
{: .no_toc }
```bash
go version
```
