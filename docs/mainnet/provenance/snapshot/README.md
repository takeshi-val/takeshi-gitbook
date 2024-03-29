---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenance.png" width="150" alt=""><figcaption></figcaption></figure>

{% hint style='info' %}
Snapshots allows a new node to join the network by recovering application state from a backup file. 
Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, 
snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **00:45 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: v1.1.2-hotfix

| BLOCK             | AGE             | DOWNLOAD                                                                                            |
| ----------------- | --------------- | --------------------------------------------------------------------------------------------------- |
| 1330988 | 2 hours | [snapshot (0.99 GB)](https://snapshots.takeshi.team/provenance/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop provenanced
cp $HOME/.provenance/data/priv_validator_state.json $HOME/.provenance/priv_validator_state.json.backup
rm -rf $HOME/.provenance/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.takeshi.team/provenance/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.provenance
mv $HOME/.provenance/priv_validator_state.json.backup $HOME/.provenance/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start provenanced && sudo journalctl -u provenanced -f --no-hostname -o cat
```
