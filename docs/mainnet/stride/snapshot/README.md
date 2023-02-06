---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://raw.githubusercontent.com/kj89/testnet_manuals/main/pingpub/logos/stride.png" width="150" alt=""><figcaption></figcaption></figure>

{% hint style='info' %}
Snapshots allows a new node to join the network by recovering application state from a backup file. 
Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, 
snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **01:30 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v5

| BLOCK             | AGE             | DOWNLOAD                                                                                            |
| ----------------- | --------------- | --------------------------------------------------------------------------------------------------- |
| 2174244 | 1 hours | [snapshot (5.08 GB)](https://snapshots.kjnodes.com/stride/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop strided
cp $HOME/.stride/data/priv_validator_state.json $HOME/.stride/priv_validator_state.json.backup
rm -rf $HOME/.stride/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.kjnodes.com/stride/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.stride
mv $HOME/.stride/priv_validator_state.json.backup $HOME/.stride/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start strided && sudo journalctl -u strided -f --no-hostname -o cat
```
