---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/cosmoshub.png" width="150" alt=""><figcaption></figcaption></figure>

{% hint style='info' %}
Snapshots allows a new node to join the network by recovering application state from a backup file. 
Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, 
snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **04:45 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v7-Theta

| BLOCK             | AGE             | DOWNLOAD                                                                                            |
| ----------------- | --------------- | --------------------------------------------------------------------------------------------------- |
| 13900456 | 4 hours | [snapshot (3.7 GB)](https://snapshots.takeshi.team/cosmoshub/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop c4ed
cp $HOME/.gaia/data/priv_validator_state.json $HOME/.gaia/priv_validator_state.json.backup
rm -rf $HOME/.gaia/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.takeshi.team/cosmoshub/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.gaia
mv $HOME/.gaia/priv_validator_state.json.backup $HOME/.gaia/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start c4ed && sudo journalctl -u c4ed -f --no-hostname -o cat
```
