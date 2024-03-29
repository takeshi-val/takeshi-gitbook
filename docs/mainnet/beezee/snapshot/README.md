---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/beezee.png" alt="" width="150"><figcaption></figcaption></figure>

{% hint style="info" %}
Snapshots allows a new node to join the network by recovering application state from a backup file. Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **06:00 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v1.0.0

| BLOCK | AGE     | DOWNLOAD                                                                             |
| ----- | ------- | ------------------------------------------------------------------------------------ |
| 8129  | 3 hours | [snapshot (0.25 GB)](https://snapshots.takeshi.team/beezee/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop bzed
cp $HOME/.bze/data/priv_validator_state.json $HOME/.bze/priv_validator_state.json.backup
rm -rf $HOME/.bze/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.takeshi.team/beezee/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.bze
mv $HOME/.bze/priv_validator_state.json.backup $HOME/.bze/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start bzed && sudo journalctl -u bzed -f --no-hostname -o cat
```
