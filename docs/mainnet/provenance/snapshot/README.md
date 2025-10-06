---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenanced_logo_name.png" alt="" width="150"><figcaption></figcaption></figure>

{% hint style="info" %}
Snapshots allows a new node to join the network by recovering application state from a backup file. Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 4 hours

**Instructions**

```bash
cd $HOME
sudo apt install -y lz4 wget  # if needed
```

**Stop the node**

```bash
sudo systemctl stop provenanced
```

**Backup validator**

{% code overflow="wrap" %}
```bash
sudo cp $HOME/.provenanced/data/priv_validator_state.json $HOME/.provenanced/priv_validator_state.json.backup
```
{% endcode %}

**Remove old data**

```bash
sudo rm -rf $HOME/.provenanced/data
sudo rm -rf $HOME/.provenanced/wasm
```

**Download and extract the latest snapshot**

{% code overflow="wrap" %}
```bash
curl -L https://snapshots.takeshi.team/provenance/provenance_latest.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.provenanced
```
{% endcode %}

**Restore validator**

{% code overflow="wrap" %}
```bash
mv $HOME/.provenanced/priv_validator_state.json.backup $HOME/.provenanced/data/priv_validator_state.json
```
{% endcode %}

**Download addrbook.json**

{% code overflow="wrap" %}
```bash
wget -O $HOME/.provenanced/config/addrbook.json "https://snapshots.takeshi.team/provenance/addrbook.json"
```
{% endcode %}

**Start the node**

{% code overflow="wrap" %}
```bash
sudo systemctl start provenanced && sudo journalctl -u provenanced -f --no-hostname -o cat
```
{% endcode %}
