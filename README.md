
# Group 1 Lesson 4 Homework Report 
--- 
*"Knowledge is Power"*

## Executive Summary 
--- 
Group 1's objective was to deploy, evaluate, and test the functionality of the 04-Tests-Scripts repository. We chose to use a TDD methodology throughout development and testing. That being said, we worked collaboratively to complete the project requirements described in the repository's `README.md`

There are many details to consider when setting up your environment for smart contract development. An awareness of the current state of different testnets and the mechanisms in which you interact with them from API keys to contract interaction can be the difference between a failed initiative and a successful deployment. 

Smart contracts cannot access the ledger directly as the ledger is maintained by miners/validators. In this case, we deployed our ballot on the Ropsten test network using Infura as our gateway to the Ethereum network. 

The rest of this report contains the descriptions of our findings. 

## Contract Data 
* Chairman Address: 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377
* Ballot Address: 0x78b0bf923b5c43f500C22e6fc7E26c22B6216e44
* Voters Address: 0x70997970C51812dc3A010C7d01b50e0d17dc79C8
* TX Hash:
0x762752d51ae37c9df23dd7a739f54f013596aaa272b7940fd6e1a2ca144d5c59

## Environment Setup 
--- 
After yarn and friends are successfully installed and runnable. The next thing we did was to update our .env file parameters. Surprisingly for Ropsten via Alchemy are deprecated. Acknowledging this, our options were to switch to Goerli or change to Infura. Infura was chosen to avoid confusion and extra steps needed to find and recieve ETH from faucets. These changes were introduced in commit f6f0820. (https://github.com/Encode-Club-Group-1/04-Tests-Scripts/commit/f6f0820aaa69d512cc278c815fcc8e008363cc88)

![image](https://user-images.githubusercontent.com/90874464/179407654-a1c1d9b4-57ff-4fe5-a1d4-5c7b9a703f33.png)

Then we copied over our keys using `cp .\.env.example .env`. 

Moving along, We ran `yarn hardhat node` to get wallet addresses and private key’s. Of special not is the third field parameter in .env 

```
$ yarn hardhat node
yarn run v1.22.19

Started HTTP and WebSocket JSON-RPC server at http://127.0.0.1:8545/


Accounts
========

WARNING: These accounts, and their private keys, are publicly known.
Any funds sent to them on Mainnet or any other live network WILL BE LOST.

Account #0: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 (10000 ETH)
Private Key: 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80

Account #1: 0x70997970C51812dc3A010C7d01b50e0d17dc79C8 (10000 ETH)
Private Key: 0x59c6995e998f97a5a0044966f0945389dc9e86dae88c7a8412f4603b6b78690d

Account #2: 0x3C44CdDdB6a900fa2b585dd299e03d12FA4293BC (10000 ETH)
Private Key: 0x5de4111afa1a4b94908f83103eb1f1706367c2e68ca870fc3fb9a804cdab365a

Account #3: 0x90F79bf6EB2c4f870365E785982E1f101E93b906 (10000 ETH)
Private Key: 0x7c852118294e51e653712a81e05800f419141751be58f605c371e15141b007a6
```

```
$ yarn hardhat accounts
yarn run v1.22.19

0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
0x70997970C51812dc3A010C7d01b50e0d17dc79C8
0x3C44CdDdB6a900fa2b585dd299e03d12FA4293BC
0x90F79bf6EB2c4f870365E785982E1f101E93b906
0x15d34AAf54267DB7D7c367839AAf71A00a2C6A65
0x9965507D1a55bcC2695C58ba16FB37d819B0A4dc
0x976EA74026E726554dB657fA54763abd0C3a0aa9
0x14dC79964da2C08b23698B3D3cc7Ca32193d9955
0x23618e81E3f5cdF7f54C3d65f7FBc0aBf5B21E8f
0xa0Ee7A142d267C1f36714E4a8F75612F20a79720
0xBcd4042DE499D14e55001CcbB24a551F3b954096
0x71bE63f3384f5fb98995898A86B02Fb2426c5788
0xFABB0ac9d68B0B445fB7357272Ff202C5651694a
0x1CBd3b2770909D4e10f157cABC84C7264073C9Ec
0xdF3e18d64BC6A983f673Ab319CCaE4f1a57C7097
0xcd3B766CCDd6AE721141F452C550Ca635964ce71
0x2546BcD3c84621e976D8185a91A922aE77ECEc30
0xbDA5747bFD65F08deb54cb465eB87D40e51B197E
0xdD2FD4581271e230360230F9337D5c0430Bf44C0
0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199
Done in 12.17s.
```
From there, we were able to deploy our contract to the Ropsten test network.

The private key is used to sign transactions. It's importance ought to be underscored as the digital signature is used to verify the sender (`msg.sender`) of a message using the signature itself. Functions that interact with digital signatures such as `ethers.getSigners()` require that these parameters are indeed present.   

We really did not need to worry about the fact that hardhat generates shared wallets for this project. But, it should also be noted that using the wallets offered via hardhat are *never* safe to use in production and can result in loss of funds. 

Deployment Output: 

```
$ yarn ts-node scripts/Ballot/deployment.ts Sushi Ramen Pho
Using address 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377
========= NOTICE =========
Request-Rate Exceeded  (this message will not be repeated)

The default API keys for each service are provided as a highly-throttled,
community resource for low-traffic projects and early prototyping.

While your application will continue to function, we highly recommended
signing up for your own API keys to improve performance, increase your
request rate/limit and enable other perks, such as metrics and advanced APIs.

For more details: https://docs.ethers.io/api-keys/
==========================
Wallet balance 1.721668484186917
Deploying Ballot contract
Proposals:
Proposal N. 1: Sushi
Proposal N. 2: Ramen
Proposal N. 3: Pho
Awaiting confirmations
Completed
Contract deployed at 0x78b0bf923b5c43f500C22e6fc7E26c22B6216e44
```

Query Proposal Output: 

```
$ yarn ts-node scripts/Ballot/queryProposal.ts 0x78b0bf923b5c43f500C22e6fc7E26c22B6216e44 2
Using address 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377
========= NOTICE =========
Request-Rate Exceeded  (this message will not be repeated)

The default API keys for each service are provided as a highly-throttled,
community resource for low-traffic projects and early prototyping.

While your application will continue to function, we highly recommended
signing up for your own API keys to improve performance, increase your
request rate/limit and enable other perks, such as metrics and advanced APIs.

For more details: https://docs.ethers.io/api-keys/
==========================
Wallet balance 1.719982082179047
Querying Ballot contract
Proposals: 
Proposal N. 1: 0x50686f0000000000000000000000000000000000000000000000000000000000
Proposal N. 2: 0
BigNumber { _hex: '0x00', _isBigNumber: true }
0
0
2
```

giveVotingRights Output: 
```
$ yarn ts-node scripts/Ballot/giveVotingRights.ts 0x78b0bf923b5c43f500C22e6fc7E26c22B6216e44 0x70997970C51812dc3A010C7d01b50e0d17dc79C8
Using address 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377
Wallet balance 1.7181812361706432
Attaching ballot contract interface to address 0x78b0bf923b5c43f500C22e6fc7E26c22B6216e44
Giving right to vote to 0x70997970C51812dc3A010C7d01b50e0d17dc79C8
Awaiting confirmations
Transaction completed. Hash: 0x762752d51ae37c9df23dd7a739f54f013596aaa272b7940fd6e1a2ca144d5c59
```
sendVote Output: 
```
$ yarn ts-node scripts/Ballot/sendVote.ts 0x78b0bf923b5c43f500C22e6fc7E26c22B6216e44 2
Using address 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377
Wallet balance 1.7180352651699133
Ballot address 0x78b0bf923b5c43f500C22e6fc7E26c22B6216e44
Voted - false
Proposal current total Votes: 0
Awaiting tx confirmation..
========= NOTICE =========
Request-Rate Exceeded  (this message will not be repeated)

The default API keys for each service are provided as a highly-throttled,
community resource for low-traffic projects and early prototyping.

While your application will continue to function, we highly recommended
signing up for your own API keys to improve performance, increase your
request rate/limit and enable other perks, such as metrics and advanced APIs.

For more details: https://docs.ethers.io/api-keys/
==========================
Transaction done - Hash: 0x1395db6287cd9891b1000b860e4ffe10fa6e5a1665a63287148ec88a2216faea
Proposal total Votes: 1
```

delegateVote Output: 
```
$ yarn ts-node scripts/Ballot/delegateVote.ts 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377 0x90F79bf6EB2c4f870365E785982E1f101E93b906
Using address 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377
Wallet balance 1.7176105401678825
Attaching ballot contract interface to address 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377
Voter at address 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377 delegate vote to 0x90F79bf6EB2c4f870365E785982E1f101E93b906 
========= NOTICE =========
Request-Rate Exceeded  (this message will not be repeated)

The default API keys for each service are provided as a highly-throttled,
community resource for low-traffic projects and early prototyping.

While your application will continue to function, we highly recommended
signing up for your own API keys to improve performance, increase your
request rate/limit and enable other perks, such as metrics and advanced APIs.

For more details: https://docs.ethers.io/api-keys/
==========================
Awaiting confirmations
Transaction completed. Hash: 0xe3236726d6de0d542dc164827b00bc3200417f5dd63b40fd7dc0d79902d1253b
```

queryWinner Output: 
```
$ yarn ts-node scripts/Ballot/queryWinner.ts 0x78b0bf923b5c43f500C22e6fc7E26c22B6216e44 
Using address 0x63FaC9201494f0bd17B9892B9fae4d52fe3BD377
Wallet balance 1.7075468921675856
Attaching ballot contract interface to address 0x78b0bf923b5c43f500C22e6fc7E26c22B6216e44
The winning proposal: Pho
```

## Summary of Contract Behaviour 
--- 
#### Overview of Contract Behaviour and Structure
![Ballot](https://user-images.githubusercontent.com/90874464/179416772-78362d46-bf48-4d5f-a1fc-18694cb2c2c9.png)

The contract is a voting contract. It has a number of functions that are used to manage the voting process. The following are the functions that are used:

The contract has a number of functions that are used to manage the voting process. The following are the functions that are used. 


## Contract Breakdown 
--- 

![image](https://user-images.githubusercontent.com/90874464/179417509-ed34b43f-8f4b-4e1f-ab41-aa57d36f5532.png)


## Testing 
--- 
![image](https://user-images.githubusercontent.com/90874464/179417316-f2da2a24-17b8-4ce6-ad57-dee792e54833.png)

## Dependency Graph 
--- 
![image](https://user-images.githubusercontent.com/90874464/179528499-01db96b0-c285-4a70-8e5d-447b366ff653.png)
