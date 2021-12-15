## Run

1. install node.js(>=14) and npm(>=8)
2. set your addresses in `config.json`

configs explanation:

```javascript
{
    "serverHost": "",       // mining pool server host
    "serverPort": 20032,    // mining pool server port
    "proxyPort": 30032,     // port which proxy bind
    "addresses": []         // your miner addresses(one address per group)
}
```

run miner-proxy:

```shell
npm install
npm run start
```

run miner:

```shell
gpu-miner -p 30032
```

# Build miner proxy

Dependencies :
- install node.js(>=14) and npm(>=8)
 
One can build the miner proxy into a self-contained binary file with:
```shell
npm install
npm run build
```

## Building for ARM64 Mac OS

At the moment, one is only able to build the MacOS binaries for ARM64 on a Mac device. 

To do so, run the following commands:
```shell
npm install
npx pkg . --target node16-macos-arm64 --no-native-build
```

# Docker and docker-compose setup

Mind configuring `SERVER_HOST` and `ADDRESSES` in the snippet below

```
version: "3.3"
services:
  miner:
    image: alephium/gpu-miner:latest
    command:
      - "-a"
      - "proxy"
    restart: always
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]

  proxy:
    image: alephium/mining-proxy:latest
    restart: unless-stopped
    ports:
      - 10973
    environment:
      - SERVER_HOST=1.2.3.4
      - PROXY_PORT=10973
      - ADDRESSES=["1A4...","12G...","1Hk...","179..."]
```
