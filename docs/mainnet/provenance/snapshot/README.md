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
| 1330988 | 2 hours | [snapshot (0.99 GB)](https://snapshots.takeshi.team/provenance/provenance_latest.tar.lz4) |

## Instructions

cd $HOME
apt install -y lz4 wget  # if needed`

**Stop the node**
`sudo systemctl stop provenanced`

**Backup validator**
`cp $HOME/.provenanced/data/priv_validator_state.json $HOME/.provenanced/priv_validator_state.json.backup`

**Remove old data**
'rm -rf $HOME/.provenanced/data'
'rm -rf $HOME/.provenanced/wasm'

**Download and extract the latest snapshot**
`curl -L https://snapshots.takeshi.team/provenance/provenance_latest.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.provenanced`

**Restore validator**
`mv $HOME/.provenanced/priv_validator_state.json.backup $HOME/.provenanced/data/priv_validator_state.json`

**Download addrbook.json**
`wget -O $HOME/.provenanced/config/addrbook.json "https://snapshots.takeshi.team/provenance/addrbook.json"`

**Start the node**
`sudo systemctl start provenanced && journalctl -fu provenanced -o cat`

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
