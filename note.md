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

