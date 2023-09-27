# backstage-app

## Setup

Update NPM
```sh
npm install -g npm@10.1.0
```

Set node to version 18
```sh
nvm use 18 && nvm alias default 18
```

Create backstage app
```sh
npx @backstage/create-app@latest
```

Set the name to `backstage-app` and a folder `backstage-app` will apperar with all the code inside 

## [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

To start the app, run:

```sh
yarn install
yarn dev
```

One `yarn dev` is complete a URL will be avaliable to run the backstage application.

## Build a docker image

```sh
yarn install --frozen-lockfile
```

```sh
yarn tsc
```

```sh
yarn build:backend --config ../../app-config.yaml
```

```sh
docker image build . -f packages/backend/Dockerfile --tag backstage
```

If the following error:
```
Dockerfile:45
--------------------
  44 |
  45 | >>> RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000 \
  46 | >>>     yarn install --frozen-lockfile --production --network-timeout 300000
  47 |
--------------------
ERROR: failed to solve: process "/bin/sh -c yarn install --frozen-lockfile --production --network-timeout 30000000" did not complete successfully: exit code: 1
```

try....
```sh
yarn cache clean
yarn install
```
or
```sh
yarn install --network-timeout 1000000
```

I spoted this:
```sh
The engine "node" is incompatible with this module. Expected version ">=18.12.0". Got "16.20.2"
```

Then try updating node version in Dockerfile
```
FROM node:18-bullseye-slim
```

Take the latest Dockerfile from [Backstage: Building a Docker image](https://backstage.io/docs/deployment/docker/) in the Host Build seciton. 

## Github Action publishing Docker image to Docker Hub

For the only image currnetly pushed to Docker Hub, run:
```sh
docker pull lloydb2/backstage-app:pr-4
```

