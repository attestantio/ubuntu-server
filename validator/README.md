# validator

This provides the details for setting up a user to run an Ethereum 2 `validator` node.

These instructions are tested on Ubuntu 19.10 server with an x64 architecture.  Results on other configurations may vary.

## Set up an `validator` user

`validator` should be run as a dedicated non-root user.  To set up the user run the following command as root:

```
adduser --home /home/validator --disabled-password --gecos 'Ethereum 2 validator' validator
mkdir /home/validator/bin
```

## Build and install the `validator` binary

To build the `validator` binary follow the instructions at [https://github.com/prysmaticlabs/prysm/#build-via-bazel](https://github.com/prysmaticlabs/prysm/#build-via-bazel).  Note the dependencies at [https://github.com/prysmaticlabs/prysm/#dependencies](https://github.com/prysmaticlabs/prysm/#dependencies) need to be installed prior to building the binary.

Once the binary has been built it can be found in the bazel installation in the directory `bazel-bin/validator/linux_amd64_stripped`.  This file must be copied to `/home/validator/bin`, then run the following command as root:

```
chown -R validator:validator /home/validator/bin
```

## Set up the `validator` service

Copy the [eth2-validator.service](eth2-validator.service) file to the `/etc/systemd/system` directory.  Note that the `validator` binary takes a number of options; to change these edit the copied file and alter the arguments as appropriate on the `ExecStart` line.

To enable `eth2-validator`, and enable automatic restarts if the process stops or the server reboots, run the following command as root:

```
systemctl enable eth2-validator
```

The `eth2-validator` service should be run as part of the overall `eth2` service, however if required it can be started and stopped using `systemctl start eth2-validator` and `systemctl stop eth2-validator`.
  
The status of the beacon chain service can be obtained with:

```
systemctl status validator
```

The output should be something like this:

```
● eth2-validator.service - Ethereum 2 Validator
   Loaded: loaded (/etc/systemd/system/eth2-validator.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2020-01-18 21:01:03 CET; 12min ago
 Main PID: 32686 (validator)
    Tasks: 18 (limit: 4915)
   Memory: 48.6M
   CGroup: /system.slice/eth2-validator.service
           └─32686 /home/validator/bin/validator

Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
Jan 18 21:13:08 api validator[32686]: {"depositInclusionSlot":0,"level":"info","msg":"Waiting to be activated","positionInActivation
```

The important thing here is to confirm that the second line (Active) states 'active (running) since...' as this confirms that `validator` is running.  If it is not the log lines should provide some information as to why it is not running.
