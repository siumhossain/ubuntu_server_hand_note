# Creating User 

Creating users in Ubuntu can be done with one of two commands: adduser and
useradd .

### Using useradd
Example

```bash
sudo useradd -d /home/sakin -m sakin
```
`-d` for created home directory for this user
`-m` flag tells the sys make a home directory to be created at this process.

> Check user list by 

```bash
ls -l /home
```
**Creating password for new User**
```bash
sudo passwd user_name
```
#### Add user another way
```bash
sudo adduser username
```
for adding lot of info
![](/mnt/2f4e3a09-7682-448e-9756-b56e2c7bfa24/git_repo/ubuntu_server_configure/user.png)

**Removing Users**

```bash
sudo userdel user_name
```
**But this will not delete user home directory.If we want to store their info we can 
make a directory and move file there**
```bash 
sudo mkdir -p /store/file_archive
sudo mv /home/username /store/file_archive
```
**Or delete parmanently**
```bash 
sudo userdel -r username
```
And remove User home directory by Using
```bash
sudo rm -r /home/username
```
# Understanding the /etc/passwd and /etc/shadow files

user account information is stored in two special text files:
* /etc/passwd
* /etc/shadow

**Command for when user change their password**
```bash
sudo passwd -S user_name
```
#### Switching User
switch 2 root
```bash
su
```
set root password
```bash
sudo passwd
```
root 2 those user who have access to root
```bash
sudo su -
```

### Managing Groups

The /etc/group file contains information regarding the groups
```bash
cat /etc/group
```
**Add group By**
```bash
sudo groupadd group_name
```
**For delete**
```bash
sudo groupdel admins
```
**Add a user in a specific group**
```bash
sudo usermod -aG group_name user_name
```
In that example, we're supplying the -a option, which means append, and
immediately following that we're using -G, which means we would like to modify
secondary group membership. I put the two options together with a single dash
( -aG ), but you could also issue them separately ( -a -G ) as well. The example I gave
only adds the user to additional groups, it doesn't replace their primary group.

---

One additional example is changing a user's /home directory. Suppose that one of
your users has undergone a name change, so you'd like to change their username,
as well as moving their previous home directory (and their files) to a new one. The
following commands will take care of that:
```bash
sudo usermod -d /home/jsmith jdoe -m
sudo usermod -l jsmith jdoe
```
---
remove a user from a group, you can use the gpasswd command to do
so. gpasswd -d will do the trick:
```bash
sudo gpasswd -d <username> <grouptoremove>
```
In fact, gpasswd can also be used in place of usermod to add a user to a group:
```bash

sudo gpasswd -a <username> <group>
```
## Managing passwords and password policies

**Locking and unlocking user account**
```bash 
sudo passwd -l username
```
> To lock an account, use the -l option

**And for unlock**
```bash
sudo passwd -u username
```
> locking an account will not prevent the user from logging in if they have
access to the server via SSH while utilizing public key authentication.

**Chek user password chabge info**
```bash
sudo chage -l username
```
**To set a user account to require a password change after a certain period of days, the following will do the trick:**
```bash
sudo chage -M 90 username
```
**set the minimum number of days with the -m flag**
```bash
sudo chage -m 5 username
```
### Setting a password policy

**Specific to the subject of setting up a password policy, we can install a PAM module
to enable this, which involves installing a new package:**
```bash
sudo apt install libpam-cracklib
```
**To enable a history requirement for your passwords (meaning the system remembers
the last several passwords a user has used, preventing them from reusing them),**
```bash
sudo nano /etc/pam.d/common-password
```
add this line
> password	required	pam_pwhistory.so
> remember=99 	use_authok


By default, members of the sudo group are able to use sudo without any restrictions.
Basically, members of this group can do anything root can do---

**To give additional users access to sudo , all you would need to do is add them to the sudo group:**

```bash
sudo usermod -aG sudo username
```
**To actually configure sudo , we use the `visudo` command. This
command assists you with editing /etc/sudoers**

give permission - edit file

username ubuntu= /usr/bin/apt

### Permision 

600 : User has read and write(4+2),group has no permission as well as world
740 : User has read,write and exicute,group has read and world has zero
770 : User has read,write and exicute,group has read,write and exicute and world has nothing
777 : Everyone has permission for everything..

**Making directory permission**
```bash
chmod 770 -R dir_name
```

## File system layout
| Directory  | Purpose |
| ------------- | ------------- |
| /  | The beginning of the file system; all directories are underneath this |
| /etc  | System-wide application configuration  |
| /home  | Home directory |
| /root  | The home directory for root (root doesn't have a directory under /home)  |
| /media  | For removable media, such as flash drives  |
| /mnt  | For volumes that are intended to stay mounted for a while  |
| /opt  | Additional software packages  |
| /bin  | Essential user binaries (ls, cp, and so on)  |
| /proc  | Virtual file system for OS-level components |
| /usr/bin  | Virtual file system for OS-level components |
| /usr/lib  | Libraries  |
| /var/log  | Log files  |

## Finding a file by name 
```bash
find / -name "syslog"
```

## Shell 
** Moving cursor on shell **

| Keyboard shortcut | Result |
| ----------------------------| ----------|
| Ctrl + a | Moves the cursor to the beginig of the line|
| Ctrl + e | Moves the cursor to the end of the line |
| Ctrl + l | Clears the screen |
| Ctrl + k | Deletes characters from the cursor to the end of the line|
| Ctrl + u | Deletes a full line |



## Viewing disk usage
```bash
df -h 
```
## Ram check
```bash
free
```
## ssh connection

```bash
ssh ip_address
```
**For different username**
```bash
ssh name@ip_address
```
ssh command by default connect in port22
For making specific port address 
```bash
ssh -p port_number name@ip_address
```

## Generating public and private keys
```bash
ssh-keygen
```

## Setting up a DHCP server for serving IP addresses

> We can start it as soon as we've finished adding our configuration:`

```bash
sudo apt install isc-dhcp-server
```
---
Now that you've installed the isc-dhcp-server package, you'll have a default
configuration file for it at /etc/dhcp/dhcpd.conf . This file will contain some default
configuration, with some example settings that are commented out. Feel free to take
a look at this file to get an idea of some of the settings you can configure. We'll create
our own dhcpd.conf file from scratch. So when you're done looking at it, copy the
existing file with a new name so we can refer to it later if we ever need to:


```bash
sudo mv /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.orig
```
Now, we're ready to create our own dhcpd.conf file. Open /etc/dhcp/dhcpd.conf in
your preferred text editor. Since the file no longer exists (we moved it), we should
start with an empty file. Here's an example dhcpd.conf file that I will explain so
that you understand how it works:

```bash 
cd /etc/dhcp/
```
```bash
sudo nano dhcpd.conf
```
`
default-lease-time 43200;
max-lease-time 86400;
option subnet-mask 255.255.255.0;
option broadcast-address 192.168.1.255;
option domain-name "local.lan";
authoritative;
subnet 192.168.1.0 netmask 255.255.255.0 {
range 192.168.1.100 192.168.1.240;
option routers 192.168.1.1;
option domain-name-servers 192.168.1.1;
}
`

```bash
cd /etc/default/
nano isc-dhcp-server
```
`change` `INTERFACESv4=""` to:
`INTERFACESv4="enp0s3"`



```bash
sudo systemct1 start isc-dhcp-server
```
Configuring an IPv6 network is beyond the scope of this chapter, but the DHCP server package we've just installed also comes with an IPv6 equivalent. Let's stop and disable this service, since we won't be using it:

Active and previous DHCP leases are stored in the /var/lib/dhcp/dhcpd.leases file,
and a typical lease entry in that file would look similar to the following:

`
lease 192.168.1.138 {
starts 0 2020/10/05 16:37:30;
ends 0 2020/10/06 16:42:30;
cltt 0 2020/20/06 16:37:30;
binding state active;
next binding state free;
rewind binding state free;
hardware ethernet 32:6e:92:01:1f:7f;
}
`


## Configure DNS server

To set up your very own DNS server, we'll ﬁrst need to install the
Berkeley Internet Name Daemon (BIND) package on our server:



```bash
sudo apt install bind9
```



We'll conﬁgure bind with actual hosts later, but se ing up a caching name server is a good way to get started.



open the file in editors 
`/etc/bind/named.conf.options`



`// forwarders {
//
0.0.0.0;
// };`



> Uncomment these lines 

```
```

```bash
forwarders {
8.8.8.8;
8.8.4.4;
};
```



After you save the ﬁle, restart the bind9 service:

```bash
sudo systemctl restart bind9
```



To be sure that everything is running smoothly, check the status of the service:

**It should report that it's `active (running) `.**



### Setting up internal DNS and adding hosts



edit this file `/etc/bind/named.conf.local`

```bash
zone "local.lan" IN {
type master;
file "/etc/bind/net.local.lan";
};
```



`create a file `

```bas
touch /etc/bind/net.local.lan
```

And those line 

```bash
$TTL 1D
@ IN SOA local.lan. hostmaster.local.lan. (
202008161; serial
8H ; refresh
4H ; retry
4W ; expire
1D ) ; minimum
IN A 192.168.1.1
;
@ IN NS hermes.local.lan.
fileserv	IN A	192.168.1.3
hermes		IN A	192.168.1.1
mailserv	IN A	192.168.1.5
mail		IN CNAME	mailserv.
web01		IN A	192.168.1.7
```

```bash
sudo systemctl restart bind9
systemctl status bind9
```



```check```

```bas
dig www.google.com
```

