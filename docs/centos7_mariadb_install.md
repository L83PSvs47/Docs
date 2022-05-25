---
title: "CentOS 7: Установка MariaDB"
keywords: [Linux, CentOS, MySQL, MariaDB]
---

Добавить репозиторий:

```bash
yum install wget
wget https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
chmod +x mariadb_repo_setup
./mariadb_repo_setup
```

Установить сервер:

```bash
yum install MariaDB-server
```

Запустить сервис и зарегистририровать его в автозагрузке:

```bash
systemctl start mariadb
systemctl enable mariadb
```

Произвести предварительную настройку:

```bash
mariadb-secure-installation
```

```output
Enter current password for root (enter for none): enter
Switch to unix_socket authentication [Y/n] n
Change the root password? [Y/n] y
New password: root-DB-Password
Re-enter new password: root-DB-Password
Remove anonymous users? [Y/n] y
Disallow root login remotely? [Y/n] y
Remove test database and access to it? [Y/n] y
Reload privilege tables now? [Y/n] y
```

## Ссылки

[How to install MariaDB on CentOS 7 / RHEL 7](https://mariadb.com/resources/blog/installing-mariadb-10-on-centos-7-rhel-7/)
