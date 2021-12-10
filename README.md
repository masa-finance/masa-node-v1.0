# Masa Testnet Node V1.0

## Get An OpenVPN File
You must must be connected to our OpenVPN network in order to join the Masa Testnet and access bootnodes and node IP's. Please reach out to an admin on Discord (https://discord.gg/SXwRKNMc) to get an OpenVPN file! Please download the OpenVPN Connect client to connect to the Masa Testnet cluster [here](https://openvpn.net/vpn-client/)

## Starting OpenVPN service example:
```sh
root@server: apt install openvpn net-tools -y

root@server: vim /etc/openvpn/client/masa-testnet-dev-client-community.ovpn  # <-- put Your config here

root@server: openvpn --config /etc/openvpn/client/masa-testnet-dev-client-community.ovpn  # <-- check it's ok and [ctrl+c]

root@server: echo '[Unit]
Description=OpenVPN connection
After=network.target

[Service]
Type=forking
ExecStart=/usr/sbin/openvpn --status /run/openvpn/masa.status 10 --cd /etc/openvpn/client/ --config /etc/openvpn/client/masa-testnet-dev-client-community.ovpn
ExecReload=/bin/kill -HUP $MAINPID
WorkingDirectory=/etc/openvpn

[Install]
WantedBy=multi-user.target
openvpn@.service ' >> /etc/systemd/system/masa-openvpn.service

root@server: systemctl daemon-reload

root@server: systemctl enable masa-openvpn.service

root@server: systemctl start masa-openvpn.service
```

### Check you can access the node IP range througe OpenVPN
Check your routing table by running `netstat -rn` from the command line to ensure you can access the Masa Testnet. You will see `172.16.239/24      10.254.0.17        UGSc         utun4` if you have OpenVPN setup correctly. 
```sh
   netstat -rn

   Internet:
   Destination        Gateway            Flags        Netif Expire
   default            192.168.1.1        UGScg          en0       
   10.254.0.1/32      10.254.0.17        UGSc         utun4       
   10.254.0.16/30     10.254.0.18        UGSc         utun4       
   10.254.0.17        10.254.0.18        UH           utun4       
   127                127.0.0.1          UCS            lo0       
   127.0.0.1          127.0.0.1          UH             lo0       
   169.254            link#6             UCS            en0      !
   172.16.239/24      10.254.0.17        UGSc         utun4       
```

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
├── network
│   ├── testnet
│       ├── genesis.json
├── node
│   ├── data
├── node-ui
├── docker-compose.yml
├── genesis.json
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
   docker exec -it masa-node-v10_masa-node_1 geth attach /qdata/dd/geth.ipc
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
