# Masa Testnet Node V1.01
## Release Date
February 22nd, 2022
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
   docker exec -it masa-node-v10_masa-node_1 
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
Masa operates several bootnodes that are included in the Docker file by default. We are also looking for community run bootnodes to add to our list. Please reach out to us on Discord or Submit a PR to this repo if you want to add a bootnode to the community list.
#### Masa Bootnodes
```
enode://ac6b1096ca56b9f6d004b779ae3728bf83f8e22453404cc3cef16a3d9b96608bc67c4b30db88e0a5a6c6390213f7acbe1153ff6d23ce57380104288ae19373ef@54.146.254.245:21000

enode://91a3c3d5e76b0acf05d9abddee959f1bcbc7c91537d2629288a9edd7a3df90acaa46ffba0e0e5d49a20598e0960ac458d76eb8fa92a1d64938c0a3a3d60f8be4@54.158.188.182:21000

enode://d87c03855093a39dced2af54d39b827e4e841fd0ca98673b2e94681d9d52d2f1b6a6d42754da86fa8f53d8105896fda44f3012be0ceb6342e114b0f01456924c@34.225.220.240:21000

enode://fcb5a1a8d65eb167cd3030ca9ae35aa8e290b9add3eb46481d0fbd1eb10065aeea40059f48314c88816aab2af9303e193becc511b1035c9fd8dbe97d21f913b9@52.1.125.71:21000
```
#### Community Bootnodes

## Node UI
### Specification
- React.js & Typescript
- Docker for deployment
- The Node UI runs when you deploy using the Docker compose files above
Navigate to you local host to interact with the Masa Node
`http://localhost:3000`
# Run With Geth