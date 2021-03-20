# Real World docker-compose

## Build.

1. git clone with submodules   
`git clone  --recurse-submodules git@github.com:collonian/realworld-compose.git `
2. build backend/frontend images
```shell
docker-compose -f docker-compose.yaml -f docker-compose.dev.yaml build
```

## Launch
1. launch mysql as daemon
```shell
cd ./mysql
docker-compose up -d
```
2. launch backend/frontend
```shell
docker-compose -f docker-compose.yaml -f docker-compose.dev.yaml up -d
```
Open your browser and connect to [backend](http://localhost:8080/tags) and [frontend](http://localhost:3000).
You can connect a remote debugger on a backend with localhost:5005.

if you want to launch on production env., just do this.
```shell
docker-compose -f docker-compose.yaml -f docker-compose.prod.yaml up -d
```
You can't access backend/frontend directly when you launch on production.
Use [nginx proxy](http://localhost) to check it.