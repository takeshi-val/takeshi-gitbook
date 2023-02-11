---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/canto.png" width="150" alt=""><figcaption></figcaption></figure>

{% hint style='info' %}
Snapshots allows a new node to join the network by recovering application state from a backup file. 
Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, 
snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 6 hours starting at **05:15 UTC**

**pruning**: 100/0/19 | **indexer**: null | **version tag**: trichomemonster-ica

| BLOCK             | AGE             | DOWNLOAD                                                                                            |
| ----------------- | --------------- | --------------------------------------------------------------------------------------------------- |
| 7037250 | 4 hours | [snapshot (0.79 GB)](https://snapshots.takeshi.team/canto/snapshot\_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop cantod
cp $HOME/.cantod/data/priv_validator_state.json $HOME/.cantod/priv_validator_state.json.backup
rm -rf $HOME/.cantod/data
```

### Download latest snapshot

```bash
curl -L https://snapshots.takeshi.team/canto/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.cantod
mv $HOME/.cantod/priv_validator_state.json.backup $HOME/.cantod/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start cantod && sudo journalctl -u cantod -f --no-hostname -o cat
```
