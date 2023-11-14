
---------Zabbix Paketi İndirme----------

```
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb

dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb

apt update
```

---------Postgre Yükleme--------
```
sudo apt update

sudo apt install postgresql postgresql-contrib

sudo systemctl start postgresql.service

sudo -i -u postgres

psql

\l 
```

-------Install Zabbix server, frontend, agent---------
```
apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent

chmod 777 /home/satu (Kullanıcı dizinine Full erişim yetkisi vermek gerekiyor. "satu" yazan yere, sizin kurulumda verdiğiniz user'ı vermeniz  gerekiyor. )
```

-----------Create initial database----------------
```
sudo -u postgres createuser --pwprompt zabbix

sudo -u postgres createdb -O zabbix zabbix

zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

nano /etc/zabbix/zabbix_server.conf (Zabbix konfigürasyon dosyasında düzenleme yapmak için nano editörü ile erişiyoruz)

DBPassword=password (Burada, database kullanıcısı oluştururken verdiğimiz şifreyi giriyoruz.)

systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

-----TimescaleDB Extension Kurulumu-------
```
apt install gnupg postgresql-common apt-transport-https lsb-release wget

/usr/share/postgresql-common/pgdg/apt.postgresql.org.sh

https://packagecloud.io/timescale/timescaledb/packages/ubuntu/jammy/timescaledb-2-postgresql-14_2.11.2~ubuntu22.04_arm64.deb?distro_version_id=237 (Bu linke gidilecek, Sunucuya yapıştırılmayacak)

curl -s https://packagecloud.io/install/repositories/timescale/timescaledb/script.deb.sh | sudo bash

apt-get update

sudo apt-get install timescaledb-2-postgresql-14=2.11.2~ubuntu22.04
```
------------TimescaleDb Tune---------------
```
systemctl status zabbix-server
systemctl stop zabbix-server

sudo timescaledb-tune

apt-get update

apt-get install postgresql-client

systemctl restart postgresql

sudo -u zabbix psql

\c zabbix

CREATE EXTENSION IF NOT EXISTS timescaledb;

cat /usr/share/zabbix-sql-scripts/postgresql/timescaledb.sql | sudo -u zabbix psql zabbix

systemctl start zabbix-server
```

Zabbix Default User: Admin
zabbix Default PW: zabbix




