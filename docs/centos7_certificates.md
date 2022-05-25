---
title: "CentOS 7: Сертификаты"
keywords: [Linux, CentOS, Certificate, Сертификаты]
---

## Введение

### RSA

RSA (аббревиатура от фамилий Rivest, Shamir и Adleman) — криптографический алгоритм с открытым ключом.

Основная предпосылка, которая привела к появлению шифрования с открытым ключом, заключалась в том, что отправитель сообщения (тот, кто шифрует сообщение), не обязательно должен быть способен его расшифровать. То есть даже имея исходное сообщение, ключ, с помощью которого оно шифровалось, и зная алгоритм шифрования, он не сможет расшифровать закрытое сообщение без знания ключа расшифрования.

Суть шифрования с открытым ключом заключается в том, что для шифрования данных используется один ключ, а для расшифрования другой (поэтому такие системы часто называют асимметричными).

Если для шифрования используется открытый ключ (public key), то для дешифрования используется закрытый (private key), и наоборот.

### Параметры обмена ключами Диффи-Хеллмана

Параметры обмена ключами Диффи-Хеллмана (Diffie-Hellman key exchange parameter) DH.

Генерация ключа DH нужна для того, чтобы при утечке закрытого ключа в асимметричном шифровании, все прошлые и будущие сообщения нельзя было бы расшифровать.

Использование DH позволяет создавать одноразовый ключ при каждом новом соединении. Таким образом, даже если хакеру достанется текущий закрытый ключ, он сможет расшифровать только текущую сессию, но не предыдущие или будущие сессии.

На данный момент 1024-битные параметры Диффи-Хеллмана считаются слабыми.

### PEM

PEM (Privacy Enhanced Mail) — это формат контейнера, который может включать только открытый сертификат или может включать целую цепочку сертификатов, включая открытый ключ, закрытый ключ и корневые сертификаты. Де-факто является форматом файла для хранения и отправки криптографических ключей, сертификатов и других данных, основанный на наборе стандартов IETF 1993 года, определяющих «почту с улучшенной конфиденциальностью». Хотя исходные стандарты так и не получили широкого распространения и были вытеснены PGP и S/MIME, определённая ими текстовая кодировка стала очень популярной. Формат PEM был в конечном итоге формализован IETF в RFC 7468.

Файлы PEM формата могут иметь следующие расширения: `.pem`, `.crt`, `.cer` (открытый ключ), и `.key` (закрытый ключ). Они представляют собой ASCII файлы, закодированные по схеме Base64. Если открыть такой файл в текстовом редакторе, то можно увидеть, что текст кода в нём начинается с тега `----- BEGIN CERTIFICATE -----` и заканчивая тегом `----- END CERTIFICATE -----`.

Файл в формате PEM, с расширением `.key` содержит только закрытый ключ конкретного сертификата, и является просто условным именем, а не стандартизованным.

Файл в формате PEM, с расширением `.csr` содержит данные запроса сертификата в зашифрованном виде, например, такие как: страна, штат, организация, домен, e-mail адрес и открытый ключ, которые используются Центрами Сертификации (Certificate Authority) для проверки WEB-сайтов. CSR файлы генерируются с использованием открытых и закрытых ключей.

### OpenSSL

Для генерации сертификатов используется утилита `opensssl`.

Основные ключи утилиты и их опции:

**s_client** — команда `s_client` реализует общий клиент SSL/TLS, который подключается к удаленному хосту с помощью SSL/TLS. Это очень полезный инструмент диагностики для серверов SSL.
**-connect host:port** — указывает хост и необязательный порт для подключения. Вместо этого можно выбрать хост и порт, используя необязательный целевой позиционный аргумент. Если ни этот, ни целевой позиционный аргумент не указаны, выполняется попытка подключения к локальному хосту через порт 4433.

**dhparam** — команда используется для управления файлами параметров DH.
**-out filename** — имя выходного файла. Имя выходного файла не должно совпадать с именем входного файла.
**2048** — размер параметров DH. Последний параметр в строке.

**req** — команда в первую очередь создаёт и обрабатывает запросы сертификатов в формате `PKCS #10`. Кроме того, она может создавать самозаверяющие сертификаты, например, для использования в качестве корневых центров сертификации.
**-x509** — опция создаёт самоподписанный сертификат вместо запроса сертификата. Обычно это используется для создания тестового сертификата или самоподписанного корневого ЦС.
**-newkey arg** — опция создаёт новый запрос сертификата и новый закрытый ключ. Аргумент принимает одну из нескольких форм. `rsa: nbits`, где nbits — количество бит, генерируемого ключа RSA.
**-days n** — количество дней действия сертификата при использовании опции `-x509`, в противном случае игнорируется.
**-nodes** — если этот параметр указан, то при создании закрытого ключа он не будет зашифрован, противном случае будет предложено ввести пароль. Теоретически можно опустить параметр `-nodes` (что означает "нет шифрования DES"), и в этом случае закрытый ключ будет зашифрован паролем. Однако это почти никогда не бывает полезно для установки сервера, потому что вам придётся либо хранить пароль на сервере, либо вводить его вручную при каждой перезагрузке.
**-keyout filename** — имя файла для записи закрытого ключа.
**-out filename** — имя файла для записи открытого ключа или запроса на сертификат.
**-subj** — устанавливает имя субъекта для нового запроса или заменяет имя субъекта при обработке запроса. Подавляет вопросы о содержимом сертификата.

## Генерация сертификатов

Создать каталог для DH:

```bash
mkdir -p /etc/ssl/dhparam
```

Создать 2048-битный ключ DH:

```bash
openssl dhparam -out /etc/ssl/dhparam/dhparam.pem 2048
```

Создать каталог для размещения сертификатов:

```bash
mkdir -p /etc/ssl/cert_dir
```

Создать самоподписанный сертификат (обратите внимание на наличие опции `-x509`):

```bash
openssl req -x509 -newkey rsa:2048 -sha256 -days 365 -nodes -keyout /etc/ssl/cert_dir/example.key -out /etc/ssl/cert_dir/example.crt
```

```output
Country Name (2 letter code) [XX]:RU
State or Province Name (full name) []:Moscow
Locality Name (eg, city) [Default City]:Moscow
Organization Name (eg, company) [Default Company Ltd]:Example
Organizational Unit Name (eg, section) []:IT
Common Name (eg, your name or your server's hostname) []:phpipam.example.com
Email Address []:admin@example.com
```

Создать запрос на сертификат (обратите внимание на отсутствие опции `-x509`):

```bash
openssl req -newkey rsa:2048 -sha256 -nodes -keyout /etc/ssl/cert_dir/example.key -out /etc/ssl/cert_dir/example.csr
```

```output
Country Name (2 letter code) [XX]:RU
State or Province Name (full name) []:Moscow
Locality Name (eg, city) [Default City]:Moscow
Organization Name (eg, company) [Default Company Ltd]:Example
Organizational Unit Name (eg, section) []:IT
Common Name (eg, your name or your server's hostname) []:phpipam.example.com
Email Address []:admin@example.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:CertificatePassword
An optional company name []:Example
```

Подписать запрос довернным ЦС (Windows Server):

```bash
certreq -submit -attrib "CertificateTemplate:WebServer" "C:\cert\teampass.csr" "C:\cert\teampass.crt"
```

Создать сертификат, который действителен для доменов example.com и example.net (SAN), для IP адреса 10.0.0.1 (SAN), имеет сильное шифрование и действителен в течение 3650 дней (~10 лет) и не требует интерактивного ввода:

```bash
openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -nodes -keyout example.key -out example.crt -subj /CN=example.com -addext subjectAltName=DNS:example.com,DNS:example.net,IP:10.0.0.1
```

Проверить какой сертификат используется для LDAP SSL (LDAPS):

```bash
openssl s_client -connect myldapsserver.domain.com:636
```

Доверенное хранилище корневых сертификатов: `/etc/pki/ca-trust/source/anchors`

Обновить информацию о корневых сертификатах после добавления нового сертификата в корневое хранилище:

```bash
update-ca-trust
```

## Конвертация SSL сертификатов в OpenSSL

Данные команды OpenSSL дают возможность преобразовать сертификаты и ключи в разные форматы. Для того чтобы сделать их совместимыми с определенными видами серверов, либо ПО. К примеру, Вам необходимо конвертировать обыкновенный файл PEM, который будет работать с Apache, в формат PFX (PKCS # 12) с целью применения его с Tomcat, либо IIS.

Конвертировать PEM в DER:

```bash
openssl x509 -outform der -in certificate.pem -out certificate.der
```

Конвертировать PEM в P7B:

```bash
openssl crl2pkcs7 -nocrl -certfile certificate.cer -out certificate.p7b -certfile CACert.cer
```

Конвертировать PEM в PFX:

```bash
openssl pkcs12 -export -out certificate.pfx -inkey privateKey.key -in certificate.crt -certfile CACert.crt
```

Конвертировать DER в PEM:

```bash
openssl x509 -inform der -in certificate.cer -out certificate.pem
```

Конвертировать P7B в PEM:

```bash
openssl pkcs7 -print_certs -in certificate.p7b -out certificate.cer
```

Конвертировать P7B в PFX:

```bash
openssl pkcs7 -print_certs -in certificate.p7b -out certificate.ceropenssl pkcs12 -export -in certificate.cer -inkey privateKey.key -out certificate.pfx -certfile CACert.cer
```

Конвертировать PFX в PEM:

```bash
openssl pkcs12 -in certificate.pfx -out certificate.cer -nodes
```

## Ссылки

[OpenSSL commands](https://www.openssl.org/docs/man1.1.1/man1/)
[Формат SSL сертификата: как конвертировать сертификат в .pem, .cer, .crt, .der, pkcs или pfx?](https://www.emaro-ssl.ru/blog/convert-ssl-certificate-formats/)