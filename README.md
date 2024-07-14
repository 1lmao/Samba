# Samba
Samba server set up so it works with both linux and windows computers

# Setting Up Samba Server on Ubuntu

## Installing Samba

To install Samba, run the following commands:

```bash
sudo apt update
sudo apt install samba
```

To verify the installation, you can check the location of the Samba binaries:

```bash
whereis samba
```

Expected output should include paths like `/usr/sbin/samba`, `/etc/samba, etc`.

## Setting up Samba

### Creating a Share

Now that Samba is installed, create a directory for sharing:

```bash
mkdir /home/<username>/sambashare/
```

Edit the Samba configuration file to add the new directory as a share:

```bash
sudo vim /etc/samba/smb.conf

or

sudo nano /etc/samba/smb.conf
```


Add the following lines at the bottom of the file:

```bash

[sambashare]
    comment = Samba on Ubuntu
    path = /home/<username>/sambashare
    read only = no
    browsable = yes

```
If using `vim`
Save and exit : `ESC` -> `:wq!`

If using `nano`
Save the file `(Ctrl-O, Enter)` and exit `(Ctrl-X)` nano.

### Restarting Samba

Restart the Samba service for the changes to take effect:

```bash
sudo service smbd restart

```

### Firewall Configuration

Update the firewall rules to allow Samba traffic:

```bash
sudo ufw allow samba

```

### Setting up User Accounts
Since Samba uses its own user authentication, set a Samba password for your user account:

```bash
sudo smbpasswd -a <username>

```
Note: The username must correspond to a system account; otherwise, it will not be saved. For example, the path `/home/<username>/sambashare` should reflect an existing system user.

## Connecting to Share
### On Ubuntu

Open the file manager, click Connect to Server, and enter:

```bash
smb://<ip-address>/sambashare
```
Replace `<ip-address>` with the IP address of your Samba server and sambashare with the name of your share.


### On macOS

In Finder, go to Go > Connect to Server and enter:

```bash
smb://<ip-address>/sambashare
```
Replace `<ip-address>` with the IP address of your Samba server and sambashare with the name of your share.

### On Windows

Open File Explorer and enter the following in the address bar:

```bash
\\<ip-address>\sambashare
```

Replace `<ip-address>` with the IP address of your Samba server and sambashare with the name of your share.

When tryig to connect we kept getting the followiung error :


To solve this we had to go on Windows Registry and make some small changes. Please execute the following instructions:

```bash
Window + r : regedit 

go to the following location:
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\

Add new DWORD:
Key: LmCompatibilityLevel
Value: 4
```

# References
https://ubuntu.com/tutorials/install-and-configure-samba#1-overview
<br>
https://serverfault.com/questions/744312/access-denied-to-samba-share-from-windows-10


