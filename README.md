# Masa Node V1.0

## Get Docker
1. Install Docker (https://www.docker.com/get-started)
    - If your Docker distribution does not contain `docker-compose`, follow [this](https://docs.docker.com/compose/install/) to install Docker Compose
    - Make sure your Docker daemon has at least 4G memory
    - Required Docker Engine 18.02.0+ and Docker Compose 1.21+


## Install The Masa Testnet Node v1.0

```
git clone https://github.com/masa-finance/masa-node-v1
cd masa-node-v1
```
Directory structure
```
masa-node-v1/
├── node
│   ├── data
├── node-ui
├── genesis.json
├── docker-compose.yml
```


1. Run ` PRIVATE_CONFIG=ignore docker-compose up -d`
   ```sh
   cd masa-node-v1
   PRIVATE_CONFIG=ignore docker-compose up -d
   ```
1. Run `docker ps` to verify that you masa-node container is **healthy**
1. Run `docker logs <container-name> -f` to view the logs for a particular container

1. __Note__: to attach geth to your node Javascript console (use the same container id or name from `docker ps`
   ```sh
   $ docker exec -it masa-node-v10_node geth attach /qdata/dd/geth.ipc
   Welcome to the Geth JavaScript console!

   instance: Geth/node1-istanbul/v1.7.2-stable/linux-amd64/go1.9.7
   coinbase: 0xd8dba507e85f116b1f7e231ca8525fc9008a6966
   at block: 70 (Thu, 18 Oct 2018 14:49:47 UTC)
    datadir: /qdata/dd
    modules: admin:1.0 debug:1.0 eth:1.0 istanbul:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

   > 
   ```
1. To shutdown the Masa Testnet Node
   ```sh
   docker-compose down
   ```

## Troubleshooting Docker
1. Docker is frozen or containers crash and reboot
    - Check if your Docker daemon is allocated enough memory (minimum 4G)

## Node UI
### Specification
- Next.js & Typescript
- Docker for deployment
### Config
Next.js is launched in Docker using the following `Dockerfile`

```
# Naively Simple Node Dockerfile

FROM node:14.17-alpine

RUN mkdir -p /home/app/ && chown -R node:node /home/app
WORKDIR /home/app
COPY --chown=node:node . .

USER node

RUN yarn install --frozen-lockfile
RUN yarn build

EXPOSE 3000
CMD [ "yarn", "start" ]
```
### Running
`cd node-ui`

Build the image
`docker build -t masa-node-ui .`

Check the local built image
`docker image ls`

Start the docker container
`docker run -p 3000:3000 masa-node-ui`

Navigate to you local host to interact with the Masa Node
`http://localhost:3000`