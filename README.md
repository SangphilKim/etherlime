[![npm version](https://badge.fury.io/js/etherlime.svg)](https://badge.fury.io/js/etherlime) 
[![travis build status](https://img.shields.io/travis/LimeChain/etherlime/master.svg)](https://travis-ci.org/LimeChain/etherlime)
[![code coverage status](https://img.shields.io/codecov/c/github/LimeChain/etherlime/master.svg)](https://codecov.io/gh/LimeChain/etherlime)
[![Documentation Status](https://readthedocs.org/projects/etherlime/badge/?version=latest)](https://etherlime.readthedocs.io/en/latest/?badge=latest)
[![Gitter chat](https://badges.gitter.im/lime-tech-talks/Lobby.png)](https://gitter.im/lime-tech-talks/Lobby)


# etherlime

**etherlime** is an ethereum development and deployment framework based on [ethers.js](https://github.com/ethers-io/ethers.js/).

This framework provides alternative to the other web3.js based frameworks and allows for ultimate control by the developer. It also adds much needed verboseness in the deployment process so that you can be aware of what is really going on (as opposed to the general shooting in the dark technique).

This framework was born out of necessity, hardships and trouble in the development and deployment of ethereum smart contract. We are trying to ease the pain of deployment, compilation and unit testing and add much needed stability to the process. In our mind ethers is much more stable alternative than web3.js for the moment therefore this framework is born.

**Milestones:**
1. [Ready]Being able to deploy compiled contracts (compiled in the truffle format) on local and infura nodes <---- Done
2. [Ready]Being able to compile contracts to the desired formats for deployment <---- Done
3. [Ready]Being able to run unit tests on the compiled contracts <---- Done
4. [Ready]Being able to run unit tests with code coverage <---- Done
5. [Not Ready]Being able to debug transactions <---- Next

## Installing

```
npm i -g etherlime
```

## Documentation

[Documentation](https://etherlime.readthedocs.io/en/latest/)

## Running embedded ganache-cli

```
etherlime ganache
```

## Setting up your project to deploy with etherlime

```
etherlime init
```
This will create `contracts` directory and `deployment` directory with `deploy.js` file inside. You can use this file to write your deployment procedure.

## Compiling

```
etherlime compile [dir] [runs] [solcVersion] [docker] [list] [all] [quite]
```

* ``dir`` - [Optional] By specifying ``dir`` you can set the root directory where to read the contracts and place the build folder. By default ``dir`` is set to the current working directory ``./``
* ``runs`` - [Optional] By specifying ``runs`` between 1 and 999 you enabled the optimizer and set how many times the optimizer will be run. By default the optimizer is not enabled.
* ``solcVersion`` - [Optional] By specifying ``solcVersion`` you can set the version of the solc which will be used for compiling the smart contracts. By default it use the solc version from the node_modules folder.
* ``docker`` - [Optional] When you want to use a docker image for your solc you should set ``docker=true`` in order ``solcVersion`` to accept the passed image.
* ``list`` - [Optional] By specifying ``list`` you can list the available solc versions. The following values can be used: ``docker``, ``releases``, ``prereleases`` and ``latestRelease``. By default only 10 version are listed
* ``all`` - [Optional] By specifying ``all`` together with ``list`` you will be able to list all available solc versions.
* ``quite`` - [Optional] Disable verboseness during compilation. By the default ``quite`` is set to false.

## Deploying

In order to deploy your deployment file following command:
```
etherlime deploy [file] [network] [secret] [-s] [compile] [runs]
```

* ``file`` - [Optional] By specifying ``--file`` you can use another file as long as you keep the structure of the file (exporting an ``async deploy`` function with ``network`` and ``secret`` params)
* ``network`` - [Optional] By specifying ``--network`` you can specify the network param to be passed to your deploy method
* ``secret`` - [Optional] By specifying ``secret`` you can specify the secret param to be passed to your deploy method. Comes in very handy for passing private keys.
* ``-s`` - [Optional] Silent - silences the verbose errors 
* ``compile`` - [Optional] Enable compilation of the smart contracts before their deployment. By default the deployment is done with a compilation
* ``runs`` - [Optional] Enables the optimizer and runs it the specified number of times

The deployment process is verbose and gives you real-time info about the performed actions. In addition there is a report of the actions when the deployment finishes (as not all of us monitor the deployment process constantly);

## Testing 

To start your tests you need to run the following command:
```
etherlime test [path] [skip-compilation]
```

* ``path`` - [Optional] By specifying ``path`` you can set a path to a selected directory or you can set the path directly to the javascript file which contains your tests. By default the ``path`` points to ``./test``
* ``skip-compilation`` - [Optional] This parameter controls wether a compilation will be ran before the tests are started. Default: false.

## Unit tests coverage
You can run the built in solidity-coverage tool by using the following command:
```
etherlime coverage [path] [port] [runs]
```

Parameters:

* ``path`` - [Optional] By specifying ``path`` you can set a path to a selected directory or you can set the path directly to the javascript file which contains your tests. By default the ``path`` points to ``./test``.
* ``port`` - [Optional] The port to run the solidity coverage testrpc (compatible with etherlime ganache deployer). Default: 8545.
* ``runs`` - [Optional] By specifying number runs you can enable the optimizer of the compiler with the provided number of optimization runs to be executed. Compilation is always performed by solidity coverage.

### History of your deploys
In order to see a list of what you've deployed you can run the following command:
```
etherlime history
```

## Quick Deployer Example

```
const etherlime = require('etherlime');

const TestContract = require('./TestContract.json');

const defaultConfigs = {
	gasPrice: 20000000000,
	gasLimit: 4700000
}

const deploy = async (network) => {

	const deployer = new etherlime.InfuraPrivateKeyDeployer('Your Private Key Goes Here', 'ropsten', 'Your Infura API Key', defaultConfigs);
	
	const result = await deployer.deploy(TestContract, {}, '0xda8a06f1c910cab18ad187be1faa2b8606c2ec86', 1539426974);
}

```

Result of `etherlime deploy` would be something like this:
[![Deployment result](DeploymentResult.png)](https://imgur.com/a/NyLX9mH)

# License
Completely MIT Licensed. Including ALL dependencies.
