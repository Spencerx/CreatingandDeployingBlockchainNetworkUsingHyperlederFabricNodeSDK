# Creating and Deploying a Blockchain Network using Hyperledger Fabric Node SDK

## Instructions for setting the blockchainNetwork

Welcome to Part 1 of building a Blockchain Application.  The first step is focused on creating and deploy a Hyperledger Blockchain Network using the Hyperledger Fabric Node SDK. 

## Included Components
* Hyperledger Fabric
* Docker
* Hyperledger Fabric SDK for node.js


## Application Workflow Diagram
![Application Workflow](images/Pattern1-Build-a-network.png)

## Prerequisites
* [Docker](https://www.docker.com/products/overview) - v1.13 or higher
* [Docker Compose](https://docs.docker.com/compose/overview/) - v1.8 or higher

## Steps
1. [Run Build.sh Script to build network](#1-run-the-build.sh-script)
2. [Start the Network](#2-start-the-network)
3. [Check the logs to see the results](#3-check-the-logs)
4. [Check the Blockchain Network](#4-check-the-blockchainnetwork)

## 1. Run the Build.sh Script
This accomplishes the following:

a. Clean up system by removing any existing blockchain docker images

b. Generate certificates

  * The `crypto-config.yaml` (Crypto configuration file) defines the identity of "who is who". It tells peers and orderers what organization they belong to and what domain they belong to.

c.  Create Peers, Orderers and Channel

  * The `configtx.yaml` file initializes a blockchain network or channel and services with an Orderer Genesis Block which serves as the first block on a chain. Additionally, membership services are installed on each channel peer (in this case, the Shop and Fitcoin Peers).

d. Build docker images of the orderer, peers, channel, network

### Open a new terminal and run the following command:
```bash
export FABRIC_CFG_PATH=$(pwd)
chmod +x cryptogen
chmod +x configtxgen
chmod +x generate-certs.sh
chmod +x generate-cfgtx.sh
chmod +x docker-images.sh
chmod +x build.sh
chmod +x clean.sh
./build.sh
```

## 2. Start the Network

Make sure the 'LOCALCONFIG' environment variable is unset if you are re-running this step after running the test below
```bash
unset LOCALCONFIG  
```

There 2 options to install chaincode on the peer nodes and start the Blockchain network. You can select any one of the following:
* Using LevelDB to store the blockchain state database. Run the following command to start the network:
```bash
docker-compose -p "fitcoin" -f "docker-compose.yaml" up -d    
```
* Using CouchDB to store the blockchain state database. Run the following command to start the network:
```bash
docker-compose -p "fitcoin" -f "docker-compose-couchdb.yaml" up -d    
```

## 3. Check the logs

You will see the results of running the script

**Command**
```bash
docker logs blockchain-setup
```
**Output:**
```bash
Register CA fitcoin-org
CA registration complete  FabricCAServices : {hostname: fitcoin-ca, port: 7054}
Register CA shop-org
CA registration complete  FabricCAServices : {hostname: shop-ca, port: 7054}
info: [EventHub.js]: _connect - options {"grpc.ssl_target_name_override":"shop-peer","grpc.default_authority":"shop-peer"}
info: [EventHub.js]: _connect - options {"grpc.ssl_target_name_override":"fitcoin-peer","grpc.default_authority":"fitcoin-peer"}
Default channel not found, attempting creation...
Successfully created a new default channel.
Joining peers to the default channel.
Chaincode is not installed, attempting installation...
Base container image present.
info: [packager/Golang.js]: packaging GOLANG from bcfit
info: [packager/Golang.js]: packaging GOLANG from bcfit
Successfully installed chaincode on the default channel.
Successfully instantiated chaincode on all peers.
```


## 4.  Check the BlockchainNetwork

Execute the following commands to to test the network by performing the `invoke` and `query` operations on the network:
```bash
cd configuration
export LOCALCONFIG=true
node config.js
cd ..
cd test/
npm install
```

If you are using LevelDB, then run the following command:
```bash
node index.js
```

If you are using CouchDB, then run the following command:
```bash
node indexCouchDB.js
```


## Additional Resources
* [Hyperledger Fabric Docs](http://hyperledger-fabric.readthedocs.io/en/latest/)
* [Hyperledger Composer Docs](https://hyperledger.github.io/composer/introduction/introduction.html)

## License
[Apache 2.0](LICENSE)
