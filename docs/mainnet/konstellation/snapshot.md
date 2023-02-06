---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/jackal.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Snapshots allows a new node to join the network by recovering application state from a backup file. Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **00:45 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v1.1.2-hotfix

| BLOCK   | AGE     | DOWNLOAD                                                                             |
| ------- | ------- | ------------------------------------------------------------------------------------ |
| 1330988 | 2 hours | [snapshot (0.99 GB)](https://snapshots.takeshi.team/jackal/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop canined
cp $HOME/.canine/data/priv_validator_state.json $HOME/.canine/priv_validator_state.json.backup
rm -rf $HOME/.canine/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.takeshi.team/jackal/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.canine
mv $HOME/.canine/priv_validator_state.json.backup $HOME/.canine/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start canined && sudo journalctl -u canined -f --no-hostname -o cat
```