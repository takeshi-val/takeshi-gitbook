---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/logo_C4E.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Snapshots allows a new node to join the network by recovering application state from a backup file. Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **01:15 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v1.1.0

| BLOCK   | AGE     | DOWNLOAD                                                                             |
| ------- | ------- | ------------------------------------------------------------------------------------ |
| 1812232 | 2 hours | [snapshot (1.0 GB)](https://snapshots.kjnodes.com/teritori/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop c4ed
cp $HOME/.c4e-chain/data/priv_validator_state.json $HOME/.c4e-chain/priv_validator_state.json.backup
rm -rf $HOME/.c4e-chain/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.kjnodes.com/teritori/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.c4e-chain
mv $HOME/.c4e-chain/priv_validator_state.json.backup $HOME/.c4e-chain/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start c4ed && sudo journalctl -u c4ed -f --no-hostname -o cat
```
