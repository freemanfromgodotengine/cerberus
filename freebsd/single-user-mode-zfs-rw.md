# Single user mode zfs read write

Using single user mode to fix problems like a typo in /etc/pam.d/system  
if you have a typo in /etc/pam.d/system you wont be able to use sudo to fix the problem

So you have to boot into single user mode to fix the issue

Turn the computer on 

* Enter you geli password to decrypt the drive
* At the boot screen select single user mode by pressing 2
* at the prompt enter your root password

if you run mount you wil see the file system is mounted read only

```
mount
```

So we need to mount /root as read write  
we do this by setting readonly to off

```
zfs set readonly=off zroot
```
