[Home](../../) | [Alpine Linux](../../alpine-linux/) | [Docker](../../docker/) | [S3CMD](../../s3cmd/) | [Go](../go/) | [System](../../system/) | [Security](../../security/) | [MySQL](../../mysql/) | [InfluxDB](../../influxdb/)

- Install docker-compose

- Running MySQL via docker-compose.

- Create the following directories

```
	mkdir -p ~/docker/etc/mysql/conf.d
```

```
	mkdir -p ~/docker/var/lib/mysql
```

- To create a docker compose service for mysql with version 5.7.23 create a file named mysql.yml

```
	nano ~/docker/mysql.yml
```

- Copy-paste the following into the editor

```
version: '2'
services:
  mysql:
    image: mysql:5.7.23
    command: --default-authentication-plugin=mysql_native_password
    restart: on-failure
    environment:
      MYSQL_ROOT_PASSWORD: 123456
      TZ: Asia/Kolkata
    ports:
      - 3306:3306
      - 33060:33060
    volumes:
      - /<path>/<to>/docker/etc/mysql/conf.d:/etc/mysql/conf.d
      - /<path>/<to>/docker/var/lib/mysql:/var/lib/mysql
    container_name: ddb001
```

- Create a mysql configuration file

```
	nano ~/docker/etc/mysql/conf.d/my.cnf
```

- Copy-paste the following contents

```
[mysqld]
bind-address = 0.0.0.0
skip-log-bin
sql_mode = NO_ENGINE_SUBSTITUTION
# Cf. https://dba.stackexchange.com/q/5666/62861
innodb_file_per_table = ON
innodb_flush_log_at_trx_commit = 2
```

- Start the Docker Compose Service for MySQL

```
	docker-compose -f ~/docker/mysql.yml up
```

- If you wish to daemonize the service change the last command to the following

```
	docker-compose -f ~/docker/mysql.yml up -d
```
