---
layout: default
title: Linux Notes
parent: Linux
---

# Linux Notes
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
* [https://www.linuxtrainingacademy.com/linux-commands-cheat-sheet/](https://www.linuxtrainingacademy.com/linux-commands-cheat-sheet/){:target="_blank"}

---

## Stress Test
To stress linux OS on CPU and more.

4 processes run for 300 seconds & for background
```bash
sudo amazon-linux-extras install epel -y
sudo apt install stress -y

nohup stress -c 4 -t 300 &
```


## Networking 
### Listening Ports
Recommended: Net-tools
```bash
sudo apt update
sudo apt install net-tools
```

```bash
netstat -tuln
```

---
