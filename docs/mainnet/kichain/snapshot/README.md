---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/kichain.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Snapshots allows a new node to join the network by recovering application state from a backup file. Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **04:15 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: kichain-upgrade-8

| BLOCK   | AGE     | DOWNLOAD                                                                              |
| ------- | ------- | ------------------------------------------------------------------------------------- |
| 8492808 | 5 hours | [snapshot (2.63 GB)](https://snapshots.takeshi.team/kichain/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop kid
cp $HOME/.kid/data/priv_validator_state.json $HOME/.kid/priv_validator_state.json.backup
rm -rf $HOME/.kid/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.takeshi.team/kichain/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.kid
mv $HOME/.kid/priv_validator_state.json.backup $HOME/.kid/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start kid && sudo journalctl -u kid -f --no-hostname -o cat
```
