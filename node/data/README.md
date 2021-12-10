# Buil From source

## Clone the repository and build the source:
```
git clone https://github.com/masa-finance/masa-node-v1
cd masa-node
make all
```
## Run the tests: 
```
make test
````
## Add PATH
### Method 1
Binaries are placed in `$REPO_ROOT/build/bin`. You must add the `bin` folder to `PATH` to make `geth` and `bootnode` easily invokable from the command line. For example, if `Users/yourname/masa-node` is the location you have cloned the masa-node repository to on your computer. In your terminal run 
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
INFO [12-08|05:37:18.160] Allocated cache and file handles         database=/Users/brendanplayford/Library/Ethereum/geth/chaindata cache=2.00GiB handles=5120
INFO [12-08|05:37:18.496] Opened ancient database                  database=/Users/brendanplayford/Library/Ethereum/geth/chaindata/ancient
INFO [12-08|05:37:18.496] Writing default main-net genesis block 
INFO [12-08|05:37:18.651] Persisted trie from memory database      nodes=12356 size=1.78MiB time=37.306849ms gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [12-08|05:37:18.651] Initialised chain configuration          config="{ChainID: 1 Homestead: 1150000 DAO: 1920000 DAOSupport: true EIP150: 2463000 EIP155: 2675000 EIP158: 2675000 Byzantium: 4370000 IsQuorum: false Constantinople: 7280000 TransactionSizeLimit: 64 MaxCodeSize: 0 Petersburg: 7280000 Istanbul: 9069000, Muir Glacier: 9200000 YOLO v2: <nil> PrivacyEnhancements: <nil> PrivacyPrecompile: <nil> Engine: ethash}"
WARN [12-08|05:37:18.651] Ethash used in full fake mode 
INFO [12-08|05:37:18.651] Initialising Ethereum protocol           name=eth versions="[65 64 63]" network=1337 dbversion=<nil>
WARN [12-08|05:37:18.651] Upgrade blockchain database version      from=<nil> to=8
INFO [12-08|05:37:18.652] Loaded most recent local header          number=0 hash="d4e567…cb8fa3" td=17179869184 age=52y8mo1w
INFO [12-08|05:37:18.652] Loaded most recent local full block      number=0 hash="d4e567…cb8fa3" td=17179869184 age=52y8mo1w
INFO [12-08|05:37:18.652] Loaded most recent local fast block      number=0 hash="d4e567…cb8fa3" td=17179869184 age=52y8mo1w
INFO [12-08|05:37:18.653] Regenerated local transaction journal    transactions=0 accounts=0
INFO [12-08|05:37:18.678] Allocated fast sync bloom                size=2.00GiB
WARN [12-08|05:37:18.679] permission-config.json file is missing. Smart-contract-based permission service will be disabled error="stat /Users/brendanplayford/Library/Ethereum/permission-config.json: no such file or directory"
INFO [12-08|05:37:18.679] No plugins to initialise 
INFO [12-08|05:37:18.679] Starting peer-to-peer node               instance=Geth/v1.9.25-stable-919800f0(quorum-v21.10.0)/darwin-amd64/go1.17.2
INFO [12-08|05:37:18.743] Initialized fast sync bloom              items=12356 errorrate=0.000 elapsed=65.268ms
INFO [12-08|05:37:18.750] New local node record                    seq=1 id=f0046acc3cd39187 ip=127.0.0.1 udp=30303 tcp=30303
INFO [12-08|05:37:18.751] Started P2P networking                   self=enode://162cfffb34b0c3e76abeb9f31541737fcd3b622e35fa3b0080a14dfb9d2a53168ac3abf10122b79d3b8d7d55516982e0f903d179916ccb51abe5cd00de1bdb07@127.0.0.1:30303
INFO [12-08|05:37:18.752] IPC endpoint opened                      url=/Users/brendanplayford/Library/Ethereum/geth.ipc isMultitenant=false
INFO [12-08|05:37:18.752] Security Plugin is not enabled 
Fatal: Consensus not specified. Exiting!!
```

# Steps

## Install the Masa protocol

```
git clone https://github.com/masa-finance/masa-node-v1
cd masa-node-v1
```
Directory structure
```
masa-node-v1/
├── network
│   ├── testnet
│       ├── genesis.json
├── node
│   ├── data
├── node-ui
├── docker-compose.yml
```
The directory includes the `genesis.json` file that is used to connect to the Masa protocol
## Initialize the node
In the `node` directory, initialize the first node:
```
geth --datadir data init ../network/genesis.json
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

Update your flag `--identity MasaMoonNode` to be unique

## Start the node
In the `node` directory, start the node by running the following command:
```
PRIVATE_CONFIG=ignore geth --identity MasaMoonNode --datadir data --bootnodes enode://ac6b1096ca56b9f6d004b779ae3728bf83f8e22453404cc3cef16a3d9b96608bc67c4b30db88e0a5a6c6390213f7acbe1153ff6d23ce57380104288ae19373ef@172.16.239.11:21000  --emitcheckpoints --istanbul.blockperiod 1 --mine --minerthreads 1 --syncmode full --verbosity 5 --networkid 190250 --rpc --rpccorsdomain "*" --rpcvhosts "*" --rpcaddr 127.0.0.1 --rpcport 8545 --rpcapi admin,db,eth,debug,miner,net,shh,txpool,personal,web3,quorum,istanbul --port 30300
```
### Additional bootnode addresses
Masa is looking for node operators to run additional bootnodes in different regions. Please submit an issue on the masa-node-v1.0 repo in Github with your enode information [here](https://github.com/masa-finance/masa-node-v1.0); for example:
```
[
"enode://1647ade9de728630faff2a69d81b2071eac873d776bfdf012b1b9e7e9ae1ea56328e79e34b24b496722412f4348b9aecaf2fd203fa56772a1a5dcdaa4a550147@127.0.0.1:30300?discport=0",
yeah 
]
```

## Attach to the node
In another terminal in the `node` directory, attach to the node:
```
geth attach data/geth.ipc
```
## Check peer count
Use the JavaScript console to check the peer count:
```
net.peerCount
```