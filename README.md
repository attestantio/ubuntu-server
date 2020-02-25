# Ethereum 2 validating infrastructure

This repository provides information about Attestant's server configuration for running an Ethereum 2 validating infrastructure: `geth` (Ethereum 1), `beacon chain` (Ethereum 2), and `validator`.

The files and instructions in this repository all fall under the [Apache LICENSE](LICENSE), however your attention is explicitly drawn to section 7 of the license regarding the disclaimer of warranty: use these instructions at your own risk.

## Individual installations

  - [eth2](eth2/README.md) setting up, configuring and running the Ethereum 2 group service
  - [geth](geth/README.md) setting up, configuring and running the Ethereum 1 `geth` process
  - [beacon-chain](beacon-chain/README.md) setting up, configuring and running the Ethereum 2 `beacon-chain` process
  - [validator](validator/README.md) setting up, configuring and running the Ethereum 2 `validator` process

Each process will run as its own user.

## Starting and stopping the eth2 service

The service is designed to be easily started and stopped with `systemctl`.

To start all components run as root:

```
systemctl start eth2
```

To stop all components run as root:

```
systemctl stop eth2
```

Individual components can be started and stopped if required, for example:

```
systemctl stop eth2-validator
systemctl start eth2-validator
```
