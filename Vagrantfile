# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.provision "shell", inline: <<-SHELL
    # packages
    wget http://repo.zabbix.com/zabbix/3.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.4-1+xenial_all.deb
    sudo dpkg -i zabbix-release_3.4-1+xenial_all.deb
    sudo apt-get update
    sudo apt-get install -y zabbix-server-pgsql zabbix-agent zabbix-frontend-php php7.0-pgsql

    # configure PostgreSQL
    sudo -u postgres createuser zabbix
    sudo -u postgres psql -c "ALTER USER zabbix WITH PASSWORD 'zabbix';"
    sudo -u postgres createdb -O zabbix zabbix_server
    zcat /usr/share/doc/zabbix-server-pgsql/create.sql.gz | sudo -u zabbix psql zabbix_server

    # configure Zabbix
    sudo sed -i "s|^DBName=zabbix|DBName=zabbix_server|; /^# DBPassword=/a \\\\nDBPassword=zabbix" /etc/zabbix/zabbix_server.conf
    sudo sed -i "s|# php_value date.timezone Europe/Riga|php_value date.timezone Europe/Warsaw|" /etc/apache2/conf-enabled/zabbix.conf
    sudo cat > /usr/share/zabbix/conf/zabbix.conf.php << "EOF"
<?php
// Zabbix GUI configuration file.
global $DB;

$DB['TYPE']     = 'POSTGRESQL';
$DB['SERVER']   = 'localhost';
$DB['PORT']     = '0';
$DB['DATABASE'] = 'zabbix_server';
$DB['USER']     = 'zabbix';
$DB['PASSWORD'] = 'zabbix';

// Schema name. Used for IBM DB2 and PostgreSQL.
$DB['SCHEMA'] = '';

$ZBX_SERVER      = 'localhost';
$ZBX_SERVER_PORT = '10051';
$ZBX_SERVER_NAME = '';

$IMAGE_FORMAT_DEFAULT = IMAGE_FORMAT_PNG;
EOF

    # enable and start services
    sudo update-rc.d zabbix-server enable
    sudo service zabbix-server start

    sudo update-rc.d zabbix-agent enable
    sudo service zabbix-agent start

    sudo service apache2 restart
  SHELL
end
