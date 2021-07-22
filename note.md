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
![](/media/sium/2f4e3a09-7682-448e-9756-b56e2c7bfa24/virtual/ubuntu/user.png)

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


