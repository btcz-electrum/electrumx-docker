# electrumx-docker
Simple Docker deployment of electrumx server for BitcoinZ

## Prerequisites
* Ubuntu Linux VPS (not OpenVZ if RAM is 4GB or less) with 80 - 100GB space, 4+ cores
* 4-8GB RAM (if 4GB RAM, create 4GB swap space)
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

* You'll need your hosts IP address to replace `<YOUR HOST IP>` in the LAST TWO lines in command list below.
```
git clone https://github.com/btcz-electrum/electrumx-docker.git
cd electrumx-docker
sudo docker build -t electrumv1 .;
sudo docker run -d --restart on-failure:5 --add-host=host.docker.internal:host-gateway --name btcz-elec1 -p 50001:50001 -p 50002:50002 -e DAEMON_URL=http://bitcoinzrpc:bitcoinz9jk01Amn3Z@host.docker.internal:1979 -e COIN=BitcoinZ -e REPORT_SERVICES=tcp://<YOUR HOST IP or HOSTNAME if using DNS>:50001,ssl://<YOUR HOST IP or HOSTNAME if using DNS>:50002,wss://<YOUR HOST IP or HOSTNAME if using DNS>:50004 -it electrumv1;
```

### Troubleshooting
* Make sure you open Firewall ports for TCP ports 50001 and 50002
* You can check logs with `sudo docker logs -f <CONTAINER ID>` (you get ID of container with `sudo docker ps`)
* If you're running to issues with the daemon crashing, you can try adding swap space (search Google).  If you're using an OpenVZ VPS (instead of KVM for example), you won't be able to add swap space.  So if you're using an OpenVZ VPS you might need to have at least 8GB of RAM.
