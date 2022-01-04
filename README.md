# Check mysql container:
```
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
26137efa979f        mysql               "docker-entrypoint..."   8 months ago        Up 10 minutes       0.0.0.0:3306->3306/tcp   db-container
```
# Check docker container mysql version:
```
$ sudo docker exec db-container mysqld --version`
mysqld  Ver 5.7.15 for Linux on x86_64 (MySQL Community Server (GPL))
```

# Check current mysqld.cnf:
`$ sudo docker exec db-container cat /etc/mysql/mysql.conf.d/mysqld.cnf`

# Copy mysqld.cnf to local:
`$ sudo docker cp db-container:/etc/mysql/mysql.conf.d/mysqld.cnf ~/mysqld.cnf`

# Edit config:

```
$ sudo nano ~/mysqld.cnf`

[mysqld]
skip-host-cache
skip-name-resolve
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
datadir		= /var/lib/mysql
#log-error	= /var/log/mysql/error.log
# By default we only accept connections from localhost
#bind-address	= 127.0.0.1
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

# Slow query settings:
slow_query_log=1
slow_query_log_file=/var/log/mysql/slow.log
long_query_time=0.1

character-set-server=utf8mb4

[client]
default-character-set=utf8mb4
```

# Copy new mysqld.cnf to the container:
`$ sudo docker cp ~/mysqld.cnf db-container:/etc/mysql/mysql.conf.d/mysqld.cnf`

# Restart mysql container:
`$ sudo docker restart container-mysql`

# Check slow query log:
`$ docker exec db-container tail -f /var/log/mysql/slow.log
`

