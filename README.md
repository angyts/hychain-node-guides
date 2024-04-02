
# The mother of all guides on setting up a Hychain Node

Sorry for not attributing the authors here, I'm pulling out all the bits and pieces from everywhere. Thank you in advance.

---

## The general steps

1. [Buy node keys](#buy-node-keys).
2. [Decide on whether you want to use Nodeops](#using-nodeops), or
3. [You want to run your own nodes at home](#running-your-own-nodes), or
4. With a server "in a cloud" (public cloud service / virtual private cloud).
5. Get some Hychain $TOPIA.
6. Delegate your node keys.
7. Grab a cup of coffee, and profit.
8. [Oh no I got stuck somewhere](#i-got-stuck).

---

### Buy node keys

Make sure you have some ETH on Ethereum mainnet, go to the [official site](https://nodes.hychain.com/) and purchase them.

---
### Using Nodeops
TODO
---
### Running your own nodes
1. Check out the [requirements](https://docs.hychain.com/docs/what-are-node-hardware-requirements), and prepare the machine properly first
2. Setting up the machine, and choosing an operating system.
3. Hardening the machine.
4. Setup the node according to the operating system of your node. Click here for Linux servers. Click here for ARM servers (like raspberry pi). Click here for windows servers. Click here for Mac servers.
--- 
### Setup for Linux servers
1. Login to your server. Recommend to [learn SSH](https://www.geeksforgeeks.org/ssh-command-in-linux-with-examples/) and login with SSH.
2. Create a non root user. For example for the user `hychain-node-user`.

```bash
sudo useradd hychain-node-user
```

3. Set a strong password.

```bash
sudo passwd hychain-node-user
```

4. Go to the home directory of `hychain-node-user`.

```bash
cd /home/hychain-node-user
```

You can verify you are there using

```bash
pwd
```

which should output

`$ /home/hychain-node-user`


5. Download the latest release

```bash
wget https://github.com/HYCHAIN/guardian-node-software/releases/download/0.0.1/guardian-cli-linux-v0.0.1.zip
```

6. Download the software to unzip it. (Dear devs, please consider using tar instead)

```bash
sudo apt-get install unzip
```

7. Unzip the file and cleanup

```bash
unzip guardian-cli-linux-v0.0.1.zip
rm guardian-cli-linux-v0.0.1.zip
```

8. Create a new wallet

```bash
./guardian-cli-linux guardian new-wallet
```

| :exclamation:  Please save the public and private key somewhere save!!!   |
|-----------------------------------------|

9. [Delegate your node keys](#delegate-your-node-keys), using this public address.

10. Test to see if it works, insert your private key into `<private key from the new-wallet command above>`.

```bash
./guardian-cli-linux guardian run <private key from the new-wallet command above> --loop-interval-ms 3600000
```

You should see the following output or something similar if it works.



12. 




---
### Setup for Windows servers
create a folder
https://github.com/HYCHAIN/guardian-node-software/releases download windows zip and move it inside the folder you created
unzip/extract the zip wile and click on the folder until you see guardian-cli-win.exe
right click and choose open terminal
guardian-cli-win guardian new-wallet (write public and secret keys somewhere)
https://delegate.xyz/?r&chainId=2911&contract=0xE1060b30D9fF01Eef71248906Ce802801a670A48
fill in the public wallet you got in new-wallet command and connect metamask (you will need some topia on hychain and not on eth)
and after that  guardian-cli-win guardian run <private key from the new-wallet> --loop-interval-ms 3600000
---
### Setup for MacOS servers

create a folder
https://github.com/HYCHAIN/guardian-node-software/releases download macos zip and move it inside the folder you created
unzip/extract the zip wile and click on the folder until you see guardian-cli-macos.exe
right click and choose open terminal
./guardian-cli-macos guardian new-wallet (write public and secret keys somewhere)
https://delegate.xyz/?r&chainId=2911&contract=0xE1060b30D9fF01Eef71248906Ce802801a670A48
fill in the public wallet you got in new-wallet command and connect metamask (you will need some topia on hychain and not on eth)
and after that  ./guardian-cli-macos guardian run <private key from the new-wallet> --loop-interval-ms 3600000

 If you are having issues with terminal just closing and you can't do anything follow this additional step
go into your system settings and check under "Privacy & Security" if it says "App ... was blocked from use"?
click allow anyway and try running command again 
---

### Delegate your node keys
https://delegate.xyz/?r&chainId=2911&contract=0xE1060b30D9fF01Eef71248906Ce802801a670A48

---
### I got stuck
Troubleshooting steps
