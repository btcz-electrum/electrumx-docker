# electrumx-docker
Simple Docker deployment of electrumx server for BitcoinZ

## Prerequisites
* Ubuntu Linux VPS with 80 - 100GB space, 4+ cores, 4-8GB RAM
* Docker 20.10+ (to allow internal host binding w/ --add-host)
* Fully synced BitcoinZ Daemon already running w/ example config below ([Daemon Binaries](https://github.com/btcz/bitcoinz/releases))
* Allow ports TCP 50001, 50002 through Firewall
* Some brains

### BitcoinZ Example Config
```
showmetrics=0
rpcthreads=50
maxconnections=50
daemon=1
server=1
whitelist=127.0.0.1
whitelist=172.17.0.2
whitelist=172.17.0.3
txindex=1
addressindex=1
timestampindex=1
spentindex=1
rpcuser=bitcoinzrpc
rpcpassword=bitcoinz9jk01Amn3Z
rpcallowip=127.0.0.1
rpcallowip=172.17.0.2
rpcallowip=172.17.0.3
```

### How To Step by Step
```
git clone https://github.com/btcz-electrum/electrumx-docker.git
cd electrumx-docker
sudo docker build -t electrumv1 .;
sudo docker run -d --add-host=host.docker.internal:host-gateway --name btcz-elec1 -p 50002:50002 -e DAEMON_URL=http://bitcoinzrpc:bitcoinz9jk01Amn3Z@host.docker.internal:1979 -e COIN=BitcoinZ -it electrumv1;
sudo docker run -d --add-host=host.docker.internal:host-gateway --name btcz-elec2 -p 50001:50001 -e DAEMON_URL=http://bitcoinzrpc:bitcoinz9jk01Amn3Z@host.docker.internal:1979 -e COIN=BitcoinZ -it electrumv1;
```

### Troubleshooting
* Make sure you open Firewall ports for TCP ports 50001 and 50002
* You can check logs with `sudo docker logs -f <CONTAINER ID>` (you get ID of container with `sudo docker ps`) 
