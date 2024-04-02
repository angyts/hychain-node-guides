# The mother of all guides on setting up a Hychain Node

Sorry for not attributing the authors here, I'm pulling out all the bits and pieces from everywhere. Thank you in advance.

---

## The general steps

1. [Buy node keys](#buy-node-keys).
2. [Decide on whether you want to use Nodeops](#using-nodeops), or
3. [You want to run your own nodes at home](#running-your-own-nodes), or
4. With a server "in a cloud" (public cloud service / virtual private cloud).
5. [Get some Hychain $TOPIA](#get-topia).
6. [Delegate your node keys](#delegate-your-node-keys).
7. [Grab a cup of coffee, and profit](#how-to-claim-rewards).
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
4. Setup the node according to the operating system of your node. [Click here for Linux servers](#setup-for-linux-servers). Click here for ARM servers (like raspberry pi). Click here for windows servers. Click here for Mac servers.
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

which should output something like

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

| :exclamation:  Please save the public and private key somewhere safe!!!   |
|-----------------------------------------|

9. [Delegate your node keys](#delegate-your-node-keys), using this public address.

10. Test to see if it works, insert your private key into `<private key from the new-wallet command above>`.

```bash
./guardian-cli-linux guardian run <private key from the new-wallet command above> --loop-interval-ms 3600000
```

You should see the following output or something similar if it works.

`$Apr 02 21:08:52 hyserver bash[5649]: [21:08:52.042] INFO: Running guardian for owner (0x22134145124151511511455145144125) with 99999999999 delegated node keys`<br>
`$Apr 02 21:08:53 hyserver bash[5649]: [21:08:53.502] INFO: Processing challenges for 99999999999 node keys`<br>
`$Apr 02 21:10:19 hyserver bash[5649]: [21:10:19.172] INFO: Sleeping for 3600000ms before running guardian again`<br>

At that point you can press `ctrl-c` to stop the command. If you didn't get this, you need to go through the steps and see where you missed out.

11. Use systemctl to create a long standing service.

```bash
sudo nano /etc/systemd/system/hychain-node.service
```

| 📓:   Note: If you meet an error, maybe your linux doesn't have `nano`, you can install it with `sudo apt-get install nano`  |
|-----------------------------------------|

Paste this inside. Remember to paste your real `private key` into the `<private key from the new-wallet command above>`.

```nano
[Unit]
Description=Hychain Node

[Service]
WorkingDirectory=/home/hychain-node-user/
ExecStart=/bin/bash -c './guardian-cli-linux guardian run <private key from the new-wallet command above> --loop-interval-ms 3600000'
Restart=always
RestartSec=3

[Install]
WantedBy=default.target
RequiredBy=network.target
```

Then click `ctrl-x` to save changes.

Start the service with

```bash
sudo systemctl daemon-reload
sudo systemctl start hychain-node.service
```

You can check on the status of the service with

```bash
sudo systemctl status hychain-node.service
```

or this command to view the logs.

```bash
sudo journalctl -fu hychain-node | ccze
```

If you want to stop it, use

```bash
sudo systemctl stop hychain-node.service
```

Now it will run even when your server restarts. But be careful of sharing your log files, because your private key might be there.

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

### Get TOPIA
1. Have some $MATIC on Polygon Mainnet (Chain ID:137). Go to any decentralised exchange. Let's use uniswap as an example. https://app.uniswap.org/. Don't google, learn to type it directly in the browser.
2. Look for the NFT WORLD $WRLD token contract address. `0xD5d86FC8d5C0Ea1aC1Ac5Dfab6E529c9967a45E9` Convince yourself and make sure it really is the correct token.
3. Key `0xD5d86FC8d5C0Ea1aC1Ac5Dfab6E529c9967a45E9` into the `Search name or paste address`

<img width="445" alt="Screenshot 2024-04-02 at 10 03 10 PM" src="https://github.com/angyts/hychain-node-guides/assets/23327200/d2697cbf-3872-49b4-8061-ebd03476fbd4">

4. Swap some $MATIC into $WRLD
5. Go to the [bridge](https://bridge.hytopia.com/bridge). https://bridge.hytopia.com/bridge
6. Then bridge your $WRLD into $TOPIA on Hychain.
7. While you are at it, add Hychain mainnet to your metamask by clicking the button below.

<img width="609" alt="Screenshot 2024-04-02 at 10 04 49 PM" src="https://github.com/angyts/hychain-node-guides/assets/23327200/9e3c0de1-846c-4c9a-b80f-eb8baba69c9a">

---

### Delegate your node keys
1) Firstly you need some $TOPIA on the Hychain, [see above](#get-topia).
2) Go to [delegate.xyx](https://delegate.xyz/?r&chainId=2911&contract=0xE1060b30D9fF01Eef71248906Ce802801a670A48). https://delegate.xyz/?r&chainId=2911&contract=0xE1060b30D9fF01Eef71248906Ce802801a670A48
3) Connect with the wallet that your bought your `node keys` on.
4) Confirm that you are on Hychain and the contract is `0xE1060b30D9fF01Eef71248906Ce802801a670A48`.
5) Key in your `public address` from above in the box and commit the transaction on chain. I think it uses about 0.4 $TOPIA.

<img width="584" alt="Screenshot 2024-04-02 at 10 07 09 PM" src="https://github.com/angyts/hychain-node-guides/assets/23327200/64b011da-d976-450a-8b24-61805c1e1b61">

---

### How to claim rewards

TODO, long running service to claim on a regular basis

./guardian-cli-linux guardian reward-to-claim 1,2,3,4,5
./guardian-cli-linux guardian claim-multiple-rewards 1,2,3,4 <signer-private-key>

---
### I got stuck
Troubleshooting steps

<button type="button" style="color: white;" ahref="#">Top</button>.

<button class="button-save large">Big Fat Button</button>

[![Button Shield]][asdasda]

</div>

<br>
<br>


<!---------------------------------------------------------------------------->

[Button Shield]: https://img.shields.io/badge/Shield_Buttons-37a779?style=for-the-badge

[License]: LICENSE
[Shield]: Types/Shield.md
[KBD]: Types/KBD.md
[#]: #

