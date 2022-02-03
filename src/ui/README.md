# Masa Node

## Node UI
### Specification
- React.js & Typescript
- Docker for deployment
### Config
```
# pull the official base image
FROM node:16
# set working direction
WORKDIR /app
# install application dependencies
COPY package.json ./
COPY package-lock.json ./
RUN npm i
# add app
COPY . ./
# start app
CMD ["npm", "start"]
```
### Running
`docker-compose up ui`

Navigate to you local host to interact with the Masa Node
`http://localhost:3000`


