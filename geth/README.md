# geth

This provides the details for setting up a user to run an Ethereum 1 `geth` node.

These instructions are tested on Ubuntu 19.10 server with an amd64 architecture.  Results on other configurations may vary.

## Set up an `eth1` user

`geth` should be run as a dedicated non-root user.  To set up the user run the following command as root:

```
adduser --home /home/eth1 --disabled-password --gecos 'Ethereum 1' eth1
```

## Fetch the `geth` binary

Ubuntu has a repository for `geth`.  To install it run the following commands as root:

```
add-apt-repository -y ppa:ethereum/ethereum
apt update
apt install -y geth
```

## Set up the `geth` service

Copy the [geth.service](geth.service) file to the `/etc/systemd/system` directory.  Note that the `geth` binary takes a number of options; to change these edit the copied file and alter the arguments as appropriate on the `ExecStart` line.

To start `geth`, and enable automatic restarts if the process stops or the server reboots, run the following commands as root:

```
systemctl enable geth
systemctl start geth
```

At this stage there should be a `geth` process running as the `eth1` user.  To confirm, run the following command as root:

```
systemctl status geth
```

The output should be something like this:

```
● geth.service - Ethereum 1 node
   Loaded: loaded (/etc/systemd/system/geth.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2020-01-12 23:45:14 CET; 5 days ago
 Main PID: 8893 (geth)
    Tasks: 28 (limit: 4915)
   Memory: 2.4G
   CGroup: /system.slice/geth.service
           └─8893 /usr/bin/geth --goerli --rpc --rpcaddr=127.0.0.1 --rpcport=8545 --ws --wsaddr=127.0.0.1 --wsport=8546

Jan 18 20:41:28 api geth[8893]: INFO [01-18|20:41:28.082] Imported new chain segment               blocks=1 txs=6   mgas=2.030 elaps
Jan 18 20:41:28 api geth[8893]: INFO [01-18|20:41:28.194] Chain reorg detected                     number=2023210 hash=aed727…78d2fe
Jan 18 20:41:28 api geth[8893]: INFO [01-18|20:41:28.195] Imported new chain segment               blocks=1 txs=7   mgas=2.073 elaps
Jan 18 20:41:29 api geth[8893]: INFO [01-18|20:41:29.890] Chain reorg detected                     number=2023210 hash=aed727…78d2fe
Jan 18 20:41:29 api geth[8893]: INFO [01-18|20:41:29.890] Imported new chain segment               blocks=1 txs=0   mgas=0.000 elaps
Jan 18 20:41:29 api geth[8893]: INFO [01-18|20:41:29.997] Imported new chain segment               blocks=1 txs=5   mgas=1.986 elaps
Jan 18 20:41:43 api geth[8893]: INFO [01-18|20:41:43.383] Chain reorg detected                     number=2023211 hash=d11575…55f39c
Jan 18 20:41:43 api geth[8893]: INFO [01-18|20:41:43.383] Imported new chain segment               blocks=1 txs=0   mgas=0.000 elaps
Jan 18 20:41:45 api geth[8893]: INFO [01-18|20:41:45.528] Deep froze chain segment                 blocks=4 elapsed=24.695ms  number
Jan 18 20:41:59 api geth[8893]: INFO [01-18|20:41:59.414] Imported new chain segment               blocks=1 txs=6   mgas=0.375 elaps
```

The important thing here is to confirm that the second line (Active) states 'active (running) since...' as this confirms that `geth` is running.  If it is not the log lines should provide some information as to why it is not running.
