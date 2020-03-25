# iocage-plugin-nextcloud
Artifact file(s) for NextCloud iocage plugin

## *Update
This 12.1-RELEASE branch is to update the NextCloud iocage plugin to the latest 
release of FreeBSD and to use the latest versions of NextCloud 18, PHP 7.4, 
MaraiDB 10.4 and Nginx 1.16 packages available from the FreeBSD repository.

#### *Plugin includes:*
* FreeBSD 12.1-RELEASE
* nextcloud-php74
* mariadb104-server
* nginx


### Instructions

* Since this uses a VNET type network jail, please make sure you have created a 
bridge and added the interface with your network connection to the bridge. The
jails epair interface will also be added to this bridge later in order to give
the jail a connection to the network.  ie. if re0 is your network interface:
```
ifconfig bridge create
ifconfig bride0 addm re0 up
```

* Download nextcloud.json to the host machine. This plugin is not in the official 
plugin repository so we must download this file manually and reference it locally in
the next command.

```
fetch https://github.com/Rob4226/iocage-plugin-nextcloud/blob/12.1-RELEASE/nextcloud.json
```

* Run the following command assuming the previously mentioned nextcloud.json is in the
current directory. Pick an unused ip address in your networks subnet to access NextCloud
and enter your networks gateway ip address in the 'defaultrouter' property.

```
iocage fetch -P nextcloud.json -n nextcloud ip4_addr="vnet0|192.168.1.45/24" interfaces=vnet0:bridge0 vnet=1 boot=1 allow_raw_sockets=1 defaultrouter="192.168.1.1"
```

That should be it. The initial passwords for NextCloud and the database will print
out to the console and also be in a file inside the jail: /root/PLUGIN
This is just a minimal install so make sure you properly secure your NextCloud
installation.

Note: I am using this with a regular install of FreeBSD, not FreeNAS, but I would
imagine it will still work because I am leaving all the files from the 11.2-RELEASE
version of this plugin.
