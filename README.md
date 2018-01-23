# Vagrant for Zabbix

This Vagrant configuration can be used for quick testing purposes of Zabbix infrastructure. The system is based on **Ubuntu 16.04** (Xenial Xerus 64-bit).

The installed software is:

|component|version|
|---|---|
|Zabbix server|**3.4**|
|Zabbix agent|**3.4**|
|PostgreSQL|**9.5.x** (default for Ubuntu 16.04)|

# Requirements

* [Vagrant](https://www.vagrantup.com/)
* [VirtualBox](https://www.virtualbox.org/)

# Usage

Just use Vagrant to start Zabbix server. From inside directory where you cloned this repository run the following command:
```
vagrant up
```

After machine finished provisioning you will be able access web interface using **[http://localhost:8080/](http://localhost:8080)** with default credentials:
  * login: **Admin**
  * password: **zabbix**

### Change ports

If you need to change ports on your host just edit Vagrantfile and replace *forwarded_port* setting.
