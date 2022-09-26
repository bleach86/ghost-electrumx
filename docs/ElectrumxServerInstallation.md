# Electrumx Server Installation

This guide will cover the default installation process for an Electrumx server running on Ubuntu 20.04 or 22.04

## GhostVault for ghostd management.
### Difficulty easy

The recommended and simplest way to run an electrumx server is to use GhostVault for ghostd management. This is make sure that ghostd is always up to date and running.

If you do not have GhostVault 1.3+ installed and running already, you can follow the guide found [here](https://github.com/ghost-coin/GhostVault#readme).

To check the version of your GhostVault, ssh into your GhostVault and run the following command.

```
cd ~/GhostVault && python3 ghostVault.py status
```

The version should be 1.3 or higher.
If your GhostVault is not 1.3 or higher, run the following to update GhostVault

```
python3 ghostVault.py update
```

Once you confirm that you are running GhostVault version 1.3 or higher, you can run the following command to complete the electrumx installation.

```
wget https://raw.githubusercontent.com/bleach86/GhostVault/main/electrumxwithghsotvaultinstall.sh -O - | bash
```

Now to enable electrumx run the following.

```
sudo service electrumx start
```

to confirm that electrumx is running, run the following.

```
sudo service electrumx status
```

It should have Green active.

and to view the output of electrumx, run the following.

```
journalctl -u electrumx -f
```

Once electrumx syncs with ghostd, your electrum server is fully operational and ready to accept connections from Ghost Electrum clients.

You can either use the public ip, or you can set a domain to point to your server.


## Manual ghostd management.
### Difficulty medium

To begin, download the latest ghost-core for x86_64-Linux from [here](https://github.com/ghost-coin/ghost-core/releases/latest).


```
wget https://github.com/ghost-coin/ghost-core/releases/download/v0.22.0.0/ghost-0.22.0.0-x86_64-pc-linux-gnu.tar.gz
```

Now unpack it.

```
tar -xvf ghost-0.22.0.0-x86_64-pc-linux-gnu.tar.gz
```

Enter the following command to create .ghost directory and make ghost.conf file

```
mkdir .ghost && nano .ghost/ghost.conf
```

Paste the following into the nano text editor.

```
addressindex=1
server=1
txindex=1
daemon=1

rpcuser=user
rpcpassword=password
rpcallowip=127.0.0.1
rpcport=51725
rpcbind=127.0.0.1
```

Now press `ctrl + x` then type `y` and press `Enter` to save and quit.

To start ghostd start by changing into the directory.

```
cd ghost-0.22.0.0
```

Now you can run ghostd

```
./ghostd
```

You can use ghost-cli to interact with ghostd. `./ghost-cli getblockchaininfo` will give you sync status information for instance. 
`./ghost-cli help` for more information on Ghost rpc commands

no change back to your home directory by entering `cd`

Run the follwoiung to install electrumx


```
wget https://raw.githubusercontent.com/bleach86/electrumx-installer/master/bootstrap.sh -O - | bash
```

Now to enable electrumx run the following.

```
sudo service electrumx start
```

to confirm that electrumx is running, run the following.

```
sudo service electrumx status
```

It should have Green active.

and to view the output of electrumx, run the following.

```
journalctl -u electrumx -f
```

Once electrumx syncs with ghostd, your electrum server is fully operational and ready to accept connections from Ghost Electrum clients.

You can either use the public ip, or you can set a domain to point to your server.

**NOTE**

For this method, you will need to update ghostd manually.

First stop ghostd by running the following from the directory with ghost-cli in it.

```
./ghost-cli stop
```

Now change to home directory and use wget and tar to download and extract the new version. Then you can change into the new directory and run `./ghostd` to run ghostd

See the official docs for more detailed information on installation and configuration options [here](https://electrumx-spesmilo.readthedocs.io/en/latest/).