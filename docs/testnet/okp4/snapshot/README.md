---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://raw.githubusercontent.com/kj89/testnet_manuals/main/pingpub/logos/okp4.png" width="150" alt=""><figcaption></figcaption></figure>

{% hint style='info' %}
Snapshots allows a new node to join the network by recovering application state from a backup file. 
Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, 
snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **03:00 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v3.0.0

| BLOCK             | AGE             | DOWNLOAD                                                                                            |
| ----------------- | --------------- | --------------------------------------------------------------------------------------------------- |
| 720729 | 25 minutes | [snapshot (0.63 GB)](https://snapshots.kjnodes.com/okp4-testnet/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop okp4d
cp $HOME/.okp4d/data/priv_validator_state.json $HOME/.okp4d/priv_validator_state.json.backup
rm -rf $HOME/.okp4d/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.kjnodes.com/okp4-testnet/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.okp4d
mv $HOME/.okp4d/priv_validator_state.json.backup $HOME/.okp4d/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start okp4d && sudo journalctl -u okp4d -f --no-hostname -o cat
```
