# Guide to Snapshotting

## What is a snapshot?
* Collection of files from a node's backend, representing its state at the point of time when it was captured.
* Useful for syncing nodes that have failed, corrupted, slowed and need to catch up, or even spinning up a new node from genesis.
* _Types_
  * Archive snapshot - Full history of the blockchain - used in full/archive nodes (massive - size can run into hundreds of GB)
  * Pruned snapshot - Contains history of the last few states. Useful for quickly setting up validators.
* Snapshots can be applied to both the consensus (beacond) and execution (geth, reth, etc.) clients. In fact, syncing can be a lot faster when both snapshots are simultaneously restored. While this document focusses on beacond snapshots, you can refer to other resources related to your execution client.
  * Example - [snapshotting on reth](https://snapshots.merkle.io/). While ETH mainnet is used in this example, the same process applies to berachain as well.
* beacond snapshots are specific to the database being used 

## Create snapshot
Shut down your node - it should not be writing any new data (which could render your snapshot corrupt). 

Verify that the process isn't running
```
ps aux|grep beacond
```

Create a snapshot archive from the data directory.
```cd $BEACOND_HOME
tar cvf - data | lz4 > snapshot.tar.lz4
```

### Pruning
Set the pruning parameters in `config/app.toml` to avoid retaining the entire chain state. 

```
pruning = "everything"

pruning-keep-recent = "1000"
pruning-interval = "10"
```

Restart beacond for the changes to take effect. Verify that the pruning is complete from the logs.
```
2024-09-04T16:43:11Z ERRR Failed to prune blocks module=state err=failed to prune blocks to height 342736: cannot prune to height 342736, it is lower than base height 3736736 targetRetainHeight=342736 newRetain
```

### Optimizations
* Snapshotting, espcially on full nodes can run for several hours. The longer your node is down, the more time it needs afterword to catch up to the current state. You can minimize this time by decoupling the archival process from the node's downtime. For example:
  * Create a copy of the data directory, start your node again and then work on compressing the snapshot.
  * If running your node on cloud or kubernetes infrastructure, use thier inbuilt snapshot mechanisms to create a copy wigth beacond shut down. This way, your node will be down for a few seconds at the most.

## Restore snapshot
Shut down your node - it should not be writing any new data (which could render your snapshot restore corrupt). 

Verify that the process isn't running
```
ps aux|grep beacond
```

**Backup your private key before proceeding** - `$BEACOND_HOME/data/priv_validator_key.json`

Reset the beacond database
```
$ beacond comet unsafe-reset-all
I[2024-09-04|17:00:57.137] Removed all blockchain history               dir=/root/.beacond/data
I[2024-09-04|17:00:57.137] Reset private validator file to genesis state keyFile=/Users/root/.beacond/config/priv_validator_key.json stateFile=/Users/root/.beacond/data/priv_validator_state.json
```

Restore your private key
```
cp /backup/priv_validator_key.json $BEACON_HOME/config/
```


### Optimizations
* Verify the age of the snapshot before attempting to restore - if you restore a day old snapshot, the node will still spend a fair amount of time in catching up to the current moment.
* Download snapshots for a location that's geographically close to your node and if possible on the same network. Eg AWS -> AWS, North America -> Noth America
* Corrupted downloads - utilize checksums if available, to verify thier integrity before you start restoring them.

## Watch Out!

* Some users have reported that it is better to backup `priv_validator_state.json` before restoring snapshots, as they've observed the node state to become inconsistent later on.
* Do not include your private key or jwt.hex with the snapshot - these are secrets to be safely gaurded, and not shared with others.

`priv_validator_state.json` - Chain state. Looks like so:
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

`jwt.hex` is used for auth between e thbeacon and execution client, can be changed peridically for better security (ensure that both your beacond and execution config reflects this). Please note that the JWT must start with `0x`.
