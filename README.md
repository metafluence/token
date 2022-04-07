# Metafluence Token

This project defined the Metafluence ERC20/BEP20 token.

# Deploy to BSC testnet

```shell
cp .env.example .env

# Set the PRIVATE_KEY value in the .env file. This will be the intial owner of the contract.

# Deploy to testnet
npx hardhat run ./scripts/deploy.ts --network bsc_testnet
```

# Deploy to BSC mainnet

```shell
cp .env.example .env

# Set the PRIVATE_KEY value in the .env file. This will be the intial owner of the contract.

# Deploy to mainnet
npx hardhat run ./scripts/deploy.ts --network bsc_mainnet
```

# Tasks

Try running some of the following tasks:

```shell
npx hardhat accounts
npx hardhat compile
npx hardhat clean
npx hardhat test
npx hardhat node
npx hardhat help
REPORT_GAS=true npx hardhat test
npx hardhat coverage
npx hardhat run scripts/deploy.ts
TS_NODE_FILES=true npx ts-node scripts/deploy.ts
npx eslint '**/*.{js,ts}'
npx eslint '**/*.{js,ts}' --fix
npx prettier '**/*.{json,sol,md}' --check
npx prettier '**/*.{json,sol,md}' --write
npx solhint 'contracts/**/*.sol'
npx solhint 'contracts/**/*.sol' --fix
```

# Etherscan verification

To try out Etherscan verification, you first need to deploy a contract to an Ethereum network that's supported by Etherscan, such as Ropsten.

In this project, copy the .env.example file to a file named .env, and then edit it to fill in the details. Enter your Etherscan API key, your Ropsten node URL (eg from Alchemy), and the private key of the account which will send the deployment transaction. With a valid .env file in place, first deploy your contract:

```shell
hardhat run --network ropsten scripts/sample-script.ts
```

Then, copy the deployment address and paste it in to replace `DEPLOYED_CONTRACT_ADDRESS` in this command:

```shell
npx hardhat verify --network ropsten DEPLOYED_CONTRACT_ADDRESS "Hello, Hardhat!"
```

# Performance optimizations

For faster runs of your tests and scripts, consider skipping ts-node's type checking by setting the environment variable `TS_NODE_TRANSPILE_ONLY` to `1` in hardhat's environment. For more details see [the documentation](https://hardhat.org/guides/typescript.html#performance-optimizations).

# Contract notes

 1) Owner`s ability to withdraw tokens from the contract. 
function withdraw(address payable addr, uint amount) external onlyOwner; 
 
The withdraw function just available for contract owner. The main reason that to move different staking campaign amounts to single staking pool if needed.  
This also brings security, because not all staked amounts staying in staked original wallets but could be transfered to cold wallets during staking time. 
 
 2) Owner`s ability to add and remove any stakings. 
function addStake(address _staker, uint256 _amount, uint256 _time) public onlyOwner; 
function removeStake(address _staker, uint _id) public onlyOwner 
 
These functions created for manamagement purposes for owner. The method can be called by Owner only.  
Some stakers accidentally transfer tokens to contract address directly, not throught the staking portal.  
To manage this types of problems, owner should have add and remove functionality to edit all records. 
 
 
 3) Owner`s ability to change status of the contract. 
function setStakeStatus(StakeStatus status) public onlyOwner; 
 
The set stake status function used for change Contract status one of the options: ACTIVE, PAUSED, COMPLETED. 
 
Users just can stake when contract status is ACTIVE.  
And their staking duration is not related to this any Statuses. Time is ticking in all statuses.  
COMPLETED status needed to owner if he want to stop this staking campaign, and create new ones.  
PAUSED status need for extreme situations to pause the specific contract, for example if we'll need upgrade smart contract.
