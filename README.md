# Redis HA setup with Sentinel 
* Vagrant (Ubuntu 16.04.7 LTS (Xenial Xerus))
* Redis
* Sentinel

## Overview
There are 3 nodes, each running Redis and Sentinel.
- `node1` - Redis master
- `node2` - Redis replica
- `node3` - Redis replica with `replica_priority 0`

Sentinel logs are located at `/var/log/redis/sentinel-default.log`

## Usage
```sh
vagrant up
```
## Test Redis replication and Sentinel failover
1. Simulate `node1` down for 1 minute
    ```sh
    # on node1
    redis-cli -p 6379 DEBUG sleep 60
    ```

2. Check thant `node2` has been promoted
    ```sh
    # on node2
    redis-cli -p 6379 info replication
    ```
    Sentinel promoted `node2` to master and reconfigured `node3` to replicate the new master.

3. Trigger a [manual failover](#manual-failover) to promote `node1` back to master

## Manual failover
```sh
redis-cli -p 26379 sentinel failover mymaster
```
## Links
* [Redis replication docs](https://redis.io/topics/replication)
* [Sentinel docs](https://redis.io/topics/sentinel/)
