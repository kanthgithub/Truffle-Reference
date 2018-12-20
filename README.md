# Truffle-Reference
All In updates and Reference for Truffle

## Truffle V5:

Bring your own compiler! You can now choose any solc-js version available at solc-bin. Just tell Truffle which version you want to use and it will automatically fetch it for you! This feature also includes the ability to use Docker and native binaries in an advanced way.

Web3 1.0 - The truffle-contract package is now using Web3.js 1.0! This will greatly improve error handling!

New migrations command - In v5, the migrate command has been completely rewritten. We have included better error messages: e.g., when a deployment fails Truffle will notify you and give you an idea of possible fixes. Truffle will now also give you more information about what is going on when you deploy a contract. This includes cost summaries and real time status updates about the amount of time a transaction has been pending.

Dry run simulations also now run automatically if you are deploying to a known public network and using the --interactive flag on the command line will give you a prompt between dry runs and real deployments.

Another improvement is the ability to configure the amount of block confirmations in between deployments. This allows the user to specify the number of blocks to wait before transactions time out.

Usage analytics - Usage analytics have been added and users can opt-in by running truffle config --enable-analytics. We think this feature will be a vital tool that will help us to make informed decisions about the future of Truffle and to determine what is most valuable to our users. When enabled, Truffle will collect information about your version number, the commands you run, and whether commands succeed or fail. And of course, it will do so in an anonymous fashion.

truffle run <command> - Truffle now gives users the ability to create custom command plugins. This feature is still in its infancy so let us know what you think!

Vyper support - Another added feature is that Truffle will now compile *.vy contracts. We have published a Truffle Box to help you get started with Vyper. You can access it by running:

$ truffle unbox vyper-example
(Make sure you have Vyper installed.)

Solidity v0.5.0 - Truffle also now ships with Solidity v0.5.0 by default. Solidity v0.4.xx is still supported and can be used by specifying the version you wish to use in your Truffle config.

Structured function parameters - Truffle has been upgraded to use Web3.js v1.0. This has allowed us to include support for passing/returning ‘struct’s in Solidity functions. To use this ability, you need to specify the following at the top of your contracts:

$ pragma experimental ABIEncoderV2
This feature allows you to use complex function arguments and have the values returned so that they can interact with other contracts via truffle-contract’s JS interface.

Help system - Now access Truffle’s built in help system by running:

$ truffle help <command>
This allows you to see all the available options for the command as well as a description of what it does.

Unique truffle develop mnemonics - Truffle will now generate random mnemonics that will persist only for you. Use caution with crypto security when working with mnemonics and private keys!

Debugger improvements - v5 includes debugger breakpoints! Add breakpoints using the ’b’ command and remove them using the ‘B’ command. Additionally we have added mapping support for the debugger!

truffle init / truffle unbox - These commands have been improved and you are now asked if you want to overwrite files in the case of name conflicts in the target directory. A force option (--force) has also been added in the case where you wish to overwrite the files automatically and avoid the prompt.

async/await - Support for async/await syntax in the Truffle console is also now available!


## Truffle Pig:

TrufflePig 🍄🐷
Greenkeeper badge

TrufflePig is a development tool that provides a simple HTTP API to find and read from Truffle-generated contract files, for use during local development.

The pig in action!

Installation
Install globally:

$ yarn global add trufflepig
Or as a devDependency to your truffle project:

$ yarn add trufflepig --dev
Prerequisites
A Truffle framework project with built contract json files
Some means of making HTTP requests and parsing the JSON results
Usage
Just run

$ trufflepig
from your truffle project and access your contracts under

http://localhost:3030/contracts?name=MyContractName
and looks like this:

{
  "contractName": "MyContractName",
  "abi": {},
  ...
}
The contracts will be queried by the contractName property in the truffle .json output. Once contract files are changed, trufflepig picks it up and serves the changed version of it.

Serving accounts data from ganache or geth / parity
Trufflepig not only helps you to access those delicious truffles but also to easily access the set up addresses in ganache, geth or parity. These will be available under

http://localhost:3030/accounts
and looks like this:

{
  "0x57bb04b8a56c4530ea75ded0a8a0632987d7ec44":"0xd4f10bbe0e132a0d7a7aea3d92e68791f548b67dc5d1dac8ad56edfbc5038ba5",
  "0x241b5d67f21f23d03dec3ffc50504472f265745f":"0xb526e1b11956eb45c3c306a9fef1775b44e22c5e6aec30e103d7d973c6b29189",
  ...
}
Usage with ganache
Start ganache using

$ ganache-cli --acctKeys ganache-accounts.json
This will create a ganache-accounts.json in the directory you run ganache from (preferably your project directory)

Then run trufflepig using

$ trufflepig --ganacheKeyFile ganache-accounts.json
Usage with keystore files
You can run your prefered ethereum development node with some accounts set up. In parity run

$ parity --keys-path YOUR_PREFERRED_KEYPATH
and in geth

$ geth --keystore YOUR_PREFERRED_KEYPATH
to define the directory for accounts to use when starting the node. This directory has to contain one or more keystore files in json format.

Trufflepig can conveniently serve those, too! Just start it with

$ trufflepig --keystoreDir YOUR_PREFERRED_KEYPATH [--keystorePassword KEYSTORE_PASSWORD]
Use the --keystorePassword option to provide a password in case your keyfiles ar encrypted.

API
Command line usage
$ trufflepig --help
Options:
  --help                  Show help [boolean]
  --version               Show version number [boolean]
  -p, --port              Port to serve the contracts from [number] [default: 3030]
  -v, --verbose           Be extra chatty [boolean] [default: false]
  -c, --contractDir       Directory to read contracts from [string] [default: "./build/contracts"]
  -g, --ganacheKeyFile    Ganache accounts file (.json), will serve accounts under /accounts [string]
  -k, --keystoreDir       Directory for keystore files, will serve accounts under /accounts [string]
  -s, --keystorePassword  Password to decrypt keystore files [string]
Programmatic usage
const TrufflePig = require('trufflepig');
 
const pig = new TrufflePig({
    // These are the defaults
    contractDir: './build/contracts',
    port: 3030,
    verbose: true,
    ganacheKeyFile: '',
    keystoreDir: '',
    keystorePassword: '',
});
 
pig.start();
