
u can do it by using nano /etc/network/interfaces

```
# The primary network interface 

auto eth0 

allow-hotplug eth0 

iface eth0 inet dhcp
```

```
sudo service networking restart
```