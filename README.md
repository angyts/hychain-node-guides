# The mother of all guides on setting up a Hychain Node

Sorry for not attributing the authors here, I'm pulling out all the bits and pieces from everywhere. Thank you in advance. This is hot off the oven, it might have errors, please leave a pull request / issue.

---

## The general steps

1. [Buy node keys](https://nodes.hychain.com/).
2. [Decide on whether you want to use Nodeops](#using-nodeops), or
3. [You want to run your own nodes at home](#running-your-own-nodes), or
4. With a server "in a cloud" (public cloud service / virtual private cloud).
5. [Get some Hychain $TOPIA](#get-topia).
6. [Delegate your node keys](#delegate-your-node-keys).
7. [Grab a cup of coffee, and profit](#how-to-claim-rewards).
8. [Oh no I got stuck somewhere](#i-got-stuck).

---
### Using Nodeops TODO
--- 

### Running your own nodes
1. Check out the [requirements](https://docs.hychain.com/docs/what-are-node-hardware-requirements), and prepare the machine properly first.
2. Setting up the machine, and choosing an operating system.
3. Hardening the machine. (TODO a guide on this)
4. Setup the node according to the operating system of your node. [Click here for Linux servers](#setup-for-linux-servers). Click here for ARM servers (like raspberry pi). [Click here for windows servers](#setup-for-Windows-servers). Click here for Mac servers.

--- 

### Setup for Linux servers

1. Login to your server. Recommend to [learn SSH](https://www.geeksforgeeks.org/ssh-command-in-linux-with-examples/) and login with SSH.
2. Create a non root user. For example for the user `hychain-node-user`.

```bash
sudo adduser --system --group hychain-node-user
```

3. Download the latest release

```bash
sudo wget https://github.com/HYCHAIN/guardian-node-software/releases/download/0.0.1/guardian-cli-linux-v0.0.1.zip -O /home/hychain-node-user/node.zip
```

4. Download the software to unzip it. (Dear devs, please consider using tar instead)

```bash
sudo apt-get update
sudo apt-get install unzip
```

5. Unzip the file and cleanup

```bash
sudo unzip -o /home/hychain-node-user/node.zip -d /home/hychain-node-user/
sudo rm /home/hychain-node-user/node.zip
```

6. Create a new wallet

```bash
sudo /home/hychain-node-user/guardian-cli-linux guardian new-wallet
```

| :exclamation:  Please save the public and private key somewhere safe!!!   |
|-----------------------------------------|

7. [Delegate your node keys](#delegate-your-node-keys), using this public address.

8. Test to see if it works, insert your private key into `<private key from the new-wallet command above>`.

```bash
sudo /home/hychain-node-user/guardian-cli-linux guardian run <private key from the new-wallet command above> --loop-interval-ms 3600000
```

You should see the following output or something similar if it works.

`$Apr 02 21:08:52 hyserver bash[5649]: [21:08:52.042] INFO: Running guardian for owner (0x22134145124151511511455145144125) with 99999999999 delegated node keys`<br>
`$Apr 02 21:08:53 hyserver bash[5649]: [21:08:53.502] INFO: Processing challenges for 99999999999 node keys`<br>
`$Apr 02 21:10:19 hyserver bash[5649]: [21:10:19.172] INFO: Sleeping for 3600000ms before running guardian again`<br>

At that point you can press `ctrl-c` to stop the command. If you didn't get this, you need to go through the steps and see where you missed out.

9. Use systemctl to create a long standing service.

```bash
sudo nano /etc/systemd/system/hychain-node.service
```

| 📓:   Note: If you meet an error, maybe your linux doesn't have `nano`, you can install it with `sudo apt-get install nano`  |
|-----------------------------------------|

Paste this inside. Remember to paste your real `private key` into the `<private key from the new-wallet command above>`.

```nano
[Unit]
Description=Hychain Node
Documentation=https://github.com/angyts/hychain-node-guides/blob/main/README.md#setup-for-linux-servers

[Service]
Type=simple
User=hychain-node-user
Group=hychain-node-user
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

1. Create a folder to keep the file. Maybe like in the documents.
2. Download [this file](https://github.com/HYCHAIN/guardian-node-software/releases/download/0.0.1/guardian-cli-windows-v0.0.1.zip). https://github.com/HYCHAIN/guardian-node-software/releases/download/0.0.1/guardian-cli-windows-v0.0.1.zip
3. Extract this file and move it inside the folder that you just created.
4. Look for this file `guardian-cli-win.exe`. Right click on it, and click `properties`. Look for this thing called `location`. It might look something like `C:\Users\Documents\hychainnode`. Copy this `location`.
5. Open the command prompt with the hotkey `Windows-R` or press `start` search for `run` to open this run window, then type `cmd` inside.

<img src="https://github.com/angyts/hychain-node-guides/assets/23327200/2c1c4be0-b42a-407f-bd6a-1db7e4563f25" height="200">

6. Go to the directory which contains your file, using `cd` then the `location` from above.

```cmd
cd <insert the location you got from above>
```

7. Create a new wallet

```cmd
guardian-cli-win guardian new-wallet
```

| :exclamation:  Please save the public and private key somewhere safe!!!   |
|-----------------------------------------|
 
8. [Delegate your node keys](#delegate-your-node-keys), using this public address.

9. Run the node once to see if it works. Insert your `private key` into `<private key from the new-wallet>`

```cmd
guardian-cli-win.exe guardian run <private key from the new-wallet> --loop-interval-ms 3600000
```

You should see the following output or something similar if it works.

`$Apr 02 21:08:52 hyserver bash[5649]: [21:08:52.042] INFO: Running guardian for owner (0x22134145124151511511455145144125) with 99999999999 delegated node keys`<br>
`$Apr 02 21:08:53 hyserver bash[5649]: [21:08:53.502] INFO: Processing challenges for 99999999999 node keys`<br>
`$Apr 02 21:10:19 hyserver bash[5649]: [21:10:19.172] INFO: Sleeping for 3600000ms before running guardian again`<br>

At that point you can press `ctrl-c` to stop the command. If you didn't get this, you need to go through the steps and see where you missed out.

10. Use `Task Scheduler` to make a long running process.
 1. Open Start
 2. Type `Task Scheduler` and open.
 3. Right click `Task Scheduler Library` and click `New Folder`, maybe name it `Hychain Node`. Select the new folder you created.
 4. Click Action > Create Basic Task
 5. Put the following settings. Before pasting it, replace the `<your folder location above>` with your `location` above. Watch out for the slashes, and make sure that the whole thing is a valid file location. Also replace the `<private key from the new-wallet>` with your `private key`. Then click finish.

| Name           | Hychain Node Launcher                                                                                                      |
|----------------|----------------------------------------------------------------------------------------------------------------------------|
| Trigger        | When I log on                                                                                                              |
| Action         | Start a Program                                                                                                            |
| Program/Script | C:\Windows\System32\cmd.exe                                                                                                |
| Add arguments  | /c <your folder location above>\guardian-cli-win.exe guardian run <private key from the new-wallet> --loop-interval-ms 3600000 |
     

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
6. Then bridge your $WRLD into $TOPIA on Hychain. Wait about 15 to 30mins.
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

#### Check rewards

This is for linux, check the node key numbers from [here](https://etherscan.io/token/0xb40cdd7599d8f52c48f29e10cfbf24918c85f7cc#balances). 
Then type each key number followed by comma.

```bash
./guardian-cli-linux guardian reward-to-claim <all the key ID followed by comma>
```
<img width="458" alt="Screenshot 2024-04-03 at 7 47 00 AM" src="https://github.com/angyts/hychain-node-guides/assets/23327200/3980d073-a011-4f93-87f2-3681ee96a832">

Don't get too excited, divide that by 18 zeros.

#### Claim rewards

Right now I think I lost more $TOPIA trying to claim, so perhaps don't do it yet? Until it accumulates more?

0.9 $TOPIA in gas spent
<img width="1115" alt="Screenshot 2024-04-03 at 7 48 46 AM" src="https://github.com/angyts/hychain-node-guides/assets/23327200/19ecd303-04e0-4f9a-8bb2-228d8eb31866">

🤡 0.000142450142450141 $TOPIA reward claimed...
<img width="1241" alt="Screenshot 2024-04-03 at 7 49 12 AM" src="https://github.com/angyts/hychain-node-guides/assets/23327200/323ef0da-4b53-4969-bf02-988c0b7f77df">

But if you want to do it, here it is, replace the `<Insert your private key here>` with your `private key`, the rewards will go to the wallet that is holding your node keys.

```bash
./guardian-cli-linux guardian claim-rewards <Insert your private key here> --owned-keys --approved-keys
```

---
### I got stuck
Troubleshooting steps

Unable to estimate gas, maybe not enough $TOPIA on the node wallet:

```bash
Apr 03 00:09:25 hyserver bash[6543]: [00:09:25.411] ERROR: Error submitting assertions for challenge (623) Error: cannot estimate gas; transaction may fail or may require manual gas limit [ See: https://links.ethers.org/v5-errors-UNPREDICTABLE_GAS_LIMIT ] (error={"reason":"execution reverted","code":"UNPREDICTABLE_GAS_LIMIT","method":"estimateGas","transaction":{"from":"0x5E57f4A43626DecC87B146436b14BCDc4243949C","maxPriorityFeePerGas":{"type":"BigNumber","hex":"0x59682f00"},"maxFeePerGas":{"type":"BigNumber","hex":"0x03a3abfc6f00"},"to":"0x6c065572f1824171186aF6dF848313784d6E5b0E","data":"0x19d6c415000000000000000000000000000000000000000000000000000000000000026f6e739bb68b3471cbefa1c0b0ef9e657e48242717d25c47c2ae2f49987c43c2a0000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000018c5000000000000000000000000000000000000000000000000000000000000274b00000000000000000000000000000000000000000000000000000000000000027366772bf0e81a034697b4b674169eed8c44a67f84dd5b10743318dabebe3b512339dd41d1f53f218732af59f8f944ffaaed92452795c48ca1200d8b0188a7d0","type":2,"accessList":null},"error":{"reason":"processing response error","code":"SERVER_ERROR","body":"{\"jsonrpc\":\"2.0\",\"id\":468,\"error\":{\"code\":-32000,\"message\":\"execution reverted\"}}\n","error":{"code":-32000},"requestBody":"{\"method\":\"eth_estimateGas\",\"params\"[{\"type\":\"0x2\",\"maxFeePerGas\":\"0x3a3abfc6f00\",\"maxPriorityFeePerGas\":\"0x59682f00\",\"from\":\"0x5e57f4a43626decc87b146436b14bcdc4243949c\",\"to\":\"0x6c065572f1824171186af6df848313784d6e5b0e\",\"data\":\"0x19d6c415000000000000000000000000000000000000000000000000000000000000026f6e739bb68b3471cbefa1c0b0ef9e657e48242717d25c47c2ae2f49987c43c2a0000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000018c5000000000000000000000000000000000000000000000000000000000000274b00000000000000000000000000000000000000000000000000000000000000027366772bf0e81a034697b4b674169eed8c44a67f84dd5b10743318dabebe3b512339dd41d1f53f218732af59f8f944ffaaed92452795c48ca1200d8b0188a7d0\"}],\"id\":468,\"jsonrpc\":\"2.0\"}","requestMethod":"POST","url":"http://hychain.calderachain.xyz/guardian-http"}}, tx={"data":"0x19d6c415000000000000000000000000000000000000000000000000000000000000026f6e739bb68b3471cbefa1c0b0ef9e657e48242717d25c47c2ae2f49987c43c2a0000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000018c5000000000000000000000000000000000000000000000000000000000000274b00000000000000000000000000000000000000000000000000000000000000027366772bf0e81a034697b4b674169eed8c44a67f84dd5b10743318dabebe3b512339dd41d1f53f218732af59f8f944ffaaed92452795c48ca1200d8b0188a7d0","to"{},"nonce":1,"from":"0x5E57f4A43626DecC87B146436b14BCDc4243949C","type":2,"maxFeePerGas":{"type":"BigNumber","hex":"0x03a3abfc6f00"},"maxPriorityFeePerGas":{"type":"BigNumber","hex":"0x59682f00"},"gasLimit":{},"chainId":{}}, code=UNPREDICTABLE_GAS_LIMIT, version=abstract-signer/5.7.0)
```

Wierd error:
![image](https://github.com/angyts/hychain-node-guides/assets/23327200/66a908b8-be6e-48c1-904c-4be782cba3c6)
Seems to be ubuntu version issues


#### [Top](#The-mother-of-all-guides-on-setting-up-a-Hychain-Node)
