# eth2

This provides the details for etting up the group service to run all Ethereum 2 processes.

These instructions are tested on Ubuntu 19.10 server with an x64 architecture.  Results on other configurations may vary.

## Set up the `eth2` service

Copy the [eth2.service](eth2.service) file to the `/etc/systemd/system` directory.

To enable `eth2`, and enable automatic restarts if the process stops or the server reboots, run the following command as root:

```
systemctl enable eth2
```

The `eth2` service is designed to start and stop all child processes (Ethereum 1 node, Ethereum 2 node, and validator), once they have been configured as per the other instructions in this repository.  To start them run the following command as root:

```
systemctl start eth2
```

and to stop them run the following command as root:

```
systemctl stop eth2
```

The `eth2` service does not by itself run any Ethereum processes, however it can be checked to see if it is active or not.  To check the status run the following command as root:

```
systemctl status eth2
```

The output should be something like this:

```
● eth2.service - Ethereum 2 service group
   Loaded: loaded (/etc/systemd/system/eth2.service; enabled; vendor preset: enabled)
   Active: active (exited) since Tue 2020-02-25 22:41:12 GMT; 6s ago
  Process: 24858 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
 Main PID: 24858 (code=exited, status=0/SUCCESS)

Feb 25 22:41:12 dev systemd[1]: Starting Ethereum 2 service group...
Feb 25 22:41:12 dev systemd[1]: Started Ethereum 2 service group.
```

The important thing here is to confirm that the second line (Active) states 'active...' as this confirms that `eth2` is active and will start its child processes.  If it is not the log lines should provide some information as to why it is not running.
