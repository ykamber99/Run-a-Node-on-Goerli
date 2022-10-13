## 📝 Intro:
Here is a breif summary of how to configure an execution & beacon node & validator on Goerli Testnet. This repo also serves as an conclusion for the Fintech-565 Advanced Blockchain midterm and hope to provide some help for friends that are not an expert in Linux system. Thanks for my friends Lucy, Wanting, Ruofan,ccc, Tieyan🧙🏻‍♀️ and "bosses" 👨🏻‍⚖️ from Foresight News who continuously help me during the "hopeless" configuration.
There are mainly 3 different methods:
1. Use Prysm Documentation
(https://docs.prylabs.network/docs/install/install-with-script#step-3-run-an-execution-client)
2. Use eth-docker
(https://eth-docker.net/docs/About/Overview)
3. Use docker & docker-compose direcly via a yml file

## 🔎 Overall Configuration:
Operating System 👉 Linux(ubuntu 22.04 x64).   
Network 👉 Goerli-Prater.  
Execution client 👉 Geth.  
EN-BN connection 👉 HTTP-JWT. 				

## Step 0: Register an ubuntu cloud environment && Get 32 test ETH
### ✔️ Cloud Environment
Choose a cloud environment as you wish. I registed on the digital ocean since it provided $200 free credit for new users with credit card connected. In principle, we'll need 16 GB Memory, 1T Disk, and 4 cores CPU to run a full node on the chain. I just choosed 8 GB Memory, 4 intel vCPUs and 160 GB Disk in convenience.
### ✔️ Test ETH
Becoming a validator on the chain requires a 32 ETH deposition, test ETH could be get from faucet, mining and discord channels.

## Method 1: Use Prysm Documentation
### Step1: Install Prysm
Create a folder called `ethereum` on r SSD, and then two subfolders within it: `consensus` and `execution`:  
📂ethereum		
┣ 📂consensus		
&& 📂execution		

```
mkdir ethereum
cd ethereum
mkdir consensus
mkdir execution
```
Navigate to `consensus` directory, download the Prysm client and make it executable:
```
mkdir prysm && cd prysm
curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh
```
### Step2: Generate JWT Secret
Navigate to the folder where `prysm.sh` is, mine is in the `~ethereum/consensus/prysm`.

```
cd ../prysm
./prysm.sh beacon-chain generate-auth-secret
```

Prysm will create a `jwt.hex` file automatically, check the file path of `jwt.hex`

### Step3: Run an execution client
#### ✔️Geth Installation (Method from Lucy)
Geth could both be installed from download page via `curl` & `tar` and using ppa.

```
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

#### ✔️Geth Execution
Then navigate to `execution` directory and start the execution node:
❗️Parameter after `--authrpc.jwtsecret` should be the file path of `jwt.hex` 
❗️`nohup` making geth running in the background and export all the logs in to `nohup.out`

```
nohup geth --goerli --http --http.api eth,net,engine,admin --authrpc.jwtsecret ~/ethereum/consensus/prysm/jwt.hex 
```

Waiting for geth syncing......😶‍🌫️

### Step4: Runnig a beacon node via Prysm and add Checkpoint
Navigate to the folder where `prysm.sh` is

```
./prysm.sh beacon-chain --checkpoint-sync-url=https://goerli.checkpoint-sync.ethpandaops.io --genesis-beacon-api-url=https://goerli.checkpoint-sync.ethpandaops.io --prater --jwt-secret=~/ethereum/consensus/prysm/jwt.hex --genesis-state=genesis.ssz --suggested-fee-recipient=0x30Ff44534b1f564097Afc4CE775B2B2f71d0a0b6
```

So far, a full && merge-ready node is alright.

### Step5: Running a validator via Prysm
#### ✔️Download the deposit CLI from the Release Page

```
curl -LO https://github.com/ethereum/staking-deposit-cli/releases/download/v2.3.0/staking_deposit-cli-76ed782-linux-amd64.tar.gz
tar -xvf staking_deposit-cli-76ed782-linux-amd64.tar.gz
rm staking_deposit-cli-76ed782-linux-amd64.tar.gz
cd staking_deposit-cli-76ed782-linux-amd64
```

In the folder will be a file named deposit which could create mnemonic phrase and keys
#### ✔️Generate the validator keys and mnemonic seed phrase

```
./deposit new-mnemonic --num_validators=1 --mnemonic_language=english --chain=prater
```

❗️Then the CLI will generate a folder named `validator_keys` with `deposit_data-*.json` and `deposit_data-*.json` in it, as well as a **❗️new mnemonic seed phrase❗️**
Copy the `validator_keys` folder to the `consensus` folder:

```
sudo cp -r validator_keys ~ethereum/consensus
```

Then import keystores
❗️parameter of `--keys-dir` should be the full path of `validator_keys` folder
```
./prysm.sh validator accounts import --keys-dir=~/ethereum/consensus/validator_keys --prater
```
❗️❗️A prysm wallet will be generated and here required the **wallet directory to be the path to `consensus` folder** mine is ~ethereum/consensus
`Successfully imported 1 accounts, view all of them by running accounts list` means account has been successfully imported via Prysm

### Step6: Deposit and become an real validator🧙🏻‍♀️








