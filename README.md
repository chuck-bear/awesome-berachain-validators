# Awesome Berachain validators [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Collection of Berachain validators resources

## Contents
[Monitoring](#monitoring)  
[Proof of Liquidity](#pol)  
[Data](#data)  
[Snapshots](#snapshots)  
[FAQ](#faq)  

## Monitoring

* [Berachain Validator Monitoring](https://grafana.tools.berachain.com/public-dashboards/d2ebb1bef39846c194e9fb731526856f?orgId=1&from=1727808520612&to=1727851720612) - Public Dashboard
* [Tenderduty](https://github.com/blockpane/tenderduty) - Comprehensive monitoring tool for Tendermint chains. Its primary function is to alert a validator if they are missing blocks, and more.
* [cosmos-validator-watcher](https://github.com/kilnfi/cosmos-validator-watcher) - Real-time Cosmos-based chains monitoring. Berachain setup explained [in Discord](https://discord.com/channels/924442927399313448/1245159849986228284/1248576784392523817).
* [Execution blocks monitoring](https://grafana.tools.berachain.com/public-dashboards/0a5740500cc64e589e9e4e3c7ac03a87?orgId=1) - Public Dashboard 
* [Cosmostation: Cosmos Validator Monitoring Service (CVMS)](https://public-cosmos-vms.cosmostation.io/?orgId=1&refresh=1m&var-chain_id=bartio_beacon_80084&var-chain=bera&from=now-1h&to=now&timezone=browser) - Public Dashboard
* [Synergy Nodes: Performance Metrics](https://berachain-v2-testnet.synergynodes.com/) - Public Dashboard
* [Hyeongjun Yu: Grafana dashboard](https://grafana.com/grafana/dashboards/20305-berachain-validator-monitoring/)


## PoL

* BGT Station - list of [vaults](https://bartio.station.berachain.com/gauge), and [validators](https://bartio.station.berachain.com/validators) - Can sort the lists to see which validators and vaults have the highest APY. 
* [Synergynode cutting board tool](https://beratools.synergynodes.com/) - Simple UI to manage your Berachef cutting board.
* [Kodiak cutting board tool](https://cutting-board.beraden.com/) - Simple UI to manage your Berachef cutting board.

## Data
* [BGTscan](https://bgtscan.com/) - BGT rewards dashboard
* [Nodes map](https://services.tienthuattoan.com/testnet/berachain-v2/map) - Berachain v2 nodes map
* [B-Harvest dashboard](https://bera-dashboard.bharvest.io/) - Validators data by B-Harvest
* [Flipside: State of the Berachain Testnet](https://flipsidecrypto.xyz/Berateam/state-of-the-berachain-testnet-bww6-t?tabIndex=2) - Overall metrics for total number of users, transactions, etc. 

## Snapshots

| Provider                                                                                                        | Database  |
|-----------------------------------------------------------------------------------------------------------------|-----------|
| [A41](https://narrow-cello-dab.notion.site/A41-Berachain-Testnet-v2-Snapshots-4d39b8e7046e4fc8bce4ce9cf5053b97) | pebbledb  |
| [Allnodes](https://www.publicnode.com/snapshots#berachain)                                                      | goleveldb |
| [Berachain (Asia)](https://storage.googleapis.com/bartio-snapshot-as/index.html)                                | pebbledb  |
| [Berachain (Europe)](https://storage.googleapis.com/bartio-snapshot-eu/index.html)                              | pebbledb  |
| [Berachain (North America)](https://storage.googleapis.com/bartio-snapshot/index.html)                          | pebbledb  |
| [Blacknodes](https://services.blacknodes.net/Berachain-V2/)                                                     | pebbledb  |
| [Blockhunters](https://blockhunters.dev/testnet/berachainv2/snapshot)                                           | goleveldb |
| [Brightlystake](https://testnet-berav2.brightlystake.com/snapshot/)                                             | pebbledb  |
| [Contribution DAO](https://services.contributiondao.com/testnet/berachain-v2/snapshots)                         | pebbledb  |
| [Imperator](https://www.imperator.co/services/chain-services/testnets/bera-v2)                                  | goleveldb |
| [L0vd services](https://chain-services.l0vd.com/testnets/berachain_v2/snapshot#sync-from-snapshot-pebbledb)     | pebbledb  |
| [Staketab](https://services.staketab.org/docs/beacon-testnet/snapshot)                                          | pebbledb  |
| [Synergy Nodes](https://synergynodes.com/service/berachain-v2-testnet)                                          | pebbledb  |
| [TTT VN](https://services.tienthuattoan.com/testnet/berachain-v2/snapshot)                                      | pebbledb  |

## FAQ
* **What are some useful commands for troubleshooting?**
    * [See here](https://services.tienthuattoan.com/testnet/berachain-v2/useful-commands#maintenance) for a list of useful commands. 
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
* **I'm unable to update the cutting board - returns an error**
  * Please see above for tools that you can use to update your cutting board. You can also run trasanctions from the command-line (ex. `cast`)
  * Check the transaction details on the explorer - what does it say? For example, [this transaction ](https://bartio.beratrail.io/tx/0x0ad986617ee16ebb60b04b988d4aa845e817e3788702b716b949eeeb0986b17f) returned an error because the operator address was not registered.
  * To set the operator address, run: `cast send 0xfb81E39E3970076ab2693fA5C45A07Cc724C93c2 "setOperator(address)" 0xYOUROPERATORADDRESS --rpc-url ETH_RPC_URL --private-key YOURPRIVATEKEYWITHOUT0x`
  * Please make sure to read the [instructions](https://discord.com/channels/924442927399313448/1245528095553753169/1263560779115003956) very carefully - addresses, amounts, etc.
  * Vault addresses can be found on [BGT Station](https://bartio.station.berachain.com/gauge), labeled as “reward vault”, or on [Bartio Explorer](https://bartio.beratrail.io/) as “contract address”. 
  * Ensure you have set an execution block number sufficiently far in the future from the current execution block. (use the [Bartio Explorer](https://bartio.beratrail.io/blocks) to see the current block number)
  * To update cutting board, run:  
  `cast send 0xfb81E39E3970076ab2693fA5C45A07Cc724C93c2 \ # Berachef address`
  `"queueNewCuttingBoard(address,uint64,(address,uint96)[])" \`
  `$VALIDATOR_ADDRESS \ # Your execution-side validator address (suggested-fee-recipient)`
  `$FUTURE_BLOCK_NUMBER \ # The future block number you want to queue this cutting board to begin at`
  `'[(0xe5519D97eA854291c35a494b28929fA7abEf12e8,5000),(0x175e2429bCb92643255abCbCDF47Fff63F7990CC,5000)]' # The array of reward vault addresses + their % of BGT emissions from your cutting board`
* **How do I check for peering issues?**
  * Poor peering can result in your node falling behind and potentially missing out on rewards.
  * Keep an eye on your logs if your suspect any peering problems - these will usually show up as a "connection refused" message, "could not resolve host" etc.
  * Check for established connections to your node. ex. on linux `netstat -plant|grep ESTABLISHED`, which gives you the source addresses to verify peering.
  * See the number of peers: 
  `curl -sS localhost:26657/net_info| jq .result.n_peers`
  * Check the list of who is peering with you:
  `curl -sS http://localhost:26657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'`
  * Get a JSON output that is useful for debugging 
  `curl -sS localhost:26657/status | jq .`
  * [Check your geographic region](https://services.tienthuattoan.com/testnet/berachain-v2/map) for the availability of nearby peers; consider relocating to a different region.
  * Change your execution client - remember that peering can affect both the execution and consensus layers. Reth appears to have substantially more peers than geth.
  * Make sure that you have alerting set up on both layers to catch any problems proactively. More information was provided during the [monitoring office hours](https://discord.com/channels/924442927399313448/1245528095553753169/1270454196940046418).

## Contributing

Your contributions are welcome! Please feel free to submit a Pull Request.

### Changelog

* 2024-11-05: Additional info added by Max from H2O Nodes.
* 2024-08-02: Initial release
