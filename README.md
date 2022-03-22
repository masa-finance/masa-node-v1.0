# Masa Testnet Node V1.01
## Release Date
February 22nd, 2022
## Requirements
Tested with: Go Version 1.16.14
Download [here](https://go.dev/dl/)
## Roadmap & Todo's
The Masa Node UI is in alpha and will get incremental releases, please report all bugs you find as an issue [here](https://github.com/masa-finance/masa-node-v1.0/issues)
# Run With Docker
This guide will get you up and running using docker. If you want to us the geth binary please navigate to the bottom section of the page [here](#run-with-geth).
## Get Docker
1. Install Docker (https://www.docker.com/get-started)
    - If your Docker distribution does not contain `docker-compose`, follow [this](https://docs.docker.com/compose/install/) to install Docker Compose
    - Make sure your Docker daemon has at least 4G memory
    - Required: Docker Engine 18.02.0+ and Docker Compose 1.21+
## Install The Masa Testnet Node v1.01
The Docker compose file also launches the Node UI which can be accessed at the following URL
`http://localhost:3000`

Navigate here to interact with the node
```
git clone https://github.com/masa-finance/masa-node-v1.0
cd masa-node-v1.0
```
### Directory structure
```
masa-node-v1/
├── network
│   ├── testnet
│       ├── genesis.json
├── node
│   ├── data
├── src
│   ├── ui
│   ├── geth files
│   ├── ...
├── docker-compose.yml
├── genesis.json
```
## Run Docker
1. Run ` PRIVATE_CONFIG=ignore docker-compose up -d`
   ```sh
   cd masa-node-v1.0
   PRIVATE_CONFIG=ignore docker-compose up -d
   ```
1. Run `docker ps` to verify that you masa-node container is **healthy**
1. Run `docker logs <container-name> -f` to view the logs for a particular container

1. __Note__: to attach geth to your node Javascript console (use the same container id or name from `docker ps`
   ```sh
   docker exec -it masa-node-v10_masa-node_1 /bin/sh
   geth attach /qdata/dd/geth.ipc

   Welcome to the Geth JavaScript console!

   instance: Geth/node1-istanbul/v1.9.24-stable-d5ef77ca(quorum-v21.7.1)/linux-amd64/go1.15.5
   coinbase: 0xa3178965a2022c8374afe6690182f54d48208d0a
   at block: 18008 (Thu Dec 09 2021 20:45:32 GMT+0000 (UTC))
   datadir: /qdata/dd
   modules: admin:1.0 debug:1.0 eth:1.0 istanbul:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0
   To exit, press ctrl-d
   > 

1. To shutdown the Masa Testnet Node
   ```sh
   docker-compose down
   ```
## Troubleshooting Docker
1. Docker is frozen or containers crash and reboot
    - Check if your Docker daemon is allocated enough memory (minimum 4G)
### Additional Bootnodes
Masa operates several bootnodes, one is already included in the Docker file by default. If you are having issues connecting to the bootnode please use an alternaitve from the list below.

We are also looking for community run bootnodes to add to our list. Please reach out to us on Discord or Submit a PR to this repo if you want to add a bootnode to the community list.
#### Masa Bootnodes
```
admin.addPeer("enode://ac6b1096ca56b9f6d004b779ae3728bf83f8e22453404cc3cef16a3d9b96608bc67c4b30db88e0a5a6c6390213f7acbe1153ff6d23ce57380104288ae19373ef@54.146.254.245:21000")
admin.addPeer("enode://91a3c3d5e76b0acf05d9abddee959f1bcbc7c91537d2629288a9edd7a3df90acaa46ffba0e0e5d49a20598e0960ac458d76eb8fa92a1d64938c0a3a3d60f8be4@54.158.188.182:21000")
admin.addPeer("enode://d87c03855093a39dced2af54d39b827e4e841fd0ca98673b2e94681d9d52d2f1b6a6d42754da86fa8f53d8105896fda44f3012be0ceb6342e114b0f01456924c@34.225.220.240:21000")
admin.addPeer("enode://fcb5a1a8d65eb167cd3030ca9ae35aa8e290b9add3eb46481d0fbd1eb10065aeea40059f48314c88816aab2af9303e193becc511b1035c9fd8dbe97d21f913b9@52.1.125.71:21000")
```
#### Community Bootnodes
```
admin.addPeer("enode://571be7fe060b183037db29f8fe08e4fed6e87fbb6e7bc24bc34e562adf09e29e06067be14e8b8f0f2581966f3424325e5093daae2f6afde0b5d334c2cd104c79@142.132.135.228:21000")
admin.addPeer("enode://269ecefca0b4cd09bf959c2029b2c2caf76b34289eb6717d735ce4ca49fbafa91de8182dd701171739a8eaa5d043dcae16aee212fe5fadf9ed8fa6a24a56951c@65.108.72.177:21000")
admin.addPeer("enode://3dc62888f70f47b7ff1178719208bf8835b94bd3ba818ac5ee5e30b6cfbd23459f0f4bfd999ec01e79b2e71c42570370299c6983c2e5268d56d75852de4e2e93@93.189.145.167:21000")
admin.addPeer("enode://163687ec1da1537ef366dbec7c988f4f3e6a4d3daec24b6ae54189b1d4b287bb21f646b657251d51480ac468ae3dbef43b69b430c884277bbdc18e6a7e447071@65.108.221.203:21001")
admin.addPeer("enode://eb0b7b308eb041ae9b75cac334552e44408e1e39a7852a6ed5c9ab9a125da7db6b32b8e083f192a2fb1d311dc9948620affa1e5e7fadf13f8306792027716545@65.108.221.203:21000")
admin.addPeer("enode://eb0b7b308eb041ae9b75cac334552e44408e1e39a7852a6ed5c9ab9a125da7db6b32b8e083f192a2fb1d311dc9948620affa1e5e7fadf13f8306792027716545@65.108.221.203:21002")
admin.addPeer("enode://cbec93685d944fc6ac18cfbed52e870a2bc5a0c3ee0ab2fd9a4bc3b9892edc01bdfdf83ed81bfbb953b028f52360aa22b5b4d535bbdee8f8ebfd31a5700d9307@107.191.52.182:21000")
admin.addPeer("enode://5a25539fdfbd8be0c42bc3070f05f3f8170840f9772fafae157c3bc849a179507b3aea14638944ee58de55133a63e1f1d9248aaa1e3a86e3cbe3caf3c40d6f96@136.244.80.248:21000")
admin.addPeer("enode://2f30d27e1e6277e9149e7e06f5114a09cf359623b04928b5d970242fa371b4d3579cfcded67246b1da937d6d6b2c2c0f144d60e2e0aed973a9c2d3601baae205@65.108.219.150:21000")
admin.addPeer("enode://39deb5f99be54e380d58d7aa40b0369e89826d9be048821c9c91219855a7cf8938a13255129422f17519d5d46a7829cdbfd4805abacb7f182cfdca398576154c@88.86.76.6:21000")
admin.addPeer("enode://d2c5cb6c5154279b07a6d6f94b42755eb5c4f675cfbf6789a78f177cd3b033a07a991bc01388e6c301f486a046e7b39b1ac463cb5905117813f10af1e5217dc0@62.113.115.132:21000")
admin.addPeer("enode://1d2c02ee6aab562e57a66f5e62a5a9db0342a4fc9b444962aaf05ea246e82cb88b315d9bca9fad9bc400c63399685667f8b146e64673b6e4bcf98f4f4865d094@193.178.172.6:21000")
admin.addPeer("enode://bf0d89bbe5a7f553aebb1a293d818bb9e1786eb9a942bb8e3e1815c6f82125cac1ca8a4c2072d7224b6369566380316f07d3a73f3163bf348818ff8fb8affdb0@95.181.59.186:21000")
admin.addPeer("enode://05a029cd3f29afd01091a9063fe146e80e6053b9b02f37b63c0d46dfd92810e00c3d4778f864f16ca8cecfe54678f166bffb5ad8fb710f3172ac8c6507e82dce@185.188.183.114:21000")
admin.addPeer("enode://6fcbfebfe871f62ee982e09aa64342267f3bd534c536239612dc38270b90819dc3720c5452aa558f15096a5e4672063f506c679417c9d86008c6b90770541a6c@159.89.117.156:21000")
admin.addPeer("enode://4d5996ccea6d21bdb85443e6bf5beb8483344cb81f4599f7d2db19f23deec09b2bc098f87de0c65729ff6c861ac51b8618563df96f699ab687ba14be039a6c27@45.132.104.180:30300")
admin.addPeer("enode://82e43e46702f1ce0126a8978c4d8be7f03b0fa82203c4d7db82c04b4c6826e1dbeca4f1d7fab0694c56bfa8bda38c68054ed485cff828917a03a9e2db21b9772@161.97.101.127:21000")
```
Submit a PR to add a bootnode to the community list [here](https://github.com/masa-finance/masa-node-v1.0/pulls). 
## Node Syncing
It can take some time for your node to fully sync to the Masa Testnet 2.0 - please be patient while your node catches up with the most recent blocks.
## Node UI
### Specification
- React.js & Typescript
- Docker for deployment
- The Node UI runs when you deploy using the Docker compose files above
Navigate to you local host to interact with the Masa Node
`http://localhost:3000`
# Run With Geth
To run from source follow these steps
## Clone the repository and build the source:
```
git clone https://github.com/masa-finance/masa-node-v1.0
cd masa-node-v1.0/src
make all
```
**`make all` must be run from within the src folder**
## Run the tests: 
```
make test
```
## Add PATH
### Method 1
Binaries are placed in `$REPO_ROOT/build/bin`. You must add the `bin` folder to `PATH` to make `geth` and `bootnode` easily invokable from the command line. For example, if `Users/yourname/masa-node-v1.0` is the location you have cloned the masa-node repository to on your computer. In your terminal run 
```
sudo nano /etc/paths
or
export PATH=$PATH:$REPO_ROOT/build/bin
```
Remember to source your $PATH or restart the terminal. Run `echo $PATH` from the command line to check that the `PATH` has been added correctly. 

For example;
```
echo $PATH

gives the following response

/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/quorum/build/bin:/Users/yourname/masa-node/build/bin:/usr/local/go/bin:/usr/local/share/dotnet:~/.dotnet/tools:/Library/Frameworks/Mono.framework/Versions/Current/Commands
```
### Method 2
The second way to make geth and bootnode easily invokable is to copy the binaries located in `$REPO_ROOT/build/bin` to a folder already in your `PATH` file such as `/usr/local/bin`.
### Method 3

You can also supplement `PATH` by adding add `PATH=$PATH:$REPO_ROOT/build/bin` to your `~/.bashrc`, `~/.bash_aliases`, or `~/bash_profile` file.
## Testing PATH
When you run geth from the command line from an arbitrary folder you will get the following output on the terminal.
```
geth

returns

INFO [12-08|05:37:18.131] Starting Geth on Ethereum mainnet... 
INFO [12-08|05:37:18.131] Bumping default cache on mainnet         provided=1024 updated=4096
INFO [12-08|05:37:18.131] Running with private transaction manager disabled - quorum private transactions will not be supported 
INFO [12-08|05:37:18.132] Maximum peer count                       ETH=50 LES=0 total=50
INFO [12-08|05:37:18.160] Set global gas cap                       cap=25000000
INFO [12-08|05:37:18.160] Running with private transaction manager disabled - quorum private transactions will not be supported 
INFO [12-08|05:37:18.160] Allocated trie memory caches             clean=1023.00MiB dirty=1024.00MiB
INFO [12-08|05:37:18.160] Allocated cache and file handles         database=/Users/brendanplayford/Library/Ethereum/geth/chaindata cache=2.00GiB ...
...
INFO [12-08|05:37:18.751] Started P2P networking                   self=enode://162cfffb34b0c3e76abeb9f31541737fcd3b622e35fa3b0080a14dfb9d2a53168ac3abf10122b79d3b8d7d55516982e0f903d179916ccb51abe5cd00de1bdb07@127.0.0.1:30303
INFO [12-08|05:37:18.752] IPC endpoint opened                      url=/Users/brendanplayford/Library/Ethereum/geth.ipc isMultitenant=false
INFO [12-08|05:37:18.752] Security Plugin is not enabled 
Fatal: Consensus not specified. Exiting!!
```
## Initialize the node
Navigate to the `node` directory and initialize the first node.
The repo directory includes the `genesis.json` file that is used to connect to the Masa protocol at the following path `../network/testnet/genesis.json`

Run the following command
```
cd node
geth --datadir data init ../network/testnet/genesis.json
```
You will get the following output;
```
INFO [12-09|18:22:24.031] Running with private transaction manager disabled - quorum private transactions will not be supported 
INFO [12-09|18:22:24.035] Maximum peer count                       ETH=50 LES=0 total=50
INFO [12-09|18:22:24.063] Set global gas cap                       cap=25000000
INFO [12-09|18:22:24.064] Allocated cache and file handles         database=/Users/brendanplayford/masa/masa-node-v1.0/node/data/geth/chaindata cache=16.00MiB handles=16
INFO [12-09|18:22:24.135] Writing custom genesis block 
INFO [12-09|18:22:24.140] Persisted trie from memory database      nodes=7 size=1.02KiB time="280.583µs" gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [12-09|18:22:24.141] Successfully wrote genesis state         database=chaindata hash="69b521…fb4c77"
INFO [12-09|18:22:24.141] Allocated cache and file handles         database=/Users/brendanplayford/masa/masa-node-v1.0/node/data/geth/lightchaindata cache=16.00MiB handles=16
INFO [12-09|18:22:24.204] Writing custom genesis block 
INFO [12-09|18:22:24.205] Persisted trie from memory database      nodes=7 size=1.02KiB time="162.437µs" gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [12-09|18:22:24.205] Successfully wrote genesis state         database=lightchaindata hash="69b521…fb4c77"
```

## Set your node identity
Set your own identity of your node on the Masa protocol to be easily identified in a list of peers. 

For example; we name our node 'MasaMoonNode' by setting the flag `--identity MasaMoonNode` will set up an identity for your node so it can be identified as MasaMoonNode in a list of peers.
**Update your flag `--identity MasaMoonNode` to be unique**
## Start the node
In the `node` directory, start the node by running the following command:
```
PRIVATE_CONFIG=ignore geth --identity MasaMoonNode --datadir data --bootnodes enode://91a3c3d5e76b0acf05d9abddee959f1bcbc7c91537d2629288a9edd7a3df90acaa46ffba0e0e5d49a20598e0960ac458d76eb8fa92a1d64938c0a3a3d60f8be4@54.158.188.182:21000 --emitcheckpoints --istanbul.blockperiod 10 --mine --miner.threads 1 --syncmode full --verbosity 5 --networkid 190250 --rpc --rpccorsdomain "*" --rpcvhosts "*" --rpcaddr 127.0.0.1 --rpcport 8545 --rpcapi admin,db,eth,debug,miner,net,shh,txpool,personal,web3,quorum,istanbul --port 30300
```
### Additional Bootnodes
Masa operates several bootnodes, one is already included in the comnand above by default. If you are having issues connecting to the bootnode please use an alternaitve from the list below.

We are also looking for community run bootnodes to add to our list. Please reach out to us on Discord or Submit a PR to this repo if you want to add a bootnode to the community list.
#### Masa Bootnodes
```
enode://ac6b1096ca56b9f6d004b779ae3728bf83f8e22453404cc3cef16a3d9b96608bc67c4b30db88e0a5a6c6390213f7acbe1153ff6d23ce57380104288ae19373ef@54.146.254.245:21000

enode://91a3c3d5e76b0acf05d9abddee959f1bcbc7c91537d2629288a9edd7a3df90acaa46ffba0e0e5d49a20598e0960ac458d76eb8fa92a1d64938c0a3a3d60f8be4@54.158.188.182:21000

enode://d87c03855093a39dced2af54d39b827e4e841fd0ca98673b2e94681d9d52d2f1b6a6d42754da86fa8f53d8105896fda44f3012be0ceb6342e114b0f01456924c@34.225.220.240:21000

enode://fcb5a1a8d65eb167cd3030ca9ae35aa8e290b9add3eb46481d0fbd1eb10065aeea40059f48314c88816aab2af9303e193becc511b1035c9fd8dbe97d21f913b9@52.1.125.71:21000
```
#### Community Bootnodes
```
enode://571be7fe060b183037db29f8fe08e4fed6e87fbb6e7bc24bc34e562adf09e29e06067be14e8b8f0f2581966f3424325e5093daae2f6afde0b5d334c2cd104c79@142.132.135.228:21000
enode://269ecefca0b4cd09bf959c2029b2c2caf76b34289eb6717d735ce4ca49fbafa91de8182dd701171739a8eaa5d043dcae16aee212fe5fadf9ed8fa6a24a56951c@65.108.72.177:21000

```
Submit a PR to add a bootnode to the community list [here](https://github.com/masa-finance/masa-node-v1.0/pulls). 
## Node Syncing
It can take some time for your node to fully sync to the Masa Testnet 2.0 - please be patient while your node catches up with the most recent blocks.
## Node UI
You must be running Docker to run the Node UI with geth
### Specification
- React.js & Typescript
- Docker for deployment
## Run The Masa Node UI
Follow these instructions to run the Node UI with geth
```
cd masa-node-v1.0
cd src
cd ui
docker-compose up ui
```
Navigate to you local host to interact with the Masa Node
`http://localhost:3000`
