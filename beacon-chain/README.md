# beacon-chain

This provides the details for setting up a user to run an Ethereum 2 `beacon-chain` node.

These instructions are tested on Ubuntu 19.10 server with an x64 architecture.  Results on other configurations may vary.

## Set up an `eth2` user

`beacon-chain` should be run as a dedicated non-root user.  To set up the user run the following command as root:

```
adduser --home /home/eth2 --disabled-password --gecos 'Ethereum 2' eth2
mkdir -p /home/eth2/bin
```

## Build and install the `beacon-chain` binary

To build the `beacon-chain` binary follow the instructions at [https://github.com/prysmaticlabs/prysm/#build-via-bazel](https://github.com/prysmaticlabs/prysm/#build-via-bazel).  Note the dependencies at [https://github.com/prysmaticlabs/prysm/#dependencies](https://github.com/prysmaticlabs/prysm/#dependencies) need to be installed prior to building the binary.

Once the binary has been built it can be found in the bazel installation in the directory `bazel-bin/beacon-chain/linux_amd64_stripped`.  This file must be copied to `/home/eth2/bin`, then run the following command as root:

```
chown -R eth2:eth2 /home/eth2/bin
```

## Set up the `eth2-beacon-chain` service

Copy the [eth2-beacon-chain.service](eth2-beacon-chain.service) file to the `/etc/systemd/system` directory.  Note that the `beacon-chain` binary takes a number of options; to change these edit the copied file and alter the arguments as appropriate on the `ExecStart` line.

To enable `eth2-beacon-chain`, and enable automatic restarts if the process stops or the server reboots, run the following command as root:

```
systemctl enable eth2-beacon-chain
```

The `eth2-beacon-chain` service should be run as part of the overall `eth2` service, however if required it can be started and stopped using `systemctl start eth2-beacon-chain` and `systemctl stop eth2-beacon-chain`.

> Note that `geth` should be fully synced before starting the beacon chain, otherwise it will not be able to find the Ethereum 2 deposit contract and so will not start properly.

The status of the beacon chain service can be obtained with:

```
systemctl status eth2-beacon-chain
```

The output should be something like this:

```
● eth-beacon-chain.service - Ethereum 2 Beacon chain
   Loaded: loaded (/etc/systemd/system/eth2-beacon-chain.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2020-01-18 21:01:02 CET; 4s ago
 Main PID: 32660 (beacon-chain)
    Tasks: 21 (limit: 4915)
   Memory: 186.6M
   CGroup: /system.slice/eth2-beacon-chain.service
           └─32660 /home/eth2/bin/beacon-chain --web3provider=ws://localhost:8546/ --http-web3provider=http://localhost:8545/

Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=info msg="Starting beacon node" prefix=node version="Prysm
Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=info msg="Starting 8 services: [*p2p.Service *powchain.Ser
Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=info msg="Collecting metrics at endpoint" endpoint=":8080"
Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=info msg="RPC-API listening on port" address="0.0.0.0:4000
Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=warning msg="You are using an insecure gRPC connection! Pr
Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=info msg="Connected to eth1 proof-of-work chain" endpoint=
Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=info msg="Starting initial chain sync..." prefix=initial-s
Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=info msg="Waiting for enough suitable peers before syncing
Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=info msg="Blockchain data already exists in DB, initializi
Jan 18 21:01:05 dev beacon-chain[32660]: time="2020-01-18 21:01:05" level=info msg="Node started p2p server" multiAddr="/ip4/148.251
```

The important thing here is to confirm that the second line (Active) states 'active (running) since...' as this confirms that `beacon-chain` is running.  If it is not the log lines should provide some information as to why it is not running.
