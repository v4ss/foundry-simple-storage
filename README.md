# Foundry Setup

First go to [https://www.getfoundry.sh] to get this command :
```bash
curl -L https://foundry.paradigm.xyz | bash
```

Run it into your terminal, then run :
```bash
foundryup
```

To see if we have installed foundry in the directory, run :
```bash
forge --version
cast --version
anvil --version
chisel --version
```

## Create a new project

Run :
```bash
forge init my_project
```

### Compile

```bash
forge compile
```

### Deploy

Run anvil to launch the VM network and the fakes accounts :
```bash
anvil
```

To deploy :
```bash
forge create ContractName [--rpc-url http://127.0.0.1:8545\] --interactive
```

#### Deploy with script

Create a new file in `script`folder : `DeploySimpleStorage.s.sol`
```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {Script} from "forge-std/Script.sol";
import {SimpleStorage} from "../src/SimpleStorage.sol";

contract DeploySimpleStorage is Script {
    function run() external returns (SimpleStorage) {
        vm.startBroadcast();
        SimpleStorage simpleStorage = new SimpleStorage();
        vm.stopBroadcast();
        return simpleStorage;
    }
}
```

Run `forge script script/DeploySimpleStorage.s.sol` to run the script and deploy the contract.

To deploy on network use this command :
```bash
forge script script/DeploySimpleStorage.s.sol --rpc-url http://127.0.0.1:8545 --broadcast --private-key PRIV_KEY
```

### Cast

Cast is a tool to do lots of things, like convert hex to decimal, or wei to eth, etc ...
```bash
cast --to-base 0x714c3 dec
```

## .ENV

Create a `.env` file in the project rrot to store secret is good for development but no for production.
To have the `.env`variables in your terminal, run :
```bash
source .env
```
Then, you are able to call this variables in your terminal like this :
```bash
echo $PRIVATE_KEY
```

### With keystore

Use the Key store file foundry file !
```bash
cast wallet import defaultKey --interactive
```
Paste your private key and set a password.

To list your recording keys run :
```bash
cast wallet list
```

Now, to run the deploy script, we can do :
```bash
forge script script/DeploySimpleStorage.s.sol --rpc-url http://127.0.0.1:8545 --account defaultKey --sender PUBLIC_KEY_ASSOCIATED_WITH_THE_PRIV_KEY --broadcast
```

### With thirdweb

Run this command to deploy your contract without store any private key in your prject folder :
```bash
npx thirdweb deploy
```

## Interact with the contract in commandline

To send and record a value :
```bash
cast send 0x5FbDB2315678afecb367f032d93F642f64180aa3 "store(uint256)" 123 --rpc-url $RPC_URL --account defaultKey
# cast send $ContractAddress $FuncSignature $Parameter --rpc-url $RPC_URL --account $CastKeySaved
```

To call a view function :
```bash
cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "retrieve()" --rpc-url $RPC_URL
```

It return the value in HEX. To convert :
```bash
cast --to-base 0x000000000000000000000000000000000000000000000000000000000000007b dec
```

## Forge formatting

You can run `forge fmt` to format your solidity code.