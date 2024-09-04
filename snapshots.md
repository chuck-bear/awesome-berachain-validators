# Guide to Snapshotting

## What is a snapshot?
* Collection of files from a node's backend, representing its state at the point of time when it was captured.
* Useful as backups, for syncing nodes that have failed, corrupted, slowed down, or even spinning up a new node.
  * Much faster to restore a snapshot than letting it sync from the network.
* _Types_
  * Archive snapshot - Full history of the blockchain - used in full/archive nodes (massive - size can run into hundreds of GBs)
  * Pruned snapshot - Contains history of the last few states. Useful for quickly setting up validators.
* Snapshots can be applied to both the consensus (beacond) and execution (geth, reth, etc.) clients. In fact, syncing can be a lot faster when both snapshots are simultaneously restored. While this document focusses on beacond snapshots, you can refer to other resources related to your specific execution client.
  * Example - [snapshotting on reth](https://snapshots.merkle.io/). While ETH mainnet is used in this example, the process on berachain remains the same.
* beacond snapshots are specific to the database being used - eg. leveldb or pebbledb. Please bear this in mind as you attempt to create or restore snapshots.

## Create snapshot
Shut down your node - it should not be writing any new data (which could render your snapshot corrupt). 

Verify that the process isn't running:
```
ps aux|grep beacond
```

Create a snapshot archive from the data directory:
```cd $BEACOND_HOME
tar cvf - data | lz4 > snapshot.tar.lz4
```

Start your node again to resume regular operation.

### Pruning
Set the pruning parameters in `config/app.toml` to avoid retaining the entire chain state. The pruning will automatically occur for the specified time interval.

```
pruning = "everything"

pruning-keep-recent = "1000"
pruning-interval = "10"
```

Restart beacond for the changes to take effect. Verify that pruning is complete from the logs.
```
2024-09-04T16:43:11Z ERRR Failed to prune blocks module=state err=failed to prune blocks to height 342736: cannot prune to height 342736, it is lower than base height 3736736 targetRetainHeight=342736 newRetain
```

### Optimizations
* Snapshotting, especially on full nodes, can run for several hours. The longer your node is down, the more time it needs afterward to catch up to the current state. You can minimize the downtime by decoupling the archival process from the node's operation. For example:
  * Create a copy of the data directory, start your node again and then work on compressing the snapshot while the node is still running.
  * If running your node on cloud or kubernetes infrastructure, use thier in-built snapshot mechanisms to create a copy with beacond shut down. This way, your node will be down for a few seconds at the most.

## Restore snapshot
Shut down your node - it should not be writing any new data (which could render your snapshot restore corrupt). 

Verify that the process isn't running:
```
ps aux|grep beacond
```

Out of an abdundance of caution, make sure your private key (`$BEACOND_HOME/data/priv_validator_key.json`) is backed up before proceeding.

Reset the beacond database:
```
$ beacond comet unsafe-reset-all
I[2024-09-04|17:00:57.137] Removed all blockchain history               dir=/root/.beacond/data
I[2024-09-04|17:00:57.137] Reset private validator file to genesis state keyFile=/root/.beacond/config/priv_validator_key.json stateFile=/root/.beacond/data/priv_validator_state.json
```

Restore your archive and replace the data directory:
```
cd $BEACOND_HOME/
rm -rf data
lz4 -dc < snapshot.tar.lz4 | tar xvf -
ls -l data
```

### Optimizations
* Verify the age of the snapshot before attempting to restore - eg. if you restore a day old snapshot, the node will still spend a fair amount of time in catching up to the current moment.
* Download snapshots for a location that's geographically close to your node and if possible on the same network. Eg. AWS -> AWS, or North America -> North America will be quicker.
* Corrupt downloads - utilize checksums if available, to verify thier integrity before you start restoring them - `sha256sum snapshot.tar.lz4`
* Use multi-connection downloads to speed up the process. For instance, [aria2c](https://aria2.github.io) is a tool that can be used to set up programatic download operations.

## Watch Out!

* Some users have reported that it is safer to copy `priv_validator_state.json` before restoring snapshots, as they've observed the node state to become inconsistent later on.
* If your snapshot restore has gone awry, you may encounter this message in the logs after the node starts up.
 *  `ERROR startup failure error="genesis doc hash in db does not match loaded genesis doc`
 *  Run `beacond comet unsafe-reset-all` and try restoring again.
* Do not include your private key or jwt.hex with the snapshot - these are secrets to be safely guarded, and not shared with others.

`priv_validator_state.json` - Indicates chain state. Looks like so:
```
{
  "height": "111",
  "round": 11,
  "step": 111,
  "signature": "xxxxxxxx",
  "signbytes": "xxxxxxxxxxx"
}
```

`priv_validator_key.json` - Your (safely guarded) key pair used for signing transactions. Looks like so:
```
{
  "address": "xxxxxxxxxxxxxxxx",
  "pub_key": {
    "type": "cometbft/PubKeyBls12_381",
    "value": "xxxxxxxxxxxxxxxx"
  },
  "priv_key": {
    "type": "cometbft/PrivKeyBls12_381",
    "value": "xxxxxxxxxxxxxxxxxxxxx"
  }
}
```

`jwt.hex` is used for auth between the beacon and execution client, can be changed peridically for better security (ensure that both your beacond and execution config reflects this). Please note that the JWT must start with `0x`.
