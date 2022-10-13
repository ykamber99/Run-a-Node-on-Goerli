📝 Intro:
Here is a breif summary of how to configure an execution & beacon node & validator on Goerli Testnet. This repo also serves as an conclusion for the Fintech-565 Advanced Blockchain midterm and hope to provide some help for friends that are not an expert in Linux system. Thanks for my friends Lucy, Wanting, ccc, Tieyan🧙🏻‍♀️ and "bosses" 👨🏻‍⚖️ from Foresight News who continuously help me during the "hopeless" configuration.
There are mainly 3 different methods:
1. Use Prysm Documentation
https://docs.prylabs.network/docs/install/install-with-script#step-3-run-an-execution-client
2. Use eth-docker
https://eth-docker.net/docs/About/Overview
3. Use docker & docker-compose direcly via a yml file

🔎 Overall Configuration:
Operating System 👉 Linux(ubuntu 22.04 x64)
Network 👉 Goerli-Prater
Execution client 👉 Geth
EN-BN connection 👉 HTTP-JWT

Step 0: Register an ubuntu cloud environment && Get 32 test ETH
✔️ Cloud Environment
Choose a cloud environment as you wish. I registed on the digital ocean since it provided $200 free credit for new users with credit card connected. In principle, we'll need 16 GB Memory, 1T Disk, and 4 cores CPU to run a full node on the chain. I just choosed 8 GB Memory, 4 intel vCPUs and 160 GB Disk in convenience.
✔️ Test ETH
Becoming a validator on the chain requires a 32 ETH deposition, test ETH could be get from faucet, mining and discord channels.

Method 1: Use Prysm Documentation
Step1: Install Prysm
Create a folder called ethereum on your SSD, and then two subfolders within it: consensus and execution:
📂ethereum
┣ 📂consensus
┣ 📂execution

```
mkdir ethereum
cd ethereum
mkdir consensus
mkdir execution
```



