# Mamute
The image is based on the [**java:8-jdk**][java-container-url] base image, fetches [**Mamute**][mamute-url] version 1.4.0, extracts and places a startup script file that does all the magic.

## Purpose of container
Provide an easy way to deploy **Mamute** in production with very easy configuration options.
**Mamute** stores it's data in a [**MySQL**][mysql-url].

> **Attention!**
>
> **MySQL** version 5.7.4 works well with **Mamute** 1.4.0 any version above 5.7.4 will fail and break and cause the polar ice caps to melt

## Eager to try it?
Start a **MySQL** container:
```
docker run -d \
    --name mysql-mamute \
    -e MYSQL_ROOT_PASSWORD=secretpassword \
    -e MYSQL_DATABASE=mamute \
    -e MYSQL_USER=mamute \
    -e MYSQL_PASSWORD=mamute \
    mysql:5.7.4
```
.
Start a **Mamute** container:
```
docker run -d \
    --name mamute \
    --link mysql-mamute:mysql \
    -e MAMUTE_PORT=8080 \
    -p 8080:8080 \
    jbasila/mamute:1.4.0
```
.
This is enough to test **Mamute** on localhost on port 8080. Navigate to http://localhost:8080.

# Environment Variables
To configure the **Mamute** properly, the following environment variables can be used.

## Mamute behaviour
The following will control the behavior of **Mamute** execution:

| Environment Name                 | Default Value | Description                                                             |
| :------------------------------- | :------------ | :---------------------------------------------------------------------- |
| **MAMUTE_HOST**                  | localhost     | _Host Name/IP Address_ of Mamute (external IP/FQDN)                     |
| **MAMUTE_PORT**                  | 80            | Internal port to use, should not be changed                             |
| **MAMUTE_ENABLE_SIGNUP**         | true          | Enable signup for users. When using LDAP consider to disable it         |
| **MAMUTE_ALLOW_QUESTION_DELETE** | true          | Allow owner/moderator to delete own question or full thread (moderator) |
| **MAMUTE_ENABLE_SOLR**           | true          | Enable SOLR Indexing engine for enabling searching the database         |
| **MAMUTE_ATTACHMENTS_PATH**      | /tmp          | Location where attachments are stored, it's recommended to change it    |

## MySQL Database Configuration
The following control the parameters of the MySQL database to connect to and store the data in:

| Environment Name | Default Value | Description                                       |
| :--------------- | :------------ | :------------------------------------------------ |
| **DB_HOST**      | mysql         | _Host Name/IP Address_ of the MySQL server to use |
| **DB_PORT**      | 3306          | The MySQL port to use                             |
| **DB_USER**      | mamute        | The database user to use                          |
| **DB_PWD**       | mamute        | The password of the database to use               |
| **DB_NAME**      | mamute        | The database name to use to store **Mamute** data |

## Mail Server Configuration
The following control the mail server configuration:

| Environment Name   | Default Value      | Description                                        |
| :----------------- | :----------------- | :------------------------------------------------- |
| **MAIL_SERVER**    | smtp.sample.server | _Host Name/IP Address_ of the mail server          |
| **MAIL_PORT**      | 25                 | SMTP server port to use                            |
| **MAIL_USE_TLS**   | false              | Enable the use of TLS                              |
| **MAIL_USERNAME**  | user               | The username to use for authenticated mail server  |
| **MAIL_PASSWORD**  | password           | The password to use for authenticated mail server  |
| **MAIL_FROM**      | no-reply@null.com  | The e-mail address to use in the automated e-mails |
| **MAIL_FROM_NAME** |"Mamute System"     | The e-mail name to use for automated e-mails       |

## LDAP Authentication Configuration

| Environment Name         | Default Value                               | Description                                          |
| :----------------------- | :------------------------------------------ | :--------------------------------------------------- |
| **LDAP_PORT**            | 389                                         | LDAP port                                            |
| **LDAP_USER**            | user                                        | LDAP DN to use for authentication                    |
| **LDAP_PASS**            | pass                                        | LDAP password to use for authentication              |
| **LDAP_USERDN**          | "OU=Users,DC=company,DC=com"                | The LDAP branch to search users in                   |
| **LDAP_GROUP_ATTR**      | memberOf                                    | The attribute to use for checking moderator gorup in |
| **LDAP_MODERATOR_GROUP** | "CN=Moderators,OU=Groups,DC=company,DC=com" | The membership that grants moderator access |
| **LDAP_USE_SSL**         | false                                       | Enable the use of SSL |


[mamute-url]: http://www.mamute.org/
[java-container-url]: https://hub.docker.com/_/java/
[mysql-url]: https://www.mysql.com/
