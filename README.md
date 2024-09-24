# Awesome Berachain validators [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Collection of Berachain validators resources

## Contents
[Monitoring](#monitoring)  
[Proof of Liquidity](#pol)  
[Data](#data)  
[Snapshots](#snapshots)  
[FAQ](#faq)  

## Monitoring

* [Tenderduty](https://github.com/blockpane/tenderduty) - Comprehensive monitoring tool for Tendermint chains. Its primary function is to alert a validator if they are missing blocks, and more.
* [cosmos-validator-watcher](https://github.com/kilnfi/cosmos-validator-watcher) - Real-time Cosmos-based chains monitoring.

## PoL

* [Synergynode cutting board tool](https://beratools.synergynodes.com/) - Simple UI to manage your Berachef cutting board.
* [Kodiak cutting board tool](https://cutting-board.beraden.com/) - Simple UI to manage your Berachef cutting board.

## Data
* [BGTscan](https://bgtscan.com/) - BGT rewards dashboard
* [Nodes map](https://services.tienthuattoan.com/testnet/berachain-v2/map) - Berachain v2 nodes map
* [B-Harvest dashboard](https://bera-dashboard.bharvest.io/) - Validators data by B-Harvest

## Snapshots

| Provider  | URL | Database |
|-----------|-----|----------|
| Imperator   | https://www.imperator.co/services/chain-services/testnets/bera-v2 | goleveldb  |
| Blacknodes  | https://services.blacknodes.net/Berachain-V2/ | pebbledb   |
| Contribution DAO  | https://services.contributiondao.com/testnet/berachain-v2/snapshots | pebbledb   |
| Staketab| https://services.staketab.org/docs/beacon-testnet/snapshot | pebbledb |
| A41 | https://narrow-cello-dab.notion.site/A41-Berachain-Testnet-v2-Snapshots-4d39b8e7046e4fc8bce4ce9cf5053b97 | pebbledb |
| Synergy Nodes | https://synergynodes.com/service/berachain-v2-testnet | pebbledb |
| L0vd services | https://chain-services.l0vd.com/testnets/berachain_v2/snapshot#sync-from-snapshot-pebbledb | pebbledb |
| TTT VN | https://services.tienthuattoan.com/testnet/berachain-v2/snapshot | pebbledb |
| Brightlystake | https://testnet-berav2.brightlystake.com/snapshot/ | pebbledb |
| Blockhunters | https://blockhunters.dev/testnet/berachainv2/snapshot | goleveldb |

## FAQ
* **How do I address missed blocks on my validator?**
    * Check disk I/O metrics, make sure your node can keep up.
    * NVMe storage is highly recomended over SSDs for better performance.
    * The CPU is another possible bottleneck. Some users have reported that overclocking has helped dissipate the problem.
    * For validators on AWS,`i4i.2xlarge` is a good example of machine spec which has been reported to work without issues.
    * For more context, [please see this relevant discussion](https://discord.com/channels/924442927399313448/1245159849986228284/1273677626615009443).
* **My BGT rewards are delayed or missing!**
  * Please review your cutting board, ensure that the weights are setup exactly the way you expect them.
  * You can use any of [these tools](#pol) to make the job easy.
  * Currently the rewards distribution is less centralized and (very rarely) likely to be disrupted. The berachain team will continue to monitor and fix any problems, but expect more decentralization (and more resileince) in the future.
  * If you updated your cutting board and are worried about the status of the update, you can check the current status through a [contract call](https://docs.berachain.com/developers/contracts/berachef). Please see discussion [here](https://discord.com/channels/924442927399313448/1245159849986228284/1275152865089949781).
* **I have questions on snapshots?**
  * More information [here](snapshots.md).

## Contributing

Your contributions are welcome! Please feel free to submit a Pull Request.

### Changelog

* 2024-09-02: Initial release
