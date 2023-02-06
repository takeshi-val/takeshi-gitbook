---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://raw.githubusercontent.com/kj89/testnet_manuals/main/pingpub/logos/kujira.png" width="150" alt=""><figcaption></figcaption></figure>

{% hint style='info' %}
Snapshots allows a new node to join the network by recovering application state from a backup file. 
Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, 
snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **00:15 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v0.7.1

| BLOCK             | AGE             | DOWNLOAD                                                                                            |
| ----------------- | --------------- | --------------------------------------------------------------------------------------------------- |
| 7733196 | 3 hours | [snapshot (1.78 GB)](https://snapshots.kjnodes.com/kujira/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop kujirad
cp $HOME/.kujira/data/priv_validator_state.json $HOME/.kujira/priv_validator_state.json.backup
rm -rf $HOME/.kujira/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.kjnodes.com/kujira/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.kujira
mv $HOME/.kujira/priv_validator_state.json.backup $HOME/.kujira/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start kujirad && sudo journalctl -u kujirad -f --no-hostname -o cat
```
