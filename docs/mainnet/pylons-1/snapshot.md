---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/rebus.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Snapshots allows a new node to join the network by recovering application state from a backup file. Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **02:00 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v0.2.0

| BLOCK   | AGE     | DOWNLOAD                                                                            |
| ------- | ------- | ----------------------------------------------------------------------------------- |
| 4106514 | 1 hours | [snapshot (1.02 GB)](https://snapshots.takeshi.team/rebus/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop rebusd
cp $HOME/.rebusd/data/priv_validator_state.json $HOME/.rebusd/priv_validator_state.json.backup
rm -rf $HOME/.rebusd/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.takeshi.team/rebus/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.rebusd
mv $HOME/.rebusd/priv_validator_state.json.backup $HOME/.rebusd/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start rebusd && sudo journalctl -u rebusd -f --no-hostname -o cat
```
