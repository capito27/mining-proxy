## Run

1. install node.js(>=14) and npm(>=8)
2. set your addresses in `config.json`

configs explanation:

```javascript
{
    "diff1TargetNumZero": 30,
    "serverHost": "",       // mining pool server host
    "serverPort": 20032,    // mining pool server port
    "proxyPort": 30032,     // port which proxy bind
    "workerName": "",       // custom worker name, length cannot exceed 32
    "address": ""           // address for receiving rewards
}
```

you can also replace `address` with four miner addresses like v0.2.x proxy:

```javascript
    "diff1TargetNumZero": 30,
    "serverHost": "",       // mining pool server host
    "serverPort": 20032,    // mining pool server port
    "proxyPort": 30032,     // port which proxy bind
    "workerName": "",       // custom worker name, length cannot exceed 32
    "addresses": []         // four miner addresses
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

N.B : At this point in time, it is only possible to build binaries for windows, linux and MacOS for the x64 architecture

Dependencies :
- install node.js(>=14), npm(>=8)

One can build the miner proxy into a self-contained binary file with the following command:
```shell
npm install
npm run build
```

The final executable can be found in the 'bin' directory.

# Docker and docker-compose setup

Mind configuring `SERVER_HOST` and `ADDRESSES` or `ADDRESS` in the snippet below

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
# If you're using multi addresses, add the 4 addresses here (in group order)
      - ADDRESSES=["1A4...","12G...","1Hk...","179..."]
# If you're using a single address, add it here. Please avoid using Exchange wallet address here!!
      - ADDRESS=1A4...
```
