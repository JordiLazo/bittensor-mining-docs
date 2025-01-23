# **Bittensor Mining Guide (Kaito)**

This guide outlines the steps required to set up and run a Bittensor miner in the [Kaito](https://github.com/OpenKaito/openkaito) subnet.

---

## **Table of Contents**
1. [Introduction](#introduction)
2. [System Requirements](#system-requirements)
3. [Installation](#installation)
    1. [Get started](#get-started-in-bittensor)
4. [Configuration](#configuration)
5. [Starting the Miner](#starting-the-miner)
6. [Monitoring and Logs](#monitoring-and-logs)
7. [Tips for Optimization](#tips-for-optimization)
8. [Troubleshooting](#troubleshooting)
9. [Resources and Support](#resources-and-support)

---

## **Introduction**
- **What is Bittensor?**
  A decentralized protocol for machine learning that rewards users for contributing to the network with TAO cryptocurrency.
  For more understanding of TAO watch this [Video](https://www.youtube.com/watch?v=WicvycWMbZA).

- **Why Mine Bittensor?**
  - Earn TAO tokens as rewards.
  - Contribute to decentralized AI.
  
- **Overview of Mining Process**
  - Set up dependencies.
  - Configure your miner.
  - Start mining and monitor your performance.

---

## **System Requirements -> DEPENDS ON THE SUBNET YOU WANT TO MINE**
- **Hardware Requirements**:
  - GPU: NVIDIA (CUDA-enabled) with at least 8GB VRAM.
  - CPU: At least 4 cores.
  - RAM: Minimum 16GB.
  - Storage: At least 50GB of free space.

- **Software Requirements**:
  - OS: Ubuntu 20.04 or newer / Windows Subsystem for Linux (WSL2).
  - Python: Version 3.8 or higher.
  - NVIDIA Drivers: Latest version.
  - CUDA Toolkit: Version 11.0 or higher.

---

## **Installation**
1. ### Get started in Bittensor
In order to mine on Bittensor you first need to install the following components:
  - [Bittensor SDK](https://docs.bittensor.com/getting-started/installation)
  - [Bittensor CLI](https://docs.bittensor.com/getting-started/install-btcli)
  - [Bittensor Wallet SDK](https://docs.bittensor.com/getting-started/install-wallet-sdk)
  - [Create Wallet](https://docs.bittensor.com/getting-started/wallets)
  - [Import Wallet](https://docs.bittensor.com/getting-started/wallets#store-your-mnemonics-safely)

  1. **Install Bittensor SDK**:

      You need to install these packages first if you are in a new environment:
      ```
      sudo apt update && sudo apt upgrade
      sudo apt install -y python3 python3-pip git build-essential curl
      sudo apt install python3.12-venv
      ```
    
      Once installed, you need to create an environment for the Bittensor SDK, activate it, download the bittensor repo and install the necessary components:
      ```
      python3 -m venv btsdk_venv
      source btsdk_venv/bin/activate
      git clone https://github.com/opentensor/bittensor.git
      cd bittensor
      pip install .
      ```

  2. **Install Bittensor CLI**:
      
      You need to create and activate a virtual environment, clone Bittensor CLI repo and install the requirements:
      ```
      python3 -m venv btcli_venv
      source btcli_venv/bin/activate
      cd btcli
      pip3 install .
      ```

  3. **Install Bittensor Wallet SDK**:

      Use this option if you want to use the Wallet SDK:
      ```
      python3 -m venv btwallet-venv
      source btwallet-venv/bin/activate
      pip install bittensor-wallet
      ```

  4. **Create Wallet**:

      If you do not have a wallet created, these are the steps required to create a wallet:

      First we activate the btcli environment:
      ```
      source btcli_venv/bin/activate
      ```

      Once activated to create a coldkey run the following command on your terminal by giving a name to your wallet, replacing the <my_coldkey>
      ```
      btcli wallet new_coldkey --wallet.name <my_coldkey>
      ```

      For example:
      ```
      btcli wallet new_coldkey --wallet.name test-coldkey
      ```

      You will see the following terminal output. The mnemonic is hidden for security reasons.
      ```
      IMPORTANT: Store this mnemonic in a secure (preferably offline place), as anyone who has possession of this mnemonic can use it to regenerate the key and access your tokens.
      The mnemonic to the new coldkey is:
      **** *** **** **** ***** **** *** **** **** **** ***** *****
      You can use the mnemonic to recreate the key in case it gets lost. The command to use to regenerate the key using this mnemonic is:
      btcli w regen_coldkey --mnemonic **** *** **** **** ***** **** *** **** **** **** ***** *****
      ```
      Then, provide this coldkey as a parameter to generate a hotkey. This will pair the hotkey with the coldkey. See below.
      Use the below command to generate the hotkey. Replace <my_coldkey> with the coldkey generated above, and <my_hotkey> with a name for your hotkey:
      ```
      btcli wallet new_hotkey --wallet.name <my_coldkey> --wallet.hotkey <my_hotkey>
      ```
      For example:
      ```
      btcli wallet new_hotkey --wallet.name test-coldkey --wallet.hotkey test-hotkey
      ```
      By using this command you will access to your hotkey and you will be able to see your "secretPhrase" and "ss58Address". Store both IDs.
      ```
      cd ~/.bittensor/wallets/test-coldkey
      cat hotkeys/test-hotkey | jq
      ```

      To locate your local wallets use this command:
      ```
      sudo apt install tree
      tree ~/.bittensor/
      ```
  5. **Import Wallet**:

      If you already have a wallet created, you need to get your ss58-address and use it in this command:
      ```
      btcli wallet regen-coldkeypub --ss58-address 5Ek....
	  btcli w regen-coldkey --mnemonic "brown fox ... dog"
      ```
      Then you need to regenerate your hotkey by using this command and changing **** for the 12 mnemonic words of your created wallet

      ```
      btcli wallet regen_hotkey --mnemonic "**** *** **** **** ***** **** *** **** **** **** ***** *****"
      ```

      Finally you can use this command to see your TAO balance:
      ```
      btcli wallet overview
      ```


2. ### Register, validate and Mine in Testnet (Kaito)
	The first step in order to mine in Kaito is to download the github repository.
	```
	git clone https://github.com/OpenKaito/openkaito.git
	cd openkaito
	pip install -e .
	python neurons/miner.py --netuid 88 --subtensor.network test --wallet.name testcoldkey --wallet.hotkey testhotkey --logging.debug --blacklist.force_validator_permit --axon.port 8091

	```
	The netuid for openkaito is 5 on mainnet, and 88 on testnet.


	In order to register your miner to the testnet you will have to request some TAO for the testnet in the Discord server of Bittensor.
	To do that you can write a message like this:
	```
	https://discord.com/channels/799672011265015819/1190048018184011867/1313215123082706995
	```

	Once we have received the TAO we must register the hotkey in the subnet. In this case netuid 88 by using this command:
	```
	btcli subnet register --netuid 88 --subtensor.network test --wallet.name coldkey --wallet.hotkey hotkey
	```


2. ### Register, validate and Mine in Mainet (Kaito)
	The first step in order to mine in Kaito is to download the github repository.
	```
	git clone https://github.com/OpenKaito/openkaito.git
	cd openkaito
	pip install -e .
	python neurons/miner.py --netuid 5 --subtensor.network mainnet --wallet.name maincoldkey --wallet.hotkey mainhotkey --logging.debug --blacklist.force_validator_permit --axon.port 8091

	```
	The netuid for openkaito is 5 on mainnet, and 88 on testnet.


	In order to register your miner to the testnet you will have to request some TAO for the testnet in the Discord server of Bittensor.
	To do that you can write a message like this:
	```
	https://discord.com/channels/799672011265015819/1190048018184011867/1313215123082706995
	```

	Once we have received the TAO we must register the hotkey in the subnet. In this case netuid 88 by using this command:
	```
	btcli subnet register --netuid 5 --subtensor.network finney --wallet.name maincoldkey --wallet.hotkey mainhotkey
	```


### Useful commands
List all subnets in the testnet 
```
btcli list subnet
```

First steps to start mining in Bittensor.

1- Install the "Bittensor SDK", "BTCLI" and "Wallet SDK" -> https://docs.bittensor.com/getting-started/installation (I am not sure if this is mandatory)
2- Create a wallet with hotkey and coldkey for a miner (the same for owner and validator)-> https://docs.bittensor.com/getting-started/wallets
3- Check your key in the blockchain-> btcli wallet overview --wallet.name miner
4- Register, Validate and Mine -> https://docs.bittensor.com/subnets/register-validate-mine
4.1 - Need to register your wallet in the subnet you want to mine
5- Select the Subnet you want to mine and read the doc (in my case is Subnet 5)-> https://github.com/OpenKaito/openkaito/blob/main/docs/running_on_mainnet.md


---->Useful commands:

Get COLDKEY ADDRESS:
cd ~/.bittensor/wallets/miner
cat coldkeypub.txt | jq

Elasticsearch password: x=b9TfiS0aoO4jvzG6ra




Message discord:

Hello guys.

I am trying to run the subnet locally following this guide. I am in this step: 
https://github.com/OpenKaito/openkaito/blob/main/docs/running_on_staging.md#5-initialize

I have this error.

error: the package 'subtensor' does not contain this feature: pow-faucet
help: packages with the missing feature: node-subtensor, node-subtensor-runtime, pallet-subtensor, subtensor-custom-rpc-runtime-api, subtensor-custom-rpc

Which feture should I use?


docker network create elastic
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.12.1
docker run --name elasticsearch --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -t docker.elastic.co/elasticsearch/elasticsearch:8.12.1