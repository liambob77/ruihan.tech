# mysql opr records

## steps to solove mariadb ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)

```bash
sudo systemctl stop mariadb.service
# kill mariadb.pid
vi /etc/my.cnf
```

> under [mysqld] add following line
> skip-grant-tables

```bash
sudo systemctl start mariadb.service
mysql -u root
```

```mysql
update mysql.user set Password=password('xxx') where User='root', Host='localhost';
flush privileges;
quit
```

restore my.cnf

```bash
sudo systemctl restart mariadb.service
```

Special cases:

> check if localhost for root exists
> check plugin is empty
> check error info, the host after root

## mysql add user and enable remote connect

```mysql
create user wl identified by 'wl';
GRANT ALL privileges on *.* to wl@'%' identified by 'wl';
flush privileges;
show grants for wl;
```

### incase firewall has been enable

```bash
centos 7
sudo firewall-cmd --list-ports
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
sudo firewall-cmd --reload
sudo systemctl stop firewalld.service
sudo systemctl disable firewalld.service
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```
