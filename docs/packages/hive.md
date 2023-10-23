# Hive

Hive is used to control all the drones that capture traffic.
It can be controlled via WEB interface or REST API.

## Config

Example configuration file (default path: `/etc/tci/hive/config.toml`):

```conf
# vi: ft=config:

# Secret key to sign cookies (and JWT).
# Good way to generate the key:
#   python -c 'import secrets; print(secrets.token_hex())'
secret_key = "super secret key"

# Configuration of the database connection.
# DB will be connected in the following way:
#   type://user:pass@host/path?options
[db]
user = "tciguidb-user"
pass = "tciguidb-password"
host = "localhost"
path = "tciguidb"
type = "mysql"
options = "charset=utf8mb4"
# track_modifications = false
# track_record_queries = false

# LDAP connection for usert authentication.
# [ldap]
# uri = "ldap://localhost:389"

# API JWT settings.
# [api]
# access_token_expiry_time = 3600
# refresh_token_expiry_time = 604800

# Basic application settings.
[gui]
host = "0.0.0.0"
port = 8080
# threaded = true
# url = "custom-url.org"
# url_scheme = "https"

# PCAP Processing settings.
# [processing]
# pcap_folder = "./pcaps"
# tmp_folder = "./tmp"
# use_docker = true

# Logging settings
# [log]
# directory = '.'
# default_name = 'tcigui.log'
```

## Packages

Find packages [here](https://github.com/FETA-Project/TrafficCaptureInfrastructure/tree/main/packages/hive)

## Installation with RPM (OracleLinux 8)

It is necessary to install SQL database and set it up in [config [db]](#config)

### 1) Installing SQL DB
```bash
# installing and enabling the database
yum install -y mariadb-server
systemctl enable --now mariadb
```

### 2) Creating user and database with mysql
The credentials you use here should be the same as in `/etc/tci/hive/config.toml` in the `[db]` section.
```bash
# first we need to connect to the database (in this example using user 'root' with password '1234', default password is empty so could just omit "-p1234")
# it's setting up user 'tciguidb-user' with password 'tciguidb-password' on a database 'tciguidb' on localhost granting privileges only on the 'tciguidb' database
mysql -u root -ppass -e “
create user ‘tciguidb-user’@‘localhost’ indentified by ‘tciguidb-password’;
create database tciguidb;
grant all privileges on tciguidb.* to ‘tciguidb-user’@‘localhost’;
flush privileges;
“
```

### 4) Installing dependencies
```bash
# here we use mariadb, you could use other MYSQL database
yum install python3 python39-devel mariadb-devel
```

### 5) Installing the hive
Download the package version you want (see [packages](#packages)).
```bash
dnf -y install /path/to/tci_hive.rpm
```

### 6) Customization
Last step would be to customize the config (located in `/etc/tci/hive/config.toml`).
If you change the `[db]` part you should also redeploy the database (script provided in `/usr/bin/tci/hive/deploy_db.sh`)


## Installation Using Docker

Note: You can also run the whole system using docker compose (see [Getting Started](../getting_started.md#running-the-tci-system-with-docker-compose))

For correctly running, hive needs a databse.
You can run this in docker as well, for example:
`docker run -d mysql -e MYSQL_ROOT_PASSWORD=toor -e MYSQL_DATABASE=tciguidb -e MYSQL_USER=tciguidb-user -e MYSQL_PASSWORD=tciguidb-password`

First download the hive tar package (see [Packages](#packages)).
Extract the package and in the root folder run `make docker`.
This will build the docker image tci_hive:\<version\>.

### Supported Enviromental Variables

* `DB_USER`
    * default: `tciguidb-user`
* `DB_PASS`
    * default: `tciguidb-password`
* `DB_HOST`
    * default: `localhost`
* `DB_PATH`
    * default: `tciguidb`
* `DB_TYPE`
    * default: `mysql`
* `DB_OPTIONS`
    * default: `charset=utf8mb4`
* `LDAP_URI`
    * default: `ldap://localhost:389`
* `HOST`
    * default: `0.0.0.0`
* `PORT`
    * default: `8080`
* `OUTPUT_FOLDER`
    * directory to use for output files (pcaps)
    * default: `/pcaps/`
* `TMP_FOLDER`
    * directory to use for temporary files
    * default: `/tmp/`
* `LOG_DIR`
    * directory to use for logs
    * default: `/var/log/tci/hive/`

### Example of Running a Docker Container:
`docker run -d tci_hive:<version> -e PORT=8000 -e LOG_DIR=/logs/hive/`


## Installation from source
...