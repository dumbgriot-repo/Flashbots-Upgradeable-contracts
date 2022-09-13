# Flashbots-Upgradeable-contracts

Lesson Type: Quiz
Estimated Time: 1-2 hours
Current Score: 100%
Maximal Extractable Value (MEV)
Image

MEV is a relatively new concept in the world of blockchains, and one that carries with it a lot of controversy. It refers to the maximum value that can be extracted from the block production apart from the standard block rewards.

Previously, it used to be called Miner Extractable Value, since miners were best positioned to extract value from block production, but as we move towards Proof of Stake and miners get replaced by validators, a more generic rename has been done to call it Maximal Extractable Value.

🤔 What does MEV stand for?


Maximal Extractable Value

Minimal Extractable Value

Miners Extracting Value
What is MEV?
In a nutshell, it's the concept of extracting value (profit) by making certain types of transactions on chain that are not block rewards themselves. Originally, it started happening because miners had control over which transactions they'd like to include in a block, and in which order. Since when creating a block, miners can include, exclude and change the transactions of the block as they wish, this means they can favor some transactions as compared to others and gain some additional profits by doing so. Note that we are talking about miners right now but things will change after the merge.

Typically, MEV happens when a certain transaction must be included in a certain block (usually the current block) to actually be profitable. Additionally, it sometimes may also need to be in a specific place in the block - for example buying a highly contested NFT drop right after the contract gets deployed, within the same block the contract was deployed, unlike us normies who wait at least until the next block so Metamask recognizes the contract as having been deployed before it sends a transaction out by which time gas price may have gone up significantly.

MEV Extraction
In theory, MEV could only be extracted by miners, and this was true in the early days. With miners having control over block production, they could guarauntee for themselves the execution of a profitable transaction, and just not include the transaction in the block if it didn't turn out to be profitable. Today, however, a large portion of MEV is extracted by independent network participants referred to as Searchers. We will learn more about exactly what they do, but in a nutshell they run complex algorithms to find opportunities for profiting on chain and have bots to automatically submit those transactions to the network.

Miners still continue to get a portion of the MEV profit made by searchers, as searchers generally tend to pay very high gas fees to try and ensure their transaction is included in the block. We'll look at some example cases shortly.

🤔 What makes MEV possible?


Poorly written smart contracts

Miners can choose which transactions to include in a block in which order

Miners can pretend to make transactions from other users keys thereby stealing any profit
Searchers
Searchers are participants which are looking for opportunities to make profitable transactions. These are generally regular users, who can code of course. Miners get benefited from these Searchers because "Searchers" usually have to pay very high gas fees to actually be able to make a profitable transaction as the competition is very high. One example that we studied was DEX Arbitrage in our flash loan example. While arbitrage could be done manually, the chances of you succeeding at doing that are miniscule. Searchers run bots to detect arbitrage opportunities on chain, and automatically submit transactions to make profit from such opportunities. Since arbitrage is one of the most common examples of MEV, searchers typically end up paying 90% of their profit to miners in gas fees to be included in the block.

This had led to the rise of research in the field of Gas Golfing - a fancy word for making minor optimizations to smart contracts and execution to try to minimize gas cost as much as possible, which allows Searchers to increase their gas price while lowering the gas fees thereby ending up with the same amount of total ETH paid for gas.

🤔 Why do searchers engage in doing gas golfing?


Decreasing gas cost allows them to increase gas price while staying within budget for gas fees to make a profit

Decreasing gas cost allows them to send their transaction at a cheaper gas price

Searchers enjoy partaking in competitive programming
Searchers use the concept of Gas Golfing to be able to program transactions in such a way that they use the least amount of gas. This is because of the formulae gas fees = gas price * gas used. So if you decrease your gas used, you can increase your gas price to arrive at the same gas fees. This helps in competitive MEV opportunities as by being able to pay higher gas fees than your competitors means that your chances of getting your transaction included are higher.

🤔 What role do Searchers play?


Searchers search and make public the best profit making opportunities on-chain

Searchers run complex algorithms and bots to try to make profit on-chain

Searchers search for MEV bots and shut them down
We talked about how to decrease gas usage in detail in one of our previous levels.

Frontrunning
There are generally two types of Searchers - those which are looking for very specific MEV opportunities, for example the NFT drop example mentioned above, and those which are running generalized bots willing to make all types of transactions which yield profit.

The latter is what are known as frontrunning bots. Frontrunners are bots that are actively looking through the mempool (transactions that have been sent by users but yet have not been mined) to search for profitable transactions. They replace the addresses with their own address, and test it locally to see if it's actually profitable. If it is they submit a modified transaction with a replaced address and a higher gas price than the original transaction, thereby extracting profit on the transaction.

Funny things can happen when multiple frontrunning bots get stuck in a loop of trying to frontrun each other, eventually ending up with virtually no profit at all.

Flashbots
We cannot talk about MEV without talking about Flashbots. Flashbots is an independent research project, which extended the go-ethereum client with a service that allows Searchers to submit transactions directly to Miners, without having to go through the public mempool. This means transactions submitted by a Searcher are not visible to others on the mempool unless they actually get included in a block by a miner, at which point it's too late to do anything about it.

As of today, the majority of MEV transactions take place through the Flashbots service, which means generalized frontrunners as described above are no longer as profitable as they used to be. Those frontrunners are based on the idea of copying transactions from the mempool and submitting them with a higher gas fees. Through Flashbots, however, transactions skip the mempool entirely, and therefore generalized frontrunners cannot pick up most of the MEV happening today.

🤔 Why do miners choose to run the Flashbots software?


Because they cannot keep track of all MEV opportunities alone

and make money from searcher's gas tips

Because miners created the Flashbots software

Because it is more efficient than running a regular mining node
Flashbots also democratized MEV much more, by providing direct access to miners for Searchers - thereby somewhat opening up the opportunity for regular users like you and me to extract MEV without being a miner ourselves.

🤔 Today, most of the MEV is extracted by?


Miners

Searchers

Relayers

Extractors
We will talk about Flashbots in more detail soon.

Use cases of MEV and Flashbots
We have discussed arbitrage above, let's look at a few more examples of MEV.

Liquidations
In DeFi lending protocols, where you can borrow an asset against collateral (for eg borrowing USDC against ETH), there is the concept of liquidation. When you borrow assets against collateral, you can only borrow less money than what your collateral is worth. For example, if I deposit 1 ETH when ETH is $3,000 - I can only borrow less than 3,000 USDC from the lending protocol. Different protocols set different limits, but typically no one lets you borrow 100% of your collateral because that means the smallest price movement in your collateral will affect your borrowed amount.

The closer I get to the upper range of my borrowing amount, the higher chance I have of getting liquidated. There might be a case because of market fluctuations that the value of your borrowed assets exceeds the value of the collateral you supplied, or exceeds the upper limit of how much you were allowed to borrow. In that case, your original collateral is snatched away

Every protocol has a different percentage but after that percentage is hit, most of the protocols allow anyone from the outside world to liquidate the borrower. Consider this very similar to how if someone doesn't pay the loan on time, their house which they kept as collateral is auctioned by the bank where the bank takes away the original value of the loan + interest and returns back the money which is left over.

Similarly, when the borrower gets liquidated, some part of their collateral goes to the lender which includes interest and the borrowed money. Along with that the borrower also has to pay a liquidation fee which goes to the user/bot which started the liquidation transaction.

Searchers run algorithms to keep track of borrowers on various lending protocols to detect if someone can be liquidated. If they find an opportunity to liquidate someone, they can extract MEV from that opportunity by being the first to get their liquidation transaction mined therefore earning the liquidating fees.

Sandwich Attacks
Sandwich Trading is a concept that perfectly fits the use case of Flashbots and MEV. Let's explain this with an example.

Suppose a user was attempting to make a large trade on a DEX, looking to sell a lot of Token A for Token B.

Selling A for B would push down the price of A and increase the price of B after the trade is executed. A second transaction, which is the exact same, made after this first one would yield in a lower amount of Token B for the same amount of Token A. Searchers can exploit this fact to actually make the user pay more for Token B than they may have originally anticipated (recall Slippage from DeFi Exchange level in Sophomore).

Since Flashbots lets you communicate with miners directly, and miners decide the order of transactions in a block, you can actually design your flashbots transactions in a way where you can specify the order in which you want your transactions to be executed. By doing so, searchers can create two transactions - one before the user's trade, and one after the user's trade - to make a profit as follows.

Searcher sells a lot of Token A for Token B, driving down the price of Token A and driving up the price of Token B
User's transaction goes through, which also sells a lot of Token A for Token B, but receives less Token B than originally anticipated. This further drives down Token A price and increases Token B price.
Searcher sells back their Token B for Token A, ending up with more Token A than they started off with, making a profit.
Since searchers add two transactions to the block right before and after the user's large trade, it is called a Sandwich Attack.

Sandwich Attack bots became so rampant with the rise of MEV and Flashbots that special decentralized exchanges had to be made with functionality to prevent exactly this. For example, mistX is a DEX that itself submits all trade transactions through Flashbots, thereby bypassing the mempool entirely, which means other Searchers cannot sandwich attack trades happening on mistX, though the miner itself still can.

Sandwich Attacks are also a big reason why privacy focused L2's and private trades are a hot topic, as if it is not possible for a Searcher of miner to know who's address is trading what and how much of it, they cannot sandwich that trade.

Recovering funds from compromised accounts
Let's look at a use case of Flashbots that is in fact a net positive, and doesn't hurt other legitimate users in the process unlike Sandwich Attacks.

Suppose your ETH private key gets stolen, and you had a bunch of mainnet funds and NFTs on it. There exist a lot of bots which scour Github for private keys which were accidentally pushed to try to steal funds from that account. Generally they are looking for ETH or other well known tokens, and often times don't care about NFTs, unclaimed airdrop tokens, lesser known tokens, or tokens staked in DeFi pools and protocols.

It is possible then, and happens occasionally, that a bot would steal all your ETH and tokens, but there are other assets they did not care about that are worth a decent amount and you want to retrieve them. Unfortunately, you need ETH to pay gas for transactions, and the second you send any ETH to a compromised account, it is likely to be stolen by the bot before you can do anything.

Enter Flashbots. A lot of people have used Flashbots for good in this case, where they can design a bundle of transactions which would first send some ETH to the compromised account from a different account, and then withdraw all the assets the bot did not steal automatically, and ask the miner to order them in that sequential order within the same block. By skipping the mempool and being sure that both transactions will get included within the same block, bots can no longer frontrun your transaction and steal your ETH before you can withdraw your assets.

In fact, this exact approach has led to countless people recovering (hundreds of) thousands from stolen accounts.

🤔 What is an example of an MEV opportunity?


Liquidations on AAVE

Trading on Uniswap

Minting the next blue chip NFT

All of the above
The good and the bad
Pros
Its good for many Defi projects because arbitrage corrects the price between multiple DEX's and protocols rely on liquidations to ensure that the borrowers don't fall below their collateral which hurts the DeFi protocol. Also, use cases like recovering stolen funds are arguably a net-positive that helps the space.

Cons
The cons are that frontrunners and gas wars result in very high gas prices for the users and it's not good for the user experience. Also, there may be a case where a miner may figure out after mining a block that there was an MEV opportunity where they could have made more profit. So in this case, they can remine blocks to include the MEV transaction which can cause instability in the system.

We learnt before that blocks can be reorganized if a fork happened due to latency on the network, and eventually the longest chain was chosen as the canonical chain which may involve certain blocks getting deleted and replaced on nodes which were following the other (shorter) chain. However, if the MEV opportunity is large enough, miners may try to deliberately cause a fork and reorganize blocks just to try to make profit, though it also involves some luck as they now have to be able to produce a longer chain than the other one.

Solution?
We briefly talked about Flashbots above but essentially Flashbots are being built so that they can solve the Cons related to MEV. They are trying to build an ecosystem that is permissionless, transparent, and fair for having better MEV and protection against front running. By democratizing access to MEV opportunities beyond just miners, Flashbots allows users like you and me to also act upon these opportunities instead of just being taken advantage of.

The issue with the current Ethereum ecosystem is that the transaction is sent in an open mempool that is viewable by all which leads to the potential for frontrunning. It also has the added disadvantage that if you send the transaction but your transaction actually doesn't get executed, you still end up losing some gas.

On the other hand, not having a public mempool is quite questionable and shady as well, as that means for sure the miners are the only one who can extract MEV. A good example of this is the Avalanche chain, where they modified the go-ethereum client to hide the public mempool entirely and only miners can receive pending transactions. This led to miners extracting all the MEV and users weren't able to do anything about it.

As a result, it spawned DAO's and projects that collaborated their resources to set up mining nodes, and then sold access to the mempool to multiple users for a fraction of the cost of running your own Avalanche node (which is currently upwards of $200,000 USD).

Instead, Flashbots created something which is really cool, they decided to allow users to privately communicate the transaction order they want and bid the amount they are willing to give. If their bid fails, the user doesn't end up paying anything, as the miner doesn't get anything if the transaction isn't profitable. This mechanism eliminates front running.

🤔 Why are frontrunning bots not common anymore?


Because frontrunning is not profitable anymore

Because there are no more profitable opportunities on Ethereum

Because most MEV goes through Flashbots today which skips the public mempool
Architecture of Flash bots
Let's understand further down how everything works underneath in flashbots

Image

(Referenced from the docs of flashbots)

As you can see in the diagram there are three parties involved the searcher, the relay, and the miner.

Essentially Searcher wants to send a private request to the miner so that his transactions are not viewable in the open mempool of Ethereum and thus preventing him from front running.

The Searcher expresses their bids for inclusion through ethereum transactions in the form of gas price or direct payment to the coinbase address (address of the miner). Using direct payments instead of gas prices allows users to make payments only if their bids are successful, whereas only having a high gas fees means miners are still incentivized to include your transaction even if it not profitable, because they will still get the gas fees from you. However, using direct payments means your transactions must be executed through a smart contract (and not an EOA) because in smart contracts you have access to the block.coinbase variable. Depending on use case, you may need to deploy a new smart contract to mainnet for this to be possible.

Now the issue comes because "Searcher" doesn't have to pay anything for the failed bids, they might do a DOS attack and spam the miners with invalid transaction bundles. To prevent this the user's transactions are first sent to the relayer who validates the bundle and also does bundle merging. The relayer also has a reputation system, where legitimate searchers build up their reputation over time whereas DOS attackers and such lose reputation thereby making it harder for them to mess with the system.

The official documentation of flashbots suggests not to integrate with relayers other than the ones from Flashbots until the system is fully decentralized because relayers have access to the full transaction data and may use it for their own benefits.

🤔 Flashbots Relay is currently centralized?


True

False
The miners which want to support the Flashbots relay run the MEV-modified Ethereum client code, for eg. mev-geth which is an extension of go-ethereum. As of today, over 80% of Ethereum miners are using the MEV-modified version of Ethereum nodes.

The miner can evaluate transaction bundles and combine the ones which don't conflict to create the most profitable block.

eth_sendBundle
Flashbots introduced a new eth_sendBundle RPC which is a standard format to interact with the flashbot relayers and miners. It includes array of arbitrary signed Ethereum transactions along with some metadata

Here is a list of all the params it takes:

{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_sendBundle",
  "params": [
    {
      txs,               // Array[String], A list of signed transactions to execute in an atomic bundle
      blockNumber,       // String, a hex encoded block number for which this bundle is valid on
      minTimestamp,      // (Optional) Number, the minimum timestamp for which this bundle is valid, in seconds since the unix epoch
      maxTimestamp,      // (Optional) Number, the maximum timestamp for which this bundle is valid, in seconds since the unix epoch
      revertingTxHashes, // (Optional) Array[String], A list of tx hashes that are allowed to revert
    }
  ]
}
🤔 What is the new RPC call introduced by Flashbots?


eth_flashbots

eth_sendBundle

eth_mevTransaction
Image(Referenced from the docs of flashbots)

Explorer
To view how MEV is progressing over the years, the flashbot team has built an explorer. Do check it out, it's amazing. Follow this link

Example of MEV
A real world arbitrage MEV transaction
Right now most of the flashbots work is still in the research and early phases. But as we all can understand, it has a lot of potentials.

Web3 is full of potential and we are just getting started 🚀 👀

Just wow 🤯

Recommended Readings
Ethereum is a Dark Forest
Escaping the Dark Forest
References
Ethereum.org docs
Flashbots docs
Submit Quiz
0 / 10

MEV Practical
In MEV Theory, we understood what MEV is, what Flashbots are, and some use cases of Flashbots. In this level we will learn how to mint an NFT using Flashbots. This is going to be a very simple use case designed to teach you how to use Flashbots, not necessarily make a profit. Finding opportunities where you can make profit using MEV is a hard problem and are typically not public information. Every Searcher is trying to do their best, and if they tell you exactly what strategies they're using, they are shooting themselves in the foot.

This tutorial is just meant to show you how you use Flashbots to send transactions in the first place, the rest is up to you!

Build
Lets build an example on how to usee flashbots

To setup a Hardhat project, Open up a terminal and execute these commands

npm init --yes
npm install --save-dev hardhat
If you are on a Windows machine, please do this extra step and install these libraries as well :)

npm install --save-dev @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers
In the same directory where you installed Hardhat run:

npx hardhat
Select Create a basic sample project
Press enter for the already specified Hardhat Project root
Press enter for the question if you want to add a .gitignore
Press enter for Do you want to install this sample project's dependencies with npm (@nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers)?
Now you have a hardhat project ready to go!

Let's install a few more dependencies to help us further

npm install @flashbots/ethers-provider-bundle @openzeppelin/contracts dotenv
Let's start off by creating a FakeNFT Contract. Under your contracts folder create a new file named FakeNFT.sol and add the following lines of code to it

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract FakeNFT is ERC721 {

    uint256 tokenId = 1;
    uint256 constant price = 0.01 ether;
    constructor() ERC721("FAKE", "FAKE") {
    }

    function mint() public payable {
        require(msg.value == price, "Ether sent is incorrect");
        _mint(msg.sender, tokenId);
        tokenId += 1;
    }
}
This is a pretty simple ERC-721 contract that allows minting an NFT for 0.01 ETH.

Now let's replace the code present in hardhat.config.js with the following lines of code

require("@nomiclabs/hardhat-waffle");
require("dotenv").config({ path: ".env" });

const QUICKNODE_RPC_URL = process.env.QUICKNODE_RPC_URL;

const PRIVATE_KEY = process.env.PRIVATE_KEY;

module.exports = {
  solidity: "0.8.4",
  networks: {
    goerli: {
      url: QUICKNODE_RPC_URL,
      accounts: [PRIVATE_KEY],
    },
  },
};
Note that we are using goerli here which is an Ethereum testnet, similar to Rinkeby and Ropsten, but the only one supported by Flashbots.

Now its time to set up some environment variables, create a new file .env under your root folder, and add the following lines of code to it.

QUICKNODE_RPC_URL="QUICKNODE_RPC_URL"
PRIVATE_KEY="YOUR-PRIVATE-KEY"
QUICKNODE_WS_URL="QUICKNODE_WS_URL"
To get your QUICKNODE_RPC_URL and QUICKNODE_WS_URL go to Quicknode, sign in, and create a new endpoint. Select Ethereum and then Goerli, and create the endpoint in Discover mode to remain on the free tier.

Image

Now copy the HTTP Provider url and paste it inplace of QUICKNODE_RPC_URL and copy WSS Provider and paste it in place of QUICKNODE_WS_URL.

Replace YOUR-PRIVATE-KEY with the private key of an account in which you have Goerli Ether, to get some Goerli ether try out this faucet

Now it's time to write some code that will help us interact with Flashbots.

Create a new file under scripts folder and name it flashbots.js and add the following lines of code to it

const {
  FlashbotsBundleProvider,
} = require("@flashbots/ethers-provider-bundle");
const { BigNumber } = require("ethers");
const { ethers } = require("hardhat");
require("dotenv").config({ path: ".env" });

async function main() {
  // Deploy FakeNFT Contract
  const fakeNFT = await ethers.getContractFactory("FakeNFT");
  const FakeNFT = await fakeNFT.deploy();
  await FakeNFT.deployed();

  console.log("Address of Fake NFT Contract:", FakeNFT.address);

  // Create a Alchemy WebSocket Provider
  const provider = new ethers.providers.WebSocketProvider(
    process.env.QUICKNODE_WS_URL,
    "goerli"
  );

  // Wrap your private key in the ethers Wallet class
  const signer = new ethers.Wallet(process.env.PRIVATE_KEY, provider);

  // Create a Flashbots Provider which will forward the request to the relayer
  // Which will further send it to the flashbot miner
  const flashbotsProvider = await FlashbotsBundleProvider.create(
    provider,
    signer,
    // URL for the flashbots relayer
    "https://relay-goerli.flashbots.net",
    "goerli"
  );

  provider.on("block", async (blockNumber) => {
    console.log("Block Number: ", blockNumber);
    // Send a bundle of transactions to the flashbot relayer
    const bundleResponse = await flashbotsProvider.sendBundle(
      [
        {
          transaction: {
            // ChainId for the Goerli network
            chainId: 5,
            // EIP-1559
            type: 2,
            // Value of 1 FakeNFT
            value: ethers.utils.parseEther("0.01"),
            // Address of the FakeNFT
            to: FakeNFT.address,
            // In the data field, we pass the function selctor of the mint function
            data: FakeNFT.interface.getSighash("mint()"),
            // Max Gas Fes you are willing to pay
            maxFeePerGas: BigNumber.from(10).pow(9).mul(3),
            // Max Priority gas fees you are willing to pay
            maxPriorityFeePerGas: BigNumber.from(10).pow(9).mul(2),
          },
          signer: signer,
        },
      ],
      blockNumber + 1
    );

    // If an error is present, log it
    if ("error" in bundleResponse) {
      console.log(bundleResponse.error.message);
    }
  });
}

main();
Now let's try to understand what's happening in these lines of code.

In the initial lines of code, we deployed the FakeNFT contract which we wrote.

After that we created an Quicknode WebSocket Provider, a signer and a Flashbots provider. Note the reason why we created a WebSocket provider this time is because we want to create a socket to listen to every new block that comes in Goerli network. HTTP Providers, as we had been using previously, work on a request-response model, where a client sends a request to a server, and the server responds back. In the case of WebSockets, however, the client opens a connection with the WebSocket server once, and then the server continuously sends them updates as long as the connection remains open. Therefore the client does not need to send requests again and again.

The reason to do that is that all miners in Goerli network are not flashbot miners. This means for some blocks it might happen that the bundle of transactions you send dont get included.

As a reason, we listen for each block and send a request in each block so that when the coinbase miner(miner of the current block) is a flashbots miner, our transaction gets included.

// Create a Alchemy WebSocket Provider
  const provider = new ethers.providers.WebSocketProvider(
    process.env.QUICKNODE_WS_URL,
    "goerli"
  );

  // Wrap your private key in the ethers Wallet class
  const signer = new ethers.Wallet(process.env.PRIVATE_KEY, provider);

  // Create a Flashbots Provider which will forward the request to the relayer
  // Which will further send it to the flashbot miner
  const flashbotsProvider = await FlashbotsBundleProvider.create(
    provider,
    signer,
    // URL for the goerli flashbots relayer
    "https://relay-goerli.flashbots.net",
    "goerli"
  );
After initializing the providers and signers, we use our provider to listen for the block event. Every time a block event is called, we print the block number and send a bundle of transactions to mint the NFT. Note the bundle we are sending may or may not get included in the current block depending on whether the coinbase miner is a flashbot miner or not.

Now to create the transaction object, we specify the chainId which is 5 for Goerli, type which is 2 because we will use the Post-London Upgrade gas model which is EIP-1559. To refresh your memory on how this gas model works, check out the Gas module in Sophomore.

We specify value which is 0.01 because that's the amount for minting 1 NFT and the to address which is the address of FakeNFT contract.

Now for data we need to specify the function selector which is the first four bytes of the Keccak-256 (SHA-3) hash of the name and the arguments of the function This will determine which function are we trying to call, in our case, it will be the mint function.

Then we specify the maxFeePerGas and maxPriorityFeePerGas to be 3 GWEI and 2 GWEI respectively. Note the values I got here are from looking at the transactions which were mined previously in the network and what Gas Fees were they using.

also, 1 GWEI = 10*WEI = 10*10^8 = 10^9

We want the transaction to be mined in the next block, so we add 1 to the current blocknumber and send this bundle of transactions.

After sending the bundle, we get a bundleResponse on which we check if there was an error or not, if yes we log it.

Now note, getting a response doesn't guarantee that our bundle will get included in the next block or not. To check if it will get included in the next block or not you can use bundleResponse.wait() but for the sake of this tutorial, we will just wait patiently for a few blocks and observe.

provider.on("block", async (blockNumber) => {
    console.log("Block Number: ", blockNumber);
    // Send a bundle of transactions to the flashbot relayer
    const bundleResponse = await flashbotsProvider.sendBundle(
      [
        {
          transaction: {
            // ChainId for the Goerli network
            chainId: 5,
            // EIP-1559
            type: 2,
            // Value of 1 FakeNFT
            value: ethers.utils.parseEther("0.01"),
            // Address of the FakeNFT
            to: FakeNFT.address,
            // In the data field, we pass the function selctor of the mint function
            data: FakeNFT.interface.getSighash("mint()"),
            // Max Gas Fees you are willing to pay
            maxFeePerGas: BigNumber.from(10).pow(9).mul(3),
            // Max Priority gas fees you are willing to pay
            maxPriorityFeePerGas: BigNumber.from(10).pow(9).mul(2),
          },
          signer: signer,
        },
      ],
      blockNumber + 1
    );

    // If an error is present, log it
    if ("error" in bundleResponse) {
      console.log(bundleResponse.error.message);
    }
  });
Now to run this code, in your terminal pointing to the root directory execute the following command:

npx hardhat run scripts/flashbots.js --network goerli
After an address is printed on your terminal, go to Goerli Etherscan and keep refreshing the page till you see Mint transaction appear(Note it takes some time for it to appear cause the flashbot miner has to be the coinbase miner for our bundle to be included in the block)

Image

Image

Boom 🤯 We now learned how to use flashbots to mint a NFT but you can do so much more 👀

GG 🥳

Readings
Flashbots Docs
Arbitrage bot using Flashbots
Submit Practical
Verify your smart contract address to pass the assessment for this level.


Ethereum Rinkeby
0x...

Lesson Type: Quiz
Estimated Time: 1-2 hours
Current Score: 100%
Metatransactions and Signature Replay
There are times when you want your dApp users to have a gas-less experience, or perhaps make a transaction without actually putting something on the chain. These types of transactions are called meta-transactions, and in this level, we will dive deep into how to design meta-transactions and also how they can be exploited if not designed carefully.

For those of you who have used OpenSea, ever noticed how OpenSea lets you make listings of your NFT for free? No matter what price you want to sell your NFT at, somehow it never charges gas beyond the initial NFT Approval transaction? The answer, Meta transactions.

Meta transactions are also commonly used for gas-less transaction experiences, for example asking the users to sign a message to claim an NFT, instead of paying gas to send a transaction to claim an NFT.

There are other use cases as well, for example letting users pay for gas fees with any token, or even fiat without needing to convert to crypto. Multisig wallets also only ask the last signer to pay gas for the transaction being made from the multisig wallet, and the other users just sign some messages.

🤔 Which of the following is NOT a valid use case for Meta Transactions?


Covering gas for early users of your dApp

Allowing users to pay in tokens other than ETH

Guaranteeing your transaction will be included in the next block
Isn't this really cool? 🎉

How does it work?
Meta transactions are just a fancy word for transactions where a third party (a relayer) pays the gas for the transaction on behalf of the user. The user just needs to sign messages (and not send a transaction) that contain information about the transaction they want to execute and hand it to the relayer. Relayers are then responsible for creating valid transactions using this data and paying for gas themselves.

Relayers could be, for example, code in your dApp to let your users experience gas-less transactions, or it could be a third-party company that you pay in fiat to execute transactions on Ethereum, etc.

At this level, we will do two things - learn about meta transactions and how to build a simple smart contract that supports meta transactions, and also learn about a security bug in the first contract we build and how to fix it. Kill two birds with one stone 😎.

🤔 What role does a relayer play in the context of metatransactions?


Relayer is responsible for taking the signed message and passing it to the backend service

Relayer bridges your tokens from Layer 1 to Layer 2 for cheaper transactions

Relayer pays the gas for transactions initiated by the users through a signature
Digital Signatures
In the previous IPFS levels, we have explained what hashing is. Before moving on, we need to understand a second fundamental concept of cryptography - digital signatures.

Image

A digital signature is a mathematical way to verify the authenticity of a message. Given some data, a private key can be used to sign that data and produce a signed message.

Then, someone else who believes they have the original input data can compare it to the signed message to retrieve the public key which signed the message. Since the public key of the sender must have been known beforehand, they can be assured that the data has not been manipulated and its integrity is upheld.

So in the example above, Alice signs the message Hello Bob! using her private key and sends it to Bob. Bob already knows that the content is Hello Bob! and now wants to verify that the message was indeed sent from Alice, so he tries to retrieve the public key from the signed message. If the public key is Alice's public key then the message was not tampered with and was securely transmitted.

Use Cases of Digital Signatures
Digital signatures are commonly used when wanting to communicate securely and ensuring that the contents of the messages were not manipulated during communication. You can send the original plaintext message, along with a signed message from a known sender, and the receiver can verify the integrity of the message by deriving the public key of the signer by comparing the plaintext message and the signed message. If the public key of the signer matches the public key of the expected sender, the message was transmitted securely!

🤔 What are digital signatures used for?


Storing passwords in a database

Storing private data by encrypting it for a certain recipient

Ensuring data integrity and proof that a certain person sent that message
Digital Signatures on Ethereum
In the case of Ethereum, user wallets can sign messages, and then the original input data can be verified with the signed message to retrieve the address of the user who signed the message. Signature verification can be done both off-chain as well as within Solidity. Here in this case the address of the user is the public key.

Understanding the concepts
Let's say we want to build a dApp where users can transfer tokens to other addresses without needing to pay gas, except just once for approval. Since the user themself will not be sending the transaction, we will be covering the gas for them 🤑.

This design is quite similar to how OpenSea works, where once you pay gas to provide approval over an ERC-721 or ERC-1155 collection, listings and sales on OpenSea are free for the seller. OpenSea only asks the sellers to sign a transaction when putting up a listing, and when a buyer comes along, the seller's signature is submitted alongside the buyer's transaction thereby transferring the NFT to the buyer and transferring the ETH to the seller - and the buyer pays all the gas for this to happen.

In our case, we will just have a relayer letting you transfer ERC-20 tokens to other addresses after initial approval 🎉.

Let's think about what all information is needed to make this happen. The smart contract needs to know everything about the token transfer, so we at least need to incorporate 4 things into the signed message:

sender address
recipient address
amount of tokens to transfer
tokenContract address of the ERC-20 smart contract
The flow will look something like this:

User first approves the TokenSender contract for infinite token transfers (using ERC20 approve function)
User signs a message containing the above information
Relayer calls the smart contract and passes along the signed message, and pays for gas
Smart contract verifies the signature and decodes the message data, and transfers the tokens from sender to recipient
Thankfully, ethers.js comes with a function called signMessage which lets us sign messages easily. However, to avoid producing arbitrary length messages, we will first hash the required data and then sign it. In the smart contract as well, we will first hash the given arguments, and then try to verify the signature against the hash.

We will code this up as a Hardhat test, so first, we need to write the smart contract.

BUIDL
Let's build an example where you can experience meta transactions in action, and we will later extend the test to show the vulnerability.

To set up a Hardhat project, Open up a terminal and execute these commands

npm init --yes
npm install --save-dev hardhat
If you are on Windows, please do this extra step and install these libraries as well :)

npm install --save-dev @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers
In the same directory where you installed Hardhat run:

npx hardhat
Select Create a basic sample project
Press enter for the already specified Hardhat Project root
Press enter for the question on if you want to add a .gitignore
Press enter for Do you want to install this sample project's dependencies with npm (@nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers)?
Now you have a hardhat project ready to go!

Let's also install OpenZeppelin contracts in the same terminal.

npm install @openzeppelin/contracts
We will create two smart contracts. The first is a super simple ERC-20 implementation, and the second is our TokenSender contract. For simplicity, we will create them both in the same .sol file.

Let's create a Solidity file named MetaTokenSender.sol in the contracts/ directory with the following code

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

contract RandomToken is ERC20 {
    constructor() ERC20("", "") {}

    function freeMint(uint amount) public {
        _mint(msg.sender, amount);
    }
}

contract TokenSender {

    using ECDSA for bytes32;

    function transfer(address sender, uint amount, address recipient, address tokenContract, bytes memory signature) public {
        bytes32 messageHash = getHash(sender, amount, recipient, tokenContract);
        bytes32 signedMessageHash = messageHash.toEthSignedMessageHash();

        address signer = signedMessageHash.recover(signature);

        require(signer == sender, "Signature does not come from sender");

        bool sent = ERC20(tokenContract).transferFrom(sender, recipient, amount);
        require(sent, "Transfer failed");
    }

    function getHash(address sender, uint amount, address recipient, address tokenContract) public pure returns (bytes32) {
        return keccak256(abi.encodePacked(sender, amount, recipient, tokenContract));
    }
}
Let's take a look at what is going on. First, let's take a look at the imports.

The Imports
The ERC20.sol import is to inherit the base ERC-20 contract implementation from OpenZeppelin, which we have learned a lot about in previous tracks.

ECDSA stands for Elliptic Curve Digital Signature Algorithm - it is the signatures algorithm used by Ethereum, and the OpenZeppelin library for ECDSA.sol contains some helper functions used for digital signatures in Solidity.

🤔 Which digital signatures algorithm does Ethereum use?


ECDSA

RSA

keccak256
The Functions
The ERC-20 contract is quite self-explanatory, as all it does is let you mint an arbitrary amount of free tokens.

For TokenSender, there are two functions here. Let's first look at the helper function - getHash - which takes in the sender address, amount of tokens, recipient address, and tokenContract address, and returns the keccak256 hash of them packed together. abi.encodePacked converts all the specified values in bytes leaving no padding in between and passes it to keccak256 which is a hashing function used by Ethereum. This is a pure function so we will also be using this client-side through Javascript to avoid dealing with keccak hashing and packed encodes in Javascript which can be a bit annoying.

The transfer function is the interesting one, which takes in the above four parameters, and a signature. It calculates the hash using the getHash helper. After that the message hash is converted to a Ethereum Signed Message Hash according to the EIP-191 . Calling this function converts the messageHash into this format "\x19Ethereum Signed Message:\n" + len(message) + message). It is important to abide by the standard for interoperability.

After doing so you call recover method in which you pass the signature which is nothing but your Ethereum Signed Message signed with the sender's private key, you compare it with the Ethereum Signed Message - signedMessageHash you generated to recover the public key which should be the address of the sender.

If the signer address is the same as the sender address that was passed in, then we transfer the ERC-20 tokens from sender to recipient.

Hardhat Test
Let's create a Hardhat test to demonstrate how this would work. Create a new file under the test directory named metatxn-test.js with the following code

const { expect } = require("chai");
const { BigNumber } = require("ethers");
const { arrayify, parseEther } = require("ethers/lib/utils");
const { ethers } = require("hardhat");

describe("MetaTokenTransfer", function () {
  it("Should let user transfer tokens through a relayer", async function () {
    // Deploy the contracts
    const RandomTokenFactory = await ethers.getContractFactory("RandomToken");
    const randomTokenContract = await RandomTokenFactory.deploy();
    await randomTokenContract.deployed();

    const MetaTokenSenderFactory = await ethers.getContractFactory(
      "TokenSender"
    );
    const tokenSenderContract = await MetaTokenSenderFactory.deploy();
    await tokenSenderContract.deployed();

    // Get three addresses, treat one as the user address
    // one as the relayer address, and one as a recipient address
    const [_, userAddress, relayerAddress, recipientAddress] =
      await ethers.getSigners();

    // Mint 10,000 tokens to user address (for testing)
    const tenThousandTokensWithDecimals = parseEther("10000");
    const userTokenContractInstance = randomTokenContract.connect(userAddress);
    const mintTxn = await userTokenContractInstance.freeMint(
      tenThousandTokensWithDecimals
    );
    await mintTxn.wait();

    // Have user infinite approve the token sender contract for transferring 'RandomToken'
    const approveTxn = await userTokenContractInstance.approve(
      tokenSenderContract.address,
      BigNumber.from(
        // This is uint256's max value (2^256 - 1) in hex
        // Fun Fact: There are 64 f's in here.
        // In hexadecimal, each digit can represent 4 bits
        // f is the largest digit in hexadecimal (1111 in binary)
        // 4 + 4 = 8 i.e. two hex digits = 1 byte
        // 64 digits = 32 bytes
        // 32 bytes = 256 bits = uint256
        "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
      )
    );
    await approveTxn.wait();

    // Have user sign message to transfer 10 tokens to recipient
    const transferAmountOfTokens = parseEther("10");
    const messageHash = await tokenSenderContract.getHash(
      userAddress.address,
      transferAmountOfTokens,
      recipientAddress.address,
      randomTokenContract.address
    );
    const signature = await userAddress.signMessage(arrayify(messageHash));

    // Have the relayer execute the transaction on behalf of the user
    const relayerSenderContractInstance =
      tokenSenderContract.connect(relayerAddress);
    const metaTxn = await relayerSenderContractInstance.transfer(
      userAddress.address,
      transferAmountOfTokens,
      recipientAddress.address,
      randomTokenContract.address,
      signature
    );
    await metaTxn.wait();

    // Check the user's balance decreased, and recipient got 10 tokens
    const userBalance = await randomTokenContract.balanceOf(
      userAddress.address
    );
    const recipientBalance = await randomTokenContract.balanceOf(
      recipientAddress.address
    );

    expect(userBalance.lt(tenThousandTokensWithDecimals)).to.be.true;
    expect(recipientBalance.gt(BigNumber.from(0))).to.be.true;
  });
});
🤔 What is uint256's max value in hexadecimal?


0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff

0x9999999999999999999999999999999999999999999999999999999999999999

0x1111111111111111111111111111111111111111111111111111111111111111
The code is explained in the comments, but let's try running the test. You can type npx hardhat test in your terminal to run the tests, and if you did everything correctly, it should pass!

That means, after the initial approval, the sender was able to transfer 10 tokens to the recipient without needing to pay gas themselves. You can extend this test further easily to see that this would even work for multiple transfers, as long as you don't exceed the user's balance of 10,000 tokens.

Security Vulnerability
Can you guess what the problem is with the code we just wrote though? 🤔

Since the signature contains the information necessary, the relayer could keep sending the signature to the contract over and over, thereby continuously transferring tokens out of the sender's account into the recipient's account.

While this may not seem like a big deal in this specific example, what if this contract was responsible for dealing with money on mainnet? If the same signature could be reused over and over, the user would lose all their tokens!

Instead, the transaction should only be executed when the user explicitly provides a second signature (while staying within the rules of the smart contract, of course).

This attack is called Signature Replay - because, well you guessed it, you're replaying a signature.

🤔 Why is signature replay a problem?


Signature replay can make it seem like the user gave permission for something they did not

Signature replay allows the relayer to pretend like they are the user and make transactions on their behalf

It's not a very good cologne
Solving for Signature Replay
For even simpler contracts, you could resolve this by having some (nested) mappings in your contract. But there are 4 variables to keep track of here per transfer - sender, amount, recipient, and tokenContract. Creating a nested mapping this deep can be quite expensive in Solidity.

Also, that would be different for each 'kind' of a smart contract - as you're not always dealing with the same use case. A more general-purpose solution for this is to create a single mapping from the hash of the parameters to a boolean value, where true indicates that this meta-transaction has already been executed, and false indicates it hasn't.

Something like mapping(bytes32 => bool).

This also has a problem though. With the current set of parameters, if Alice sent 10 tokens to Bob, it would go through the first time, and the mapping would be updated to reflect that. However, what if Alice genuinely wants to send 10 more tokens to Bob a second time?

Since digital signatures are deterministic, i.e. the same input will give the same output for the same set of keys, that means Alice would never be able to send Bob 10 tokens again!

To avoid this, we introduce a fifth parameter, the nonce.

The nonce is just a random number value, and can be selected by the user, the contract, be randomly generated, it doesn't matter - as long as the user's signature includes that nonce. Since the exact same transaction but with a different nonce would produce a different signature, the above problem is solved!

Let's implement this 🚀

Smart Contract Changes
We need to update the smart contract's code in three places.

First, we will add a mapping(bytes32 => bool) to keep track of which signatures have already been executed.

Second, we will update our helper function getHash to take in a nonce parameter and include that in the hash.

Third, we will update our transfer function to also take in the nonce and pass it onto getHash when verifying the signature. It will also update the mapping after the signature is verified.

Here's the updated code:

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

contract RandomToken is ERC20 {
    constructor() ERC20("", "") {}

    function freeMint(uint amount) public {
        _mint(msg.sender, amount);
    }
}

contract TokenSender {

    using ECDSA for bytes32;

    // New mapping
    mapping(bytes32 => bool) executed;

    // Add the nonce parameter here
    function transfer(address sender, uint amount, address recipient, address tokenContract, uint nonce, bytes memory signature) public {
        // Pass ahead the nonce
        bytes32 messageHash = getHash(sender, amount, recipient, tokenContract, nonce);
        bytes32 signedMessageHash = messageHash.toEthSignedMessageHash();

        // Require that this signature hasn't already been executed
        require(!executed[signedMessageHash], "Already executed!");

        address signer = signedMessageHash.recover(signature);

        require(signer == sender, "Signature does not come from sender");

        // Mark this signature as having been executed now
        executed[signedMessageHash] = true;
        bool sent = ERC20(tokenContract).transferFrom(sender, recipient, amount);
        require(sent, "Transfer failed");
    }

    // Add the nonce parameter here
    function getHash(address sender, uint amount, address recipient, address tokenContract, uint nonce) public pure returns (bytes32) {
        return keccak256(abi.encodePacked(sender, amount, recipient, tokenContract, nonce));
    }
}
Let's update our tests as well to reflect this. We will have two tests - one which proves that signatures cannot be replayed anymore, and one which proves that two distinct signatures with different nonces can still work, even if everything else is the same.

Here are the updated tests

const { expect } = require("chai");
const { BigNumber } = require("ethers");
const { arrayify, parseEther } = require("ethers/lib/utils");
const { ethers } = require("hardhat");

describe("MetaTokenTransfer", function () {
  it("Should let user transfer tokens through a relayer with different nonces", async function () {
    // Deploy the contracts
    const RandomTokenFactory = await ethers.getContractFactory("RandomToken");
    const randomTokenContract = await RandomTokenFactory.deploy();
    await randomTokenContract.deployed();

    const MetaTokenSenderFactory = await ethers.getContractFactory(
      "TokenSender"
    );
    const tokenSenderContract = await MetaTokenSenderFactory.deploy();
    await tokenSenderContract.deployed();

    // Get three addresses, treat one as the user address
    // one as the relayer address, and one as a recipient address
    const [_, userAddress, relayerAddress, recipientAddress] =
      await ethers.getSigners();

    // Mint 10,000 tokens to user address (for testing)
    const tenThousandTokensWithDecimals = parseEther("10000");
    const userTokenContractInstance = randomTokenContract.connect(userAddress);
    const mintTxn = await userTokenContractInstance.freeMint(
      tenThousandTokensWithDecimals
    );
    await mintTxn.wait();

    // Have user infinite approve the token sender contract for transferring 'RandomToken'
    const approveTxn = await userTokenContractInstance.approve(
      tokenSenderContract.address,
      BigNumber.from(
        // This is uint256's max value (2^256 - 1) in hex
        "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
      )
    );
    await approveTxn.wait();

    // Have user sign message to transfer 10 tokens to recipient
    let nonce = 1;

    const transferAmountOfTokens = parseEther("10");
    const messageHash = await tokenSenderContract.getHash(
      userAddress.address,
      transferAmountOfTokens,
      recipientAddress.address,
      randomTokenContract.address,
      nonce
    );
    const signature = await userAddress.signMessage(arrayify(messageHash));

    // Have the relayer execute the transaction on behalf of the user
    const relayerSenderContractInstance =
      tokenSenderContract.connect(relayerAddress);
    const metaTxn = await relayerSenderContractInstance.transfer(
      userAddress.address,
      transferAmountOfTokens,
      recipientAddress.address,
      randomTokenContract.address,
      nonce,
      signature
    );
    await metaTxn.wait();

    // Check the user's balance decreased, and recipient got 10 tokens
    let userBalance = await randomTokenContract.balanceOf(userAddress.address);
    let recipientBalance = await randomTokenContract.balanceOf(
      recipientAddress.address
    );

    expect(userBalance.eq(parseEther("9990"))).to.be.true;
    expect(recipientBalance.eq(parseEther("10"))).to.be.true;

    // Increment the nonce
    nonce++;

    // Have user sign a second message, with a different nonce, to transfer 10 more tokens
    const messageHash2 = await tokenSenderContract.getHash(
      userAddress.address,
      transferAmountOfTokens,
      recipientAddress.address,
      randomTokenContract.address,
      nonce
    );
    const signature2 = await userAddress.signMessage(arrayify(messageHash2));
    // Have the relayer execute the transaction on behalf of the user
    const metaTxn2 = await relayerSenderContractInstance.transfer(
      userAddress.address,
      transferAmountOfTokens,
      recipientAddress.address,
      randomTokenContract.address,
      nonce,
      signature2
    );
    await metaTxn2.wait();

    // Check the user's balance decreased, and recipient got 10 tokens
    userBalance = await randomTokenContract.balanceOf(userAddress.address);
    recipientBalance = await randomTokenContract.balanceOf(
      recipientAddress.address
    );

    expect(userBalance.eq(parseEther("9980"))).to.be.true;
    expect(recipientBalance.eq(parseEther("20"))).to.be.true;
  });

  it("Should not let signature replay happen", async function () {
    // Deploy the contracts
    const RandomTokenFactory = await ethers.getContractFactory("RandomToken");
    const randomTokenContract = await RandomTokenFactory.deploy();
    await randomTokenContract.deployed();

    const MetaTokenSenderFactory = await ethers.getContractFactory(
      "TokenSender"
    );
    const tokenSenderContract = await MetaTokenSenderFactory.deploy();
    await tokenSenderContract.deployed();

    // Get three addresses, treat one as the user address
    // one as the relayer address, and one as a recipient address
    const [_, userAddress, relayerAddress, recipientAddress] =
      await ethers.getSigners();

    // Mint 10,000 tokens to user address (for testing)
    const tenThousandTokensWithDecimals = parseEther("10000");
    const userTokenContractInstance = randomTokenContract.connect(userAddress);
    const mintTxn = await userTokenContractInstance.freeMint(
      tenThousandTokensWithDecimals
    );
    await mintTxn.wait();

    // Have user infinite approve the token sender contract for transferring 'RandomToken'
    const approveTxn = await userTokenContractInstance.approve(
      tokenSenderContract.address,
      BigNumber.from(
        // This is uint256's max value (2^256 - 1) in hex
        "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
      )
    );
    await approveTxn.wait();

    // Have user sign message to transfer 10 tokens to recipient
    let nonce = 1;

    const transferAmountOfTokens = parseEther("10");
    const messageHash = await tokenSenderContract.getHash(
      userAddress.address,
      transferAmountOfTokens,
      recipientAddress.address,
      randomTokenContract.address,
      nonce
    );
    const signature = await userAddress.signMessage(arrayify(messageHash));

    // Have the relayer execute the transaction on behalf of the user
    const relayerSenderContractInstance =
      tokenSenderContract.connect(relayerAddress);
    const metaTxn = await relayerSenderContractInstance.transfer(
      userAddress.address,
      transferAmountOfTokens,
      recipientAddress.address,
      randomTokenContract.address,
      nonce,
      signature
    );
    await metaTxn.wait();

    // Have the relayer attempt to execute the same transaction again with the same signature
    // This time, we expect the transaction to be reverted because the signature has already been used.
    expect(relayerSenderContractInstance.transfer(
      userAddress.address,
      transferAmountOfTokens,
      recipientAddress.address,
      randomTokenContract.address,
      nonce,
      signature
    )).to.be.revertedWith('Already executed!');
  });
});
The first test here has the user sign two distinct signatures with two distinct nonces, and the relayer executes them both. In the second test, however, the relayer attempts to execute the same signature twice. The second time the relayer tries to use the same signature, we expect the transaction to revert.

If you run npx hardhat test here and all tests succeed, that means that the second transaction with the replay attack was reverted.

This shows that signature replay can no longer happen, and the vulnerability is secured! 🥳🥳

🤔 Is this contract vulnerable to Signature Replay?

Is this contract vulnerable to Signature Replay?

Yes

No
Conclusion
This turned out to be a longer level than I was expecting, but I hope you learned something cool! Meta transactions are a great utility tool to have in your belt, but make sure you implement them properly.

As always, feel free to ask on Discord if you have any questions or need help, we will be more than happy to help you!

Ciao, WAGMI! 🚀🚀🚀

Submit Quiz
0 / 7

Lesson Type: Quiz
Estimated Time: 30-60 minutes
Current Score: 100%
Gas Optimizations in Solidity
In this tutorial, we will learn about some of the gas optimization techniques in Solidity. This has been one of our most-requested levels, so let's get started without further ado 👀

Tips and Tricks
Variable Packing
If you remember we talked about storage slots in one of our previous levels. Now the interesting point in solidity if you remember is that each storage slot is 32 bytes.

This storage can be optimized which will further mean gas optimization when you deploy your smart contract if you pack your variables correctly.

Packing your variables means that you pack or put together variables of smaller size so that they collectively form 32 bytes. For example, you can pack 32 uint8 into one storage slot but for that to happen it is important that you declare them consecutively because the order of declaration of variables matters in solidity.

Given two code samples:

uint8 num1;
uint256 num2;
uint8 num3;
uint8 num4;
uint8 num5;
uint8 num1;
uint8 num3;
uint8 num4;
uint8 num5;
uint256 num2;
The second one is better because in the second one solidity compiler will put all the uint8's in one storage slot but in the first case it will put uint8 num1 in one slot but now the next one it will see is a uint256 which is in itself requires 32 bytes cause 256/8 bits = 32 bytes so it cant be put in the same storage slot as uint8 num1 so now it will require another storage slot. After that uint8 num3, num4, num5 will be put in another storage slot. Thus the second example requires 2 storage slots as compared to the first example which requires 3 storage slots.

It's also important to note that elements in memory and calldata cannot be packed and are not optimized by solidity's compiler.

🤔 The Solidity compiler can pack variables regardless of their order?


True

False
Storage vs Memory
Changing storage variables requires more gas than variables in memory. It's better to update storage variables at the end after all the logic has lready been implemented.

So given two samples of code

contract A {
    uint public counter = 0;
    
    function count() {
        for(uint i = 0; i < 10; i++) {
            counter++;
        }
    }
    
}
contract B {
    uint public counter = 0;
    
    function count() {
        uint copyCounter;
        for(uint i = 0; i < 10; i++) {
            copyCounter++;
        }
        counter = copyCounter;
    }
    
}
The second sample of code is more gas optimized because we are only writing to the storage variable counter only once as compared to the first sample where we were writing to storage in every iteration. Even though we are performing one extra write overall in the second code sample, the 10 writes to memory and 1 write to storage is still cheaper than 10 writes directly to storage.

🤔 Should you write to storage variables in a loop, or create a local copy and update the storage variable at the end even if it means having a higher number of total read/writes?


Write to storage because having higher number of writes overall is worse for gas

Create a local copy in memory because the cost of additional write is still lower than writing to storage all the time
Fixed length and Variable-length variables
We talked about how fixed length and variable length variables are stored. Fixed-length variables are stored in a stack whereas variable-length variables are stored in a heap.

Essentially why this happens is because in a stack you exactly know where to find a variable and its length whereas in a heap there is an extra cost of traversing given the variable nature of the variable

So if you can make your variables fixed size, it's always good for gas optimizations

Given two examples of code:

string public text = "Hello";
uint[] public arr;
bytes32 public text = "Hello";
uint[2] public arr;
The second example is more gas optimized because all the variables are of fixed length.

External, Internal, and Public functions
Calling functions in solidity can be very gas-intensive, its better you call one function and extract all data from it than call multiple functions

Recall the public functions are those which can be called both externally (by users and other smart contracts) and internally (from another function in the same contract).

However, when your contract is creating functions that will only be called externally it means the contract itself cant call these functions, it's better you use the external keyword instead of public because all the input variables in public functions are copied to memory which costs gas whereas for external functions input variables are stored in calldata which is a special data location used to store function arguments and it requires less gas to store in calldata than in memory

The same principle applies as to why it's cheaper to call internal functions rather than public functions. This is because when you call internal functions the arguments are passed as references of the variables and are not again copied into memory but that doesn't happen in the case of public functions.

🤔 If your function is only ever going to be called by users and other smart contracts, what is the most gas efficient visibility modifier?


public

external

private
Function modifiers
This is a fascinating one because a few weeks ago, I was debugging this error from one of our students and they were experiencing the error “Stack too deep”. This usually happens when you declare a lot of variables in your function and the available stack space for that function is no longer available. As we saw in the Ethereum Storage level, the EVM only allows upto 16 variables within a single function as that it cannot perform operations beyond 16 levels of depth in the stack.

Now even after moving a lot of the require statements in the modifier it wasn't helping because function modifiers use the same stack as the function on which they are put. To solve this issue we used an internal function inside the modifier because internal functions don't share the same restricted stack as the original function but modifier does.

🤔 Function modifiers use the same stack as the function they are attached to?


True

False
🤔 What is the maximum number of local variables you can define within a single function?


16

32

256
Use libraries
Libraries are stateless contracts that don't store any state. Now when you call a public function of a library from your contract, the bytecode of that function doesn't get deployed with your contract, and thus you can save some gas costs. For example, if you contract has functions to sort or to do maths etc. You can put them in a library and then call these library functions to do the maths or sorting for your contract. To read more about libraries follow this link.

There is a small caveat however. If you are writing your own libraries, you will need to deploy them and pay gas - but once deployed, it can be reused by other smart contracts without deploying it themselves. Since they don't store any state, libraries only need to be deployed once to the blockchain and are assigned an address that the Solidity compiler is smart enough to figure out itself. Therefore, if you use libraries from OpenZeppelin for example, they will not add to your deployment cost.

Short Circuiting Conditionals
If you are using (||) or (&&) it's better to write your conditions in such a way so that the least functions/variable values are executed or retrieved in order to determine if the entire statement is true or false.

Since conditional checks will stop the second they find the first value which satisfies the condition, you should put the variables most likely to validate/invalidate the condition first. In OR conditions (||), try to put the variable with the highest likelihood of being true first, and in AND conditions (&&), try to put the variable with the highest likelihood of being false first. As soon as that variable is checked, the conditional can exit without needing to check the other values, thereby saving gas.

🤔 If `isContract` returns true when an address is a smart contract, and false otherwise - is this the best way to write the conditional for a contract that is often called by external contracts?

If `isContract` returns true when an address is a smart contract, and false otherwise - is this the best way to write the conditional for a contract that is often called by external contracts?

Yes

it doesn't matter

No

isContract should come first
Free up Storage
Since storage space costs gas, you can actually free up storage and delete unnecessary data to get gas refunds. So if you no longer need some state values, use the delete keyword in Solidity for some gas refunds.

Short Error Strings
Make sure that the error strings in your require statements are of very short length, the more the length of the string, the more gas it will cost.

require(counter >= 100, "NOT REACHED"); // Good
require(balance >= amount, "Counter is still to reach the value greater than or equal to 100, ............................................";
The first requirement is more gas optimized than the second one.

NOTE: In newer versions of Solidity, there are now custom errors using the error keyword which behave very similar to events and can achieve similar gas optimizations.

Thank you all for staying tuned to this article 🚀 Hope you liked it :)

🤔 Which one of the following is NOT a reason to gas optimize your code?


Allows you to write more complex functions

Makes it cheaper for users to interact with your dApp

Ethereum provides incentives for developers who write optimized code
References
Mudit Gupta - Gas Optimizations Tips and Gas Optimizations Tips Part - 2
Submit Quiz
0 / 7

Lesson Type: Quiz
Estimated Time: 2-3 hours
Current Score: 100%
Upgradeable Contracts and Patterns
We know that smart contracts on Ethereum are not upgradeable, as the code is immutable and cannot be changed once it is deployed. But writing perfect code the first time around is hard, and as humans we are all prone to making mistakes. Sometimes even contracts which have been audited turn out to have bugs that cost them millions.

🤔 You cannot upgrade a smart contract's code itself


True

False
In this level, we will learn about some design patterns that can be used in Solidity to write upgradeable smart contracts.

How does it work?
To upgrade our contracts we use something called the Proxy Pattern. The word Proxy might sound very familiar to you because it is not a web3-native word.

Image

Essentially how this pattern works is that a contract is split into two contracts - Proxy Contract and the Implementation contract.

The Proxy Contract is responsible for managing the state of the contract which involves persistent storage whereas Implementation Contract is responsible for executing the logic and doesn't store any persistent state. User calls the Proxy Contract which further does a delegatecall to the Implementation Contract so that it can implement the logic. Remember we studied delegatecall in one of our previous levels 👀

Image

🤔 Why do we use delegatecall in proxy pattern, and not regular call?


It is cheaper for gas and more efficient

It allows the state to live in one contract and persist across upgrades made to the implementation contract
🤔 The proxy pattern splits your contract into what?


A smart contract and a legal contract

A state contract and a code contract

A Proxy Contract and an Implementation Contract
This pattern becomes interesting when Implementation Contract can be replaced which means the logic which is executed can be replaced by another version of the Implementation Contract without affecting the state of the contract which is stored in the proxy.

🤔 What design pattern can be used to write upgradeable smart contracts?


Proxy Patterns

Composition Patterns

Singleton Patterns
There are mainly three ways in which we can replace/upgrade the Implementation Contract:

Diamond Implementation
Transparent Implementation
UUPS Implementation
We will however only focus on Transparent and UUPS because they are the most commonly used ones.

To upgrade the Implementation Contract you will have to use some method like upgradeTo(address) which will essentially change the address of the Implementation Contract from the old one to the new one.

🤔 What makes the Proxy Pattern upgradeable?


They are a special type of contract in Ethereum that lets you change the code

They forward all function calls to an implementation contract and the address of the implementation contract can be updated

They communicate with web2 services for processing functions which are upgradeable
But the important part lies in where should we keep the upgradeTo(address) function, we have two choices that are either keep it in the Proxy Contract which is essentially how Transparent Proxy Pattern works, or keep it in the Implementation Contract which is how the UUPS contract works.

Image

Another important thing to note about this Proxy Pattern is that the constructor of the Implementation Contract is never executed.

When deploying a new smart contract, the code inside the constructor is not a part of the contract's runtime bytecode because it is only needed during the deployment phase and runs only once. Now because when Implementation Contract was deployed it was initially not connected to the Proxy Contract as a reason any state change that would have happened in the constructor is now not there in the Proxy Contract which is used to maintain the overall state.

As a reason Proxy Contracts are unaware of the existence of constructors. Therefore, instead of having a constructor, we use something called an initializer function which is called by the Proxy Contract once the Implementation Contract is connected to it. This function does exactly what a constructor is supposed to do but is now included in the runtime bytecode as it behaves like a regular function and is callable by the Proxy Contract.

🤔 Implementation contracts should contain constructors?


True

False
Using OpenZeppelin contracts, you can use their Initialize.sol contract which makes sure that your initialize function is executed only once just like a contructor

// contracts/MyContract.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

contract MyContract is Initializable {
    function initialize(
        address arg1,
        uint256 arg2,
        bytes memory arg3
    ) public payable initializer {
        // "constructor" code...
    }
}
Above given code is from Openzeppelin's documentation and provides an example of how the initializer modifier ensures that the initialize function can only be called once. This modifier comes from the Initializable Contract

🤔 Why do implementation contracts contain initializers?


It's another Solidity keyword like constructor which is only used for upgradeable contracts

It allows the code to be included in the bytecode so the Proxy contract can delegatecall the initializer
We will now study Proxy patterns in detail 🚀 👀

Transparent Proxy Pattern
The Transparent Proxy Pattern is a simple way to separate responsibilities between Proxy and Implementation contracts. In this case, the upgradeTo function is part of the Proxy contract, and the Implementation can be upgraded by calling upgradeTo on the proxy thereby changing where future function calls are delegated to.

There are some caveats though. There might be a case where the Proxy Contract and Implementation Contract have a function with the same name and arguments. Imagine if Proxy Contract has a owner() function and so does Implementation Contract. In Transparent Proxy contracts, this problem is dealt by the Proxy contract which decides whether a call from the user will execute within the Proxy contract itself or the Implementation Contract based on the msg.sender global variable

So if the msg.sender is the admin of the proxy then the proxy will not delegate the call and will try to execute the call if it understands it. If it's not the admin address, the proxy will delegate the call to the Implementation Contract even if the matches one of the proxy's functions.

Issues with Transparent Proxy Pattern
As we know that the address of the owner will have to be stored in the storage and using storage is one of the most inefficient and costly steps in interacting with a smart contract every time the user calls the proxy, the proxy checks whether the user is the admin or not which adds unnecessary gas costs to majority of the transactions taking place.

UUPS Proxy Pattern
The UUPS Proxy Pattern is another way to separate responsibilities between Proxy and Implementation contracts. In this case, the upgradeTo function is also part of the Implementation contract, and is called using a delegatecall through the Proxy by the owner.

In UUPS whether its the admin or the user, all the calls are sent to the Implementation Contract The advantage of this is that every time a call is made we will not have to access the storage to check if the user who started the call is an admin or not which improved efficiency and costs. Also because its the Implementation Contract you can customize the function according to your need by adding things like Timelock, Access Control etc with every new Implementation that comes up which couldn't have been done in the Transparent Proxy Pattern

Issues with UUPS Proxy Pattern
The issue with this is now because the upgradeTo function exists on the side of the Implementation contract developer has to worry about the implementation of this function which may sometimes be complicated and because more code has been added, it increases the possibility of attacks. This function also needs to be in all the versions of Implementation Contract which are upgraded which introduces a risk if maybe the developer forgets to add this function and then the contract can no longer be upgraded.

BUIDL
Lets build an example where you can experience how to build an upgradeable contract. We will be using the UUPS upgradeability pattern through this example, though you can build one with the Transparent Proxy Pattern as well!

To setup a Hardhat project, Open up a terminal and execute these commands

npm init --yes
npm install --save-dev hardhat
If you are on Windows, please do this extra step and install these libraries as well :)

npm install --save-dev @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers
In the same directory where you installed Hardhat run:

npx hardhat
Select Create a basic sample project
Press enter for the already specified Hardhat Project root
Press enter for the question on if you want to add a .gitignore
Press enter for Do you want to install this sample project's dependencies with npm (@nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers)?
Now you have a hardhat project ready to go!

We will be using libraries from openzeppelin which support upgradeable contracts. To install those libraries, in the same folder execute the following command:
npm i @openzeppelin/contracts-upgradeable @openzeppelin/hardhat-upgrades @nomiclabs/hardhat-etherscan --save-dev
Replace the code in your hardhat.config.js with the following code to be able to use these libraries:

require("@nomiclabs/hardhat-ethers");
require("@openzeppelin/hardhat-upgrades");
require("@nomiclabs/hardhat-etherscan");

module.exports = {
  solidity: "0.8.4",
};
Start by creating a new file inside the contracts directory called LW3NFT.sol and add the following lines of code to it

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/UUPSUpgradeable.sol";
import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";


contract LW3NFT is Initializable, ERC721Upgradeable, UUPSUpgradeable, OwnableUpgradeable   {
    // Note how we created an initialize function and then added the
    // initializer modifier which ensure that the
    // initialize function is only called once
    function initialize() public initializer  {
        // Note how instead of using the ERC721() constructor, we have to manually initialize it
        // Same goes for the Ownable contract where we have to manually initialize it
        __ERC721_init("LW3NFT", "LW3NFT");
        __Ownable_init();
        _mint(msg.sender, 1);
    }
    function _authorizeUpgrade(address newImplementation) internal override onlyOwner {

    }
}
Lets try to understand what's happening in this contract in a bit more detail

If you look at all the contracts which LW3NFT is importing, you will realize why they are important. First being the Initializable contract from Openzeppelin which provides us with the initializer modifier which ensures that the initialize function is only called once. The initialize function is needed because we cant have a contructor in the Implementation Contract which in this case is the LW3NFT contract

It imports ERC721Upgradeable and OwnableUpgradeable because the original ERC721 and Ownable contracts have a constructor which cant be used with proxy contracts.

🤔 It is possible to use `ERC721.sol` from OpenZeppelin within an upgradeable contract?


True

False
Lastly we have the UUPSUpgradeable Contract which provides us with the upgradeTo(address) function which has to be put on the Implementation Contract in case of a UUPS proxy pattern.

After the declaration of the contract, we have the initialize function with the initializer modifier which we get from the Initializable contract. The initializer modifier ensures the initialize function can only be called once. Also note that the new way in which we are initializing ERC721 and Ownable contract. This is the standard way of initializing upgradeable contracts and you can look at the function here. After that we just mint using the usual mint function.

function initialize() public initializer  {
    __ERC721_init("LW3NFT", "LW3NFT");
    __Ownable_init();
    _mint(msg.sender, 1);
}
Another interesting function which we dont see in the normal ERC721 contract is the _authorizeUpgrade which is a function which needs to be implemented by the developer when they import the UUPSUpgradeable Contract from Openzeppelin, it can be found here. Now why this function has to be overwritten is intresting because it gives us the ability to add authorization on who can actually upgrade the given contract, it can be changed according to requirements but in our case we just added an onlyOwner modifier.

function _authorizeUpgrade(address newImplementation) internal override onlyOwner {

    }
Now lets create another new file inside the contracts directory called LW3NFT2.sol which will be the upgraded version of LW3NFT.sol

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

import "./LW3NFT.sol";

contract LW3NFT2 is LW3NFT {

    function test() pure public returns(string memory) {
        return "upgraded";
    }
}
This smart contract is much easier because it is just inheriting LW3NFT contract and then adding a new function called test which just returns a string upgraded.

Pretty easy right? 🤯

Wow 🙌 , okay we are done with writing the Implementation Contract, do we now need to write the Proxy Contract as well?

Good news is nope, we dont need to write the Proxy Contract because Openzeppelin deploys and connects a Proxy Contract automatically when we use there library to deploy the Implementation Contract.

So lets try to do that, In your test directory create a new file named proxy-test.js and lets have some fun with code

const { expect } = require("chai");
const { ethers } = require("hardhat");
const hre = require("hardhat");

describe("ERC721 Upgradeable", function () {
  it("Should deploy an upgradeable ERC721 Contract", async function () {
    const LW3NFT = await ethers.getContractFactory("LW3NFT");
    const LW3NFT2 = await ethers.getContractFactory("LW3NFT2");

    let proxyContract = await hre.upgrades.deployProxy(LW3NFT, {
      kind: "uups",
    });
    const [owner] = await ethers.getSigners();
    const ownerOfToken1 = await proxyContract.ownerOf(1);

    expect(ownerOfToken1).to.equal(owner.address);

    proxyContract = await hre.upgrades.upgradeProxy(proxyContract, LW3NFT2);
    expect(await proxyContract.test()).to.equal("upgraded");
  });
});
Lets see whats happening here, We first get the LW3NFT and LW3NFT2 instance using the getContractFactory function which is common to all the levels we have been teaching till now. After that the most important line comes in which is:

let proxyContract = await hre.upgrades.deployProxy(LW3NFT, {
  kind: "uups",
});
This function comes from the @openzeppelin/hardhat-upgrades library that you installed, It essentially uses the upgrades class to call the deployProxy function and specifies the kind as uups. When the function is called it deploys the Proxy Contract, LW3NFT Contract and connects them both. More info about this can be found here.

Note that the initialize function can be named anything else, its just that deployProxy by default calls the function with name initialize for the initializer but you can modify it by changing the defaults 😇

After deploying, we test that the contract actually gets deployed by calling the ownerOf function for Token ID 1 and checking if the NFT was indeed minted.

Now the next part comes in where we want to deploy LW3NFT2 which is the upgraded contract for LW3NFT.

For that we execute the upgradeProxy method again from the @openzeppelin/hardhat-upgrades library which upgrades and replaces LW3NFT with LW3NFT2 without changing the state of the system

proxyContract = await hre.upgrades.upgradeProxy(proxyContract, LW3NFT2);
To test if it was actually replaced we call the test() function, and ensured that it returned "upgraded" even though that function wasn't present in the original LW3NFT contract.

Here you go, you learn how to upgrade a smart contract today.

LFG 🚀

Readings
Timelock was mentioned in the given article, to learn more about it you can read the following article
Access Control was also mentioned and you can read about it more here
References
OpenZeppelin Docs
OpenZeppelin Youtube
🤔 Which of the following is NOT a type of Proxy Pattern?


Hexagon Pattern

Transparent Proxy Pattern

Diamond Pattern

UUPS Pattern
Submit Quiz
0 / 9

Lesson Type: Quiz
Estimated Time: 1-2 hours
Current Score: 100%
Malicious External Contracts
In crypto world, you will often hear about how contracts which looked legitimate were the reason behind a big scam. How are hackers able to execute malicious code from a legitimate looking contract?

We will learn one method today 👀

What will happen?
There will be three contracts - Attack.sol, Helper.sol and Good.sol. User will be able to enter an eligibility list using Good.sol which will further call Helper.sol to keep track of all the users which are eligible.

Attack.sol will be designed in such a way that eligibility list can be manipulated, lets see how 👀

Build
Lets build an example where you can experience how the attack happens.

To setup a Hardhat project, Open up a terminal and execute these commands

npm init --yes
npm install --save-dev hardhat
If you are on Windows, please do this extra step and install these libraries as well :)

npm install --save-dev @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers
In the same directory where you installed Hardhat run:

npx hardhat
Select Create a basic sample project
Press enter for the already specified Hardhat Project root
Press enter for the question on if you want to add a .gitignore
Press enter for Do you want to install this sample project's dependencies with npm (@nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers)?
Now you have a Hardhat project ready to go!

Start by creating a new file inside the contracts directory called Good.sol

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
import "./Helper.sol";

contract Good {
    Helper helper;
    constructor(address _helper) payable {
        helper = Helper(_helper);
    }

    function isUserEligible() public view returns(bool) {
        return helper.isUserEligible(msg.sender);
    }

    function addUserToList() public  {
        helper.setUserEligible(msg.sender);
    }

    fallback() external {}
    
}
After creating Good.sol, create a new file inside the contracts directory named Helper.sol

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract Helper {
    mapping(address => bool) userEligible;

    function isUserEligible(address user) public view returns(bool) {
        return userEligible[user];
    }

    function setUserEligible(address user) public {
        userEligible[user] = true;
    }

    fallback() external {}
}
The last contract that we will create inside the contracts directory is Attack.sol

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract Attack {
    address owner;
    mapping(address => bool) userEligible;

    constructor() {
        owner = msg.sender;
    }

    function isUserEligible(address user) public view returns(bool) {
        if(user == owner) {
            return true;
        }
        return false;
    }

    function setUserEligible(address user) public {
        userEligible[user] = true;
    }
    
    fallback() external {}
}
You will notice that the fact about Attack.sol is that it will generate the same ABI as Helper.sol eventhough it has different code within it. This is because ABI only contains function definitions for public variables, functions and events. So Attack.sol can be typecasted as Helper.sol.

Now because Attack.sol can be typecasted as Helper.sol, a malicious owner can deploy Good.sol with the address of Attack.sol instead of Helper.sol and users will believe that he is indeed using Helper.sol to create the eligibility list.

In our case, the scam will happen as follows. The scammer will first deploy Good.sol with the address of Attack.sol. Then when the user will enter the eligibility list using addUserToList function which will work fine because the code for this function is same within Helper.sol and Attack.sol.

The true colours will be observed when the user will try to call isUserEligible with his address because now this function will always return false because it calls Attack.sol's isUserEligible function which always returns false except when its the owner itself, which was not supposed to happen.

Lets try to write a test and see if this scam actually works, create a new file inside the test folder named attack.js

const { expect } = require("chai");
const { BigNumber } = require("ethers");
const { ethers, waffle } = require("hardhat");

describe("Attack", function () {
  it("Should change the owner of the Good contract", async function () {
    // Deploy the Attack contract
    const attackContract = await ethers.getContractFactory("Attack");
    const _attackContract = await attackContract.deploy();
    await _attackContract.deployed();
    console.log("Attack Contract's Address", _attackContract.address);

    // Deploy the good contract
    const goodContract = await ethers.getContractFactory("Good");
    const _goodContract = await goodContract.deploy(_attackContract.address, {
      value: ethers.utils.parseEther("3"),
    });
    await _goodContract.deployed();
    console.log("Good Contract's Address:", _goodContract.address);

    const [_, addr1] = await ethers.getSigners();
    // Now lets add an address to the eligibility list
    let tx = await _goodContract.connect(addr1).addUserToList();
    await tx.wait();

    // check if the user is eligible
    const eligible = await _goodContract.connect(addr1).isUserEligible();
    expect(eligible).to.equal(false);
  });
});
To run this test, open up your terminal pointing to the root of the directory for this level and execute this command:

npx hardhat test
If all your tests passed, this means that the scam was successful and that the user will never be determined eligible

Prevention
Make the address of the external contract public and also get your external contract verified so that all users can view the code

Create a new contract, instead of typecasting an address into a contract inside the constructor. So instead of doing Helper(_helper) where you are typecasting _helper address into a contract which may or may not be the Helper contract, create an explicit new helper contract instance using new Helper().

Example

contract Good {
    Helper public helper;
    constructor() {
        helper = new Helper();
    }
🤔 How do you make sure that your contract is trusted by your users?


Get your contract and external contract verified on etherscan

Make sure your contract's code is not verified on etherscan

Make sure your contract's code is verified but the external contract you are calling from your contract is not verified on etherscan
🤔 How can a malicious actor prevent users from joining the whitelist even though the user calls the `addUserToWhitelist` function in Good.sol?

How can a malicious actor prevent users from joining the whitelist even though the user calls the `addUserToWhitelist` function in Good.sol?

He can't prevent it

He can add malicious code inside the helper contract

He can block the UI through which the user is calling `Good.sol` contract
Wow, lots of learning right? 🤯

Beaware of scammers, you might need to double check the code of a new dApp you want to put money in.

Submit Quiz
0 / 2
