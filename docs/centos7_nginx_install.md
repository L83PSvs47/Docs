---
title: "CentOS 7: Установка NGINX"
keywords: [Linux, CentOS, NGINX]
---

Добавить репозиторий:

```bash
vi /etc/yum.repos.d/nginx.repo
```

```ini
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

По умолчанию используется репозиторий для стабильной версии nginx. Если предпочтительно использовать пакеты для основной версии nginx, выполните следующую команду:

```bash
yum-config-manager --enable nginx-mainline
```

Установить nginx:

```bash
yum install nginx
```

При запросе подтверждения GPG-ключа проверьте, что отпечаток ключа совпадает с `573B FD6B 3D8F BC64 1079 A6AB ABF5 BD82 7BD9 BF62`, и, если это так, подтвердите его.

Запустить сервис и зарегистрировать его в автозагрузке:

```bash
systemctl start nginx
systemctl enable nginx
```

## Ссылки

[nginx: пакеты для Linux](https://nginx.org/ru/linux_packages.html)
