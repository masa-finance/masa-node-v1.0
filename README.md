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
## Node UI
### Specification
- React.js & Typescript
- Docker for deployment
- The Node UI runs when you deploy using the Docker compose files above
Navigate to you local host to interact with the Masa Node
`http://localhost:3000`
#Run With Geth