# How to build a DApp on TomoChain

## Guide on how to develop a simple Smart Contract written in Solidity, and deploy it on TomoChain testnet

This article will take you through the process of **building a basic DApp on TomoChain** — an adoption tracking system for a pet shop!

![Image](https://cdn-images-1.medium.com/max/1000/1*748rgm3xNS1qtAj-X87AXw.jpeg)

In this guide we will be covering:

- Setting up Truffle, the most popular development framework for Ethereum which also works perfectly for TomoChain
- Creating a Truffle project
- Creating a TomoChain wallet
- Requesting free tokens using TomoChain faucet
- Writing a smart contract
- Compiling and migrating the smart contract to TomoChain
- Connecting Metamask to TomoChain testnet
- Creating a user interface to interact with the smart contract

## What is TomoChain?

**[TomoChain](https://tomochain.com/)** is an innovative solution to the scalability problem with the Ethereum blockchain, and other blockchain platforms. TomoChain POSV masternode architecture offers **near-zero transaction fee and instant transaction confirmation. Security, stability and chain finality** are guaranteed via novel techniques such as double validation and uniform randomization.

TomoChain **supports all EVM-compatible smart-contracts, protocols, and atomic cross-chain token transfers.** Scaling techniques such as sharding, EVM parallelisation, private-chain generation, hardware integration will be continuously researched and integrated into TomoChain which will become an **ideal scalable smart-contract public blockchain for decentralized apps, token issuance and token integration for small and big businesses.**

> # *Every DApp running on Ethereum can be easily ported to TomoChain*

## Why should developers build DApps on TomoChain?

[![Watch the video](https://img.youtube.com/vi/8J4h2zsd_UE/maxresdefault.jpg)](https://youtu.be/8J4h2zsd_UE)
*(Click link to watch the Youtube video!)*

Remember *[CryptoKitties](https://www.cryptokitties.co/")* in 2017? A single DApp brought the whole Ethereum blockchain to their knees. The network was congested, with endless waiting times for transaction confirmation and high transaction fees. Porting to TomoChain would seem a good idea for the cute kitties.

TomoChain mainnet can process 2'000 TPS, wich is **100x faster than the Ethereum blockchain,** and for a fraction of the cost. If this is not good enough, the Vietnam-based company is working on its Sharding solution aiming to deliver 20'000–30'000 TPS by Q2 2019.

In this tutorial, we will see **how to build a DApp using Solidity** and then deploy it to **TomoChain** blockchain.

> ***Note:** Because deploying a smart contract on mainnet is much similar to testnet, the differences are just the configuration information, this document will explicitly mention the differences where possible*

---

## 0. Prerequisites

To start building your DApp you will need to install some programs:

- Install **[Node.js](https://nodejs.org/en/download/)** & **npm** (“Node.js Package Manager”)
- Install **[Git](https://git-scm.com/downloads)**

To check that Node is installed properly, open a console (admin PowerShell on Windows) and type `node -v`. This should print a version number, like `v10.15.0`.

To test npm, type `npm -v` and you should see the version number, like `6.4.1`.

---

## 1. Getting Started: Installation

**[Truffle Framework](https://truffleframework.com/)** is a great tool for developing DApps. You can use Truffle to deploy your smart contracts to TomoChain.

We only need this single command to install Truffle, the popular development framework for Ethereum.

```
npm install -g truffle
```

You can verify that Truffle is correctly installed typing `truffle version`.

---

## 2. Creating a Truffle project

Truffle initializes in the current directory, so first create a directory in your development folder of choice and then move inside it.

```
mkdir pet-shop-tutorial
cd pet-shop-tutorial
```

Let’s see **[how to create a Truffle project](https://truffleframework.com/docs/truffle/getting-started/creating-a-project).** There are two options. You can create a bare new project from scratch with no smart contracts included, and the other option for those just getting started, you can use **[Truffle Boxes](https://truffleframework.com/boxes)**, which are example applications and project templates.

![Image](https://cdn-images-1.medium.com/max/800/1*0iPGzZ_MuACqNeSRw1ymUg.png)

*Pet Shop Truffe Box*

There is a special Truffle Box for this tutorial called `pet-shop`, which includes the basic project structure as well as code for the user interface. Use the `truffle unbox` command to unpack this Truffle Box:

```
truffle unbox pet-shop
```

The default Truffle directory structure contains a series of folders and files. If you want to know more, please check [Truffle tutorials](https://truffleframework.com/tutorials/pet-shop#directory-structure).

> ***Note:** This tutorial is focused on the whole process to build a DApp on TomoChain, so we will not enter into all the details.*

---

## 3. Creating a TOMO Wallet

**You will need a wallet address and some tokens.** We will show you how to do it on both TomoChain Testnet and Mainnet.

### 3.1 Create a TOMO wallet and save your Mnemonic

You can create a new TOMO wallet using **TomoWallet** mobile app for [Android](https://play.google.com/store/apps/details?id=com.tomochain.wallet) or [iOS](https://itunes.apple.com/us/app/tomo-wallet/id1436476145?mt=8). Under *Settings* go to *Advanced Settings*, here you can *Choose network* and select `TomoChain TestNet` or `TomoChain` [mainnet].

Go to *Settings* menu, select *Backup wallet* and then **Continue**. Here you can see your wallet’s private key and the 12-word recovery phrase. **Write down the 12-word recovery phrase.**

![Image](https://cdn-images-1.medium.com/max/800/1*hRnmqGhR8mtCiT-AHb4C4A.png)
*TomoWallet*

You can also create a new [TomoChain wallet with MetaMask, MyEtherWallet or TrustWallet](https://docs.tomochain.com/get-started/wallet/). For instance, for mainnet you can go to [MyEtherWallet](https://www.myetherwallet.com/) and select **TOMO (tomochain.com)** on the top right corner. Enter a password and then Create a new wallet. **Write down your recovery phrase.**

For this tutorial, my wallet address (testnet) is:

```
0xc9b694877acd4e2e100e095788a591249c38b9c5
```

My recovery phrase (12-word mnemonic) is:

```
myth ahead spin horn minute tag spirit please gospel infant clog camera
```

Write them down. This will be needed later. **Notice that your wallet address (public key) and your recovery phrase will be different than mine.**

> ***Important!** Always keep your private key and recovery phrase **secret!***

### 3.2 Get some TOMO funds

Tokens are required for different matters, like smart contract deployment or to use in DApps.

**Testnet:** Receive 15 free testnet TOMO tokens using [TomoChain’s Faucet](https://faucet.testnet.tomochain.com/).

**Mainnet:** You need real TOMO tokens from exchanges.

Go to faucet and collect `30 TOMO`. Now your wallet has enough balance to do everything in this tutorial so… let’s go ahead!

### 3.3 The Block Explorer

To check the balance of a wallet address, you can use **TomoScan**.

**Testnet:** [https://scan.testnet.tomochain.com/](https://scan.testnet.tomochain.com/)

**Mainnet:** [https://scan.tomochain.com/](https://scan.tomochain.com/)

---

## 4. Writing the smart contract

We’ll start our DApp by writing the smart contract that acts as the back-end logic and storage.

1. Create a new file named `Adoption.sol` in the `contracts/` directory.

2. Copy the following code:

```
pragma solidity ^0.5.0;

contract Adoption {
  address[16] public adopters;
  
  // Adopting a pet
  function adopt(uint petId) public returns (uint) {
    // check that petId is in range of our adopters array
    require(petId >= 0 && petId <= 15);
    
    // add the address who called this function to our adopter array
    adopters[petId] = msg.sender;
    
    // return the petId provided as a confirmation
    return petId;
  }
  
  // Retrieving the adopters
  function getAdopters() public view returns (address[16] memory) {
    return adopters;
  }
  
}
```

> ***Note:** Code from [Truffle’s Pet-Shop tutorial](https://truffleframework.com/tutorials/pet-shop#writing-the-smart-contract) — if you want to look deeper into the Solidity code, they slowly go through the Truffle link explaining the details.*

---

## 5. Compiling

Solidity is a compiled language, meaning we need to compile our Solidity to bytecode for the **Ethereum Virtual Machine (EVM)** to execute. Think of it as translating our human-readable Solidity into something the EVM understands.

> # *TomoChain is EVM-compatible, which means that every contract written in Ethereum can be seamlessly ported to TomoChain without effort*

In a terminal, make sure you are in the root of the directory that contains the DApp and type:

```
truffle compile
```

You should see output similar to the following:

```
Compiling ./contracts/Migrations.sol... 
Compiling ./contracts/Adoption.sol... 
Writing artifacts to ./build/contracts
```

---

## 6. Migration — Deploying

Now that we’ve successfully compiled, it’s time to **migrate your smart contracts to TomoChain’s blockchain!**

**A migration is a deployment script meant to alter the state of your application’s contracts**, moving it from one state to the next. *(More about migrations in the [Truffle documentation](https://truffleframework.com/docs/truffle/getting-started/running-migrations)).*

### 6.1 Create the migration scripts

Open the `migrations/` directory and you will see one JavaScript file: `1_initial_migration_js`. This handles deploying the `Migrations.sol` contract to observe subsequent smart contract migrations, and ensures we don't double-migrate unchanged contracts in the future.

Now we are ready to create our own migration script.

1. Create a new file named `2_deploy_contracts.js` in the migrations/ directory.

2. Add the following content to the 2_deploy_contracts.js file:

```
var Adoption = artifacts.require("Adoption");

module.exports = function(deployer) {
  deployer.deploy(Adoption);
};
```

### 6.2 Configure the migration networks in truffle.js

Now we are almost ready to deploy to TomoChain. Let’s see [how to deploy your smart contract to a custom provider](https://truffleframework.com/tutorials/using-infura-custom-provider), any blockchain of your choice, like **TomoChain.**

Before starting the migration, we need to specify the **blockchain** where we want to deploy our smart contracts, specify the **address** to deploy — the wallet we just created, and optionally the gas, gas price, etc.

1. Install Truffle’s `HDWalletProvider`, a separate npm package to find and sign transactions for addresses derived from a 12-word `mnemonic` — in a certain blockchain. ([Read more about HDWalletProvider.](https://github.com/trufflesuite/truffle-hdwallet-provider))

```
npm install truffle-hdwallet-provider
```

2. Open `truffle.js` file (`truffle-config.js` on Windows). You can edit here the migration settings: networks, chain IDs, gas... The current file has only a single network defined, you can define multiple. We will add three networks to migrate our DApp: `development`, `tomotestnet` and `tomomainnet`.

The official [TomoChain documentation — Networks](https://docs.tomochain.com/general/networks/) is very handy. Both Testnet and Mainnet **network configurations** are described there. We need the `RPC endpoint`, the `Chain id` and the `HD derivation path`.

Replace the `truffle.js` file with this new content:

```
'use strict'

var HDWalletProvider = require("truffle-hdwallet-provider");

var mnemonic = '<PUT YOUR WALLET 12-WORD RECOVERY PHRASE HERE>';

module.exports = {
  networks: {
    development: {
      provider: () => new HDWalletProvider(
        mnemonic,
        "http://127.0.0.1:8545",
      ),
      host: "127.0.0.1",
      port: "8545",
      network_id: "*", // Match any network id
    },
    
    tomotestnet: {
      provider: () => new HDWalletProvider(
        mnemonic,
        "https://testnet.tomochain.com",
        0,
        1,
        true,
        "m/44'/889'/0'/0/",
      ),
      network_id: "89",
      gas: 2000000,
      gasPrice: 10000000000000,
    },
    
    tomomainnet: {
      provider: () => new HDWalletProvider(
        mnemonic,
        "https://rpc.tomochain.com",
        0,
        1,
        true,
        "m/44'/889'/0'/0/",
      ),
      network_id: "88",
      gas: 2000000,
      gasPrice: 10000000000000,
    }
  }
};
```

3. Remember to **update the `truffle.js` file using your own wallet recovery phrase.** Copy the 12 words obtained previously and paste it as the value of the `mnemonic` variable.

```
var mnemonic = '<PUT YOUR WALLET 12-WORD RECOVERY PHRASE HERE>';
```

Done. Please, notice the `tomotestnet` network will be used to deploy our smart contract. We have also added the `tomomainnet` network, in case you want to deploy to TomoChain Mainnet. However, if you are familiar with [Ganache](https://truffleframework.com/ganache), you could use the `development` network to do the local test as well if you want to. *Ganache is a locally running personal blockchain for Ethereum development you can use to deploy contracts, develop applications, and run tests.*

We have added the migration configuration so **we are now able to deploy to public blockchains like TomoChain** (both testnet and mainnet).

> ***Warning:** In production, we highly recommend storing the mnemonic in another secret file (loaded from environment variables or a secure secret management system), to reduce the risk of the mnemonic becoming known. If someone knows your mnemonic, they have all of your addresses and private keys!*

*Want to try? With npm package `dotenv` you can load an environment variable from a file `.env`, — then update your truffle.js to use this secret `mnemonic`.*

### 6.3 Start the migration

You should have your smart contract already compiled. Otherwise, now it’s a good time to do it with `truffle compile`.

Back in our terminal, migrate the contract to TomoChain testnet network:

```
truffle migrate --network **tomotestnet**
```

The migrations will start…

```
Starting migrations...
======================
> Network name:    'tomotestnet'
> Network id:      89
> Block gas limit: 84000000

1_initial_migration.js
======================

Deploying 'Migrations'
   ----------------------
   > transaction hash:    0x77d9cdf0fb810fd6cec8a5616a3519e7fa5d42ad07506802f0b6bc10fa9e8619
   > Blocks: 2            Seconds: 4
   > contract address:    0xA3919059C38b1783Ac41C336AAc6438ac5fd639d
   > account:             0xc9b694877AcD4e2E100e095788A591249c38b9c5
   > balance:             27.15156
   > gas used:            284844
   > gas price:           10000 gwei
   > value sent:          0 ETH
   > total cost:          2.84844 ETH
   
> Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:             2.84844 ETH
   
2_deploy_contracts.js
=====================

Deploying 'Adoption'
   --------------------
   > transaction hash:    0x1c48f603520147f8eebc984fadc944aa300ceab125cf40f77b1bb748460db272
   > Blocks: 2            Seconds: 4
   > contract address:    0xB4Bb4FebdA9ec02427767FFC86FfbC6C05Da2A73
   > account:             0xc9b694877AcD4e2E100e095788A591249c38b9c5
   > balance:             24.19238
   > gas used:            253884
   > gas price:           10000 gwei
   > value sent:          0 ETH
   > total cost:          2.53884 ETH
   
> Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:             2.53884 ETH
   
Summary
=======
> Total deployments:   2
> Final cost:          5.38728 ETH
```

The transaction ID is:

```
0x1c48f603520147f8eebc984fadc944aa300ceab125cf40f77b1bb748460db272
```

The contract address is:

```
0xB4Bb4FebdA9ec02427767FFC86FfbC6C05Da2A73
```

**Congratulations!** You have already deployed your smart contract to TomoChain. All this in just 8 seconds. We started with `30 TOMO` and the deployment has costed `5.38 TOMO` in gas fees.

> ***Note:** The command to deploy to TomoChain mainnet is very similar:
> `truffle migrate --network tomomainnet`*

### *** Troubleshooting ***

- **Error:** `smart contract creation cost is under allowance`. **Why?** Increasing transaction fees for smart contract creation is one of the ways TomoChain offers to defend against spamming attacks. **Solution:** edit truffle.js and add more gas/gasPrice to deploy.

- **Error:** `insufficient funds for gas * price + value`. **Why?** You don’t have enough tokens in your wallet for gas fees. **Solution:** you need more funds in your wallet to deploy, go to faucet and get more tokens.

### 6.4 Check the deployment transaction

If you want to verify that your contract was deployed successfully, you can check on **TomoScan** [testnet](https://scan.testnet.tomochain.com/) (or [mainnet](https://scan.tomochain.com/)). In the search field, type in the transaction ID for your new contract.

You should see details about the transaction, including the block number where the transaction was secured.

![Image](https://cdn-images-1.medium.com/max/800/1*7AlMGUJ6mz316IjIl85xSg.png)

*TomoScan transaction*

You can also enter your wallet address on the TomoScan search bar. You will find 4 transactions out. Your contract has been successfully deployed to TomoChain.

**Congratulations!** You’ve deployed your contract to TomoChain using Truffle. You have written your first smart contract and deployed it to a public blockchain. It’s time to interact with our smart contract now to make sure it does what we want.

---

## 7. Testing the smart contract

It is a good idea to test your smart contracts. You can write some tests in the `test/` directory and execute with `truffle test`. Find more details on [Truffle’s Pet Shop tutorial](https://truffleframework.com/tutorials/pet-shop#testing-the-smart-contract).

![Image](https://cdn-images-1.medium.com/max/800/1*sojw1kL7hDba7VxRrU3T6A.png)

---

## 8. Creating a user interface to interact with the smart contract

Now **we’ve created the smart contract and deployed it to TomoChain blockchain (testnet)**. It’s time to create a UI so that people can use the shop!

Included with the `pet-shop` Truffle Box was code for the app’s front-end. That code exists within the `src/` directory.

The front-end doesn’t use a build system (webpack, grunt, etc.) to be as easy as possible to get started. The structure of the app is already there; we’ll add in the functions unique to Ethereum.

1. Open `/src/js/app.js` in a text editor.

2. Examine the file. Note that there is a global `App` object to manage our application, load in the pet data in `init()` and then call the function `initWeb3()`. The [web3 JavaScript library](https://github.com/ethereum/web3.js/) interacts with the Ethereum blockchain. It can retrieve user accounts, send transactions, interact with smart contracts, and more.

3. The current file has some incomplete functions that you must fill in. Replace the old code and paste this **new code**

```
App = {
  web3Provider: null,
  
  contracts: {},
  
  init: async function() {
    // Load pets.
    $.getJSON('../pets.json', function(data) {
      var petsRow = $('#petsRow');
      var petTemplate = $('#petTemplate');
      for (i = 0; i < data.length; i ++) {
        petTemplate.find('.panel-title').text(data[i].name);
        petTemplate.find('img').attr('src', data[i].picture);
        petTemplate.find('.pet-breed').text(data[i].breed);
        petTemplate.find('.pet-age').text(data[i].age);
        petTemplate.find('.pet-location').text(data[i].location);
        petTemplate.find('.btn-adopt').attr('data-id', data[i].id);
        petsRow.append(petTemplate.html());
      }
    });
    return await App.initWeb3();
  },
  
  initWeb3: async function() {
    //----
    // Modern dapp browsers...
    if (window.ethereum) {
      App.web3Provider = window.ethereum;
      try {
        // Request account access
        await window.ethereum.enable();
      } catch (error) {
        // User denied account access...
        console.error("User denied account access")
      }
    }
    // Legacy dapp browsers...
    else if (window.web3) {
      App.web3Provider = window.web3.currentProvider;
    }
    // If no injected web3 instance is detected, fall back to Ganache
    else {
      App.web3Provider = new     Web3.providers.HttpProvider('http://localhost:7545');
    }
    web3 = new Web3(App.web3Provider);
    //----
    
    return App.initContract();
  },
  
  initContract: function() {
    //----
    $.getJSON('Adoption.json', function(data) {
      // Get the necessary contract artifact file and instantiate it with truffle-contract
      var AdoptionArtifact = data;
      App.contracts.Adoption = TruffleContract(AdoptionArtifact);
      // Set the provider for our contract
      App.contracts.Adoption.setProvider(App.web3Provider);
      // Use our contract to retrieve and mark the adopted pets
      return App.markAdopted();
    });
    //----
    
    return App.bindEvents();
  },
  
  bindEvents: function() {
    $(document).on('click', '.btn-adopt', App.handleAdopt);
  },
  
  markAdopted: function(adopters, account) {
    //----
    var adoptionInstance;
    App.contracts.Adoption.deployed().then(function(instance) {
      adoptionInstance = instance;
      return adoptionInstance.getAdopters.call();
    }).then(function(adopters) {
      for (i = 0; i < adopters.length; i++) {
        if (adopters[i] !== '0x0000000000000000000000000000000000000000') {
          $('.panel-pet').eq(i).find('button').text('Success').attr('disabled', true);
        }
      }
    }).catch(function(err) {
      console.log(err.message);
    });
    //----
  },
  
  handleAdopt: function(event) {
    event.preventDefault();
    var petId = parseInt($(event.target).data('id'));
    
    //----
    var adoptionInstance;
    web3.eth.getAccounts(function(error, accounts) {
      if (error) {
        console.log(error);
      }
      var account = accounts[0];
      App.contracts.Adoption.deployed().then(function(instance) {
        adoptionInstance = instance;
        // Execute adopt as a transaction by sending account
        return adoptionInstance.adopt(petId, {from: account});
      }).then(function(result) {
        return App.markAdopted();
      }).catch(function(err) {
        console.log(err.message);
      });
    });
    //---
  }
};

$(function() {
  $(window).load(function() {
    App.init();
  });
});
```

Here is what these functions do:

`initWeb3()` Checks if we are using modern DApp browsers or the more recent versions of [MetaMask](https://github.com/MetaMask).

`initContract()` Retrieves the artifact file for our smart contract. **Artifacts are information about our contract such as its deployed address and Application Binary Interface (ABI). The ABI is a JavaScript object defining how to interact with the contract including its variables, functions and their parameters.** We then call the app's `markAdopted()` function in case any pets are already adopted from a previous visit.

`markAdopted()` After calling `getAdopters()`, we then loop through all of them, checking to see if an address is stored for each pet. Ethereum initializes the array with 16 empty addresses. This is why we check for an empty address string rather than null or other falsey value. Once a `petId` with a corresponding address is found, we disable its Adopt button and change the button text to "Success", so the user gets some feedback.

`handleAdopt()` We get the deployed contract and store the instance in `adoptionInstance`. We're going to send a transaction instead of a call by executing the `adopt()` function with both the pet's ID and an object containing the account address. Then, we proceed to call our `markAdopted()` function to sync the UI with our newly stored data.

---

## 9. Interacting with the DApp in a browser

Now we’re ready to use our DApp! 

### 9.1 Install and configure MetaMask

1. Install the [MetaMask browser extension](https://metamask.io/) in Chrome or FireFox.

2. Once installed, you’ll see the MetaMask fox icon next to your address bar. Click the icon and MetaMask will open up.

3. Create a New password. Then, write down the Secret Backup Phrase and accept the terms. By default, MetaMask will create a new Ethereum address for you.

![Image](https://cdn-images-1.medium.com/max/800/1*tV2bQfZ2vVhvpOOwKY0Y5g.png)

*Initiating MetaMask*

4. Now we’re connected to the Ethereum network,with a brand new wallet with 0 ETH.

5. Let’s now connect MetaMask to TomoChain (testnet). Click the menu with the “Main Ethereum Network” and select **Custom RPC**. Use the [Networks data from TomoChain](https://docs.tomochain.com/general/networks/) (testnet) and click **Save.**

![Image](https://cdn-images-1.medium.com/max/800/1*Dm4qhGJOjnolRwxX-VN94w.png)
*Connecting MetaMask to TomoChain (testnet)*

6. The network name at the top will switch to say “TomoChain testnet”. Now that we are on TomoChain network we can import TomoChain wallets.

We could use the TOMO wallet we created previously, but better **let’s create a new TOMO wallet** and add a few TOMO tokens — you know how to do it.

7. Once you have created your new TOMO wallet, **copy the private key**. Back to MetaMask, click on the top-right circle and select **Import Account**. Paste the private key and *voilà*! Your TOMO wallet is loaded in MetaMask.

![Image](https://cdn-images-1.medium.com/max/800/1*AjEHidU-h0Ae0CXTsQUJ5Q.png)

*Importing a wallet*

### 9.2 Using the DApp

We will now start a local web server and interact with the DApp. We’re using the `lite-server`. This shipped with the pet-shop Truffle box.

The settings for this are in the files `bs-config.json` and `package.json`, if you want to take a look. These tell npm to run our local install of `lite-server` when we execute `npm run dev` from the console.

Start the local web server:

```
npm run dev
```

The dev server will launch and automatically open a new browser tab containing your DApp.

![Image](https://cdn-images-1.medium.com/max/800/1*gq766GpFC3UUCMoPW_3Isw.png)

*Pete’s Pet Shop*

Normally, a MetaMask notification automatically requests a connection.

2. To use the DApp, click the **Adopt** button on the pet of your choice.

3. You’ll be automatically prompted to aprove the transaction by MetaMask. Set some Gas and click **Confirm** to approve the transaction.

![Image](https://cdn-images-1.medium.com/max/800/1*KNOJi0WwGoYF7jy_AQz43Q.png)

*Adoption transaction review*

4. You’ll see the button next to the adopted pet change to say **“Success”** and become disabled, just as we specified, because the pet has now been adopted.

![Image](https://cdn-images-1.medium.com/max/800/0*iwMICrpZrGfJiNSY.png)

*Adoption success*

And in MetaMask you’ll see the transaction listed:

![Image](https://cdn-images-1.medium.com/max/800/1*iqZsMFlAA3NCkOO-xfEjiQ.png)

*MetaMask transaction*

**Congratulations!** You have taken a huge step to becoming a full-fledged DApp developer. You have all the tools you need to start making more advanced DApps and now you can make your DApp live for others to use deploying to TomoChain, *the most efficient blockchain for the token economy*!

