---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/composable.png" alt=""><figcaption></figcaption></figure>

{% hint style='info' %}
Snapshots allows a new node to join the network by recovering application state from a backup file. 
Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, 
snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **10:15 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v2.3.5

| BLOCK             | AGE             | DOWNLOAD                                                                                            |
| ----------------- | --------------- | --------------------------------------------------------------------------------------------------- |
| 140848 | 22 minutes | [snapshot (0.2 GB)](https://snapshots.takeshi.team/composable/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop banksyd
cp $HOME/.banksy/data/priv_validator_state.json $HOME/.banksy/priv_validator_state.json.backup
rm -rf $HOME/.banksy/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.takeshi.team/composable/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.banksy
mv $HOME/.banksy/priv_validator_state.json.backup $HOME/.banksy/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start banksyd && sudo journalctl -u banksyd -f --no-hostname -o cat
```
