# Server

## Ubuntu
`Installation`
```bash
$ sudo apt update
$ sudo apt upgrade -y
$ sudo apt install mysql-server -y
```

`Version`
```bash
$ mysql --version
```

`Manual:service`
```bash
$ sudo service mysql start
$ sudo service mysql status
$ sudo service mysql restart
$ sudo service mysql stop
```

`Auto:systemctl`
```bash
$ sudo systemctl enable mysql
$ sudo systemctl status mysql
$ sudo systemctl restart mysql
$ sudo systemctl stop mysql
```

`Root Password Initialization`
```bash
$ sudo mysql -u root
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'NEW_PASSWORD';
mysql> FLUSH PRIVILEGES;
mysql> EXIT;
```

`Access`  
`PASSWORD`:`sudo cat /etc/mysql/debian.cnf`
```bash
$ sudo mysql
$ sudo mysql -u root
$ sudo mysql -u root -p
```

`Remove`
```bash
$ sudo systemctl stop mysql
$ sudo apt purge mysql-server mysql-client mysql-common mysql-server-core-* mysql-client-core-*
$ sudo apt autoremove -y
$ sudo apt autoclean
$ sudo rm -rf /etc/mysql /var/lib/mysql /var/log/mysql* /var/run/mysqld /usr/lib/mysql /usr/include/mysql /usr/share/mysql /var/lib/dpkg/info/mysql*
```

### Configuration

`/etc/mysql/mysql.conf.d/mysqld.cnf`
```bash
[mysqld]
socket  = /var/run/mysqld/mysqld.sock
```
`/tmp/mysql.sock`
```bash
$ sudo ln -s /var/run/mysqld/mysqld.sock /tmp/mysql.sock
```


`Data:/etc/mysql/my.cnf`
```ini
[mysqld]
datadir = /mnt/d/mysql
```
```bash
$ sudo service mysql stop
$ sudo mv /var/lib/mysql /mnt/d/mysql
$ sudo chown -R mysql:mysql /mnt/d/mysql
$ sudo chmod -R 755 /mnt/d/mysql
$ sudo service mysql start
```


### Query
```sql
SHOW DATABASES;
USE [DATABASE];
SHOW TABLES;
```


## Windows

`WIN + R`
```cmd
services.msc
```
