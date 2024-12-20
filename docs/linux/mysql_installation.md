---
layout: default
title: MySQL Installation
parent: Linux
---

<link rel="stylesheet" type="text/css" href="/assets/css/code_form.css">

# How to Install MySQL on Ubuntu
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
* [https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04){:target="_blank"}

---

## Installation
### Update
{: .no_toc }
{% include ubuntu_update.md %}


### Install
{: .no_toc }
```bash
sudo apt install mysql-server
sudo systemctl start mysql.service
```
Warning: As of July 2022, an error will occur when you run the mysql_secure_installation script without some further configuration.

### Configure
{: .no_toc }

Because the `mysql_secure_installation` script performs a number of other actions that are useful for keeping your MySQL installation secure, it’s still recommended that you run it before you begin using MySQL to manage your data. To avoid entering this recursive loop, though, you’ll need to first adjust how your root MySQL user authenticates.
```bash
sudo mysql

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
exit
```
```bash
mysql -u root -p

ALTER USER 'root'@'localhost' IDENTIFIED WITH auth_socket;
exit
```

```bash
sudo mysql_secure_installation
```

Note that even though you’ve set a password for the root MySQL user, this user is not currently configured to authenticate with a password when connecting to the MySQL shell.

---

## Configuration
### Create A User
```bash
sudo mysql

CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```
{% include code_generator.md %}

### Create Database
```bash
CREATE DATABASE database;
```

### Grant User Access to Database
```bash
GRANT ALL PRIVILEGES ON database.* to 'user'@'localhost';
FLUSH PRIVILEGES;
```

---
