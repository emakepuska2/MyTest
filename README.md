# Instructions
This is a tutorial on starting with geth, ganache, truffle, mist wallet and ethereum smart contracts.

Firstly you will need to install all the mentioned tools.

The tools we will use
Truffle: A development environment, testing framework and asset pipeline for Ethereum. 
Ganache: It was called TestRPC before, what Ganache does is simple, it creates a virtual Ethereum blockchain, and it generates some fake accounts that we will use during development.
Mist: It’s a browser for decentralized web apps.
Ethereum wallet: It’s a version of Mist, but only opens one single dapp, the Ethereum Wallet. Mist and Ethereum Wallet are just UI fronts.

Installations
Truffle :npm install -g truffle
Ganache: npm install -g ganache-cli

If you clone the project just skip the command truffle init


To test our code, we will use Ganache
First command in a command prompt should be: ganache-cli -p 7545 (it tells you in which port is ganache working)
Ganache will generate test accounts for us, they have 100 ether by default, and are unlocked so we can send ether from them freely.

Now in another command prompt, in the root project we start truffle:
truffle compile
truffle migrate --network development


Compile will compile our Solidity code to bytecode (the code that the Ethereum Virtual Machine (EVM) understands), in our case, Ganache emulates the EVM.

Migrate will deploy the code to the blockchain, in our case, the blockchain could be found in the network “development” we set earlier in the “truffle-config.js” file.

To interact with ganache we use:
truffle console --network development

To assign the accounts addresses that are created in the ganache we execute these commands:

account0=web3.eth.accounts[0]
account1=web3.eth.accounts[1]



then  we will reference an instance of the contract that truffle deployed :
Wrestling.deployed().then(inst => { WrestlingInstance = inst })

TO see the accounts address we use:
WrestlingInstance.wrestler1.call()

Truffle, during the migration, picked up the default account from Ganache, which is the first account, because we didn’t specify another account address during the migration, or another address in the configuration file of Truffle.

Then we need to assign another account as an opponent:
WrestlingInstance.registerAsAnOpponent({from: account1})
Upon executing, in our cmd we will see info on how much gass was used, and the event name

REMARK: all the functions are declared in the smart contract called Wrestling.sol

We can do the same to take the address of account 1:
WrestlingInstance.wrestler2.call()

We can make wrestler 1 and 2 wrestle for ethers:

WrestlingInstance.wrestle({from: account0, value: web3.toWei(2, "ether")})
WrestlingInstance.wrestle({from: account1, value: web3.toWei(3, "ether")})


Creating private network on geth:
The file “genesis.json” is simply the configuration file that geth needs to create a new blockchain. 
If you run geth without specifying any parameter, it will try to connect to the mainnet. The mainnet is the main network of Ethereum, the real blockchain of Ethereum.

Open another command prompt to execute geth by using this command:
geth --datadir=./chaindata/ init ./genesis.json
We launch geth and  we specify where is the blockchain stored.
 Geth is started by writting: geth --datadir=./chaindata/ --rpc (rpc tells to geth to accept RPC connections- we use this so truffle can be connected to geth)

To connect mist with the private network we use the ipc connection that it is shown in the geth command prompt.
Open a new cmd and execute this command : mist --args --rpc ./(path that it is shown in your console) for example: /Users/project/chaindata/geth.ipc

After connection you can add new accounts in the wallets in mist, to get work done we should open another cmd and execute "geth attach" which allows us to interact with it.

With out main account created on the Mist UI, we will run this command inside of “geth attach” console:

miner.start()

It will start a miner, the process that will confirm transactions, and after a few seconds or minutes (depending or your computer), you should start seeing ether being added to your balance (And also to your main account)

In the command-line interface where we launched “geth attach”, we will unlock the account, so we can use it to migrate the Smart Contract from Truffle, using the following command:

personal.unlockAccount('your contract address that you create on Mist', 'your password')


Now, back to the command-line interface where we previously launched Truffle, run this command:

truffle migrate --network ourTestNet



The “ourTestNet” is the configuration necessary to connect to the geth instance. Geth starts by default on port 8545.

truffle console --network ourTestNet

Wrestling.address
JSON.stringify(Wrestling.abi)- this is done to get the Contracts ABI so we can interact with it in Mist Browser.
Be sure to copy the whole json format but do not include the apostrophes at the begining and at the end.

In Mist you have a field Contracts, there we are going to watch the Contract. 
In this for you add the contract address: given to you bby truffle (Wrestling.address) and the abi that you just coppied.

After this step you can interact with the contract based on the function that the contract has such as: Register as an Opponent, Wrestle and so on.

References and acknowledgments go to:https://hackernoon.com/ethereum-development-walkthrough-part-2-truffle-ganache-geth-and-mist-8d6320e12269

