---
description: >-
  With our state sync services you will be able to catch up latest chain block
  in matter of minutes
---

# State sync

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenanced_logo_name.png" alt="" width="150"><figcaption></figcaption></figure>

{% hint style="info" %}
State Sync allows a new node to join the network by fetching a snapshot of the application state at a recent height instead of fetching and replaying all historical blocks. Since the application state is generally much smaller than the blocks, and restoring it is much faster than replaying blocks, this can reduce the time to sync with the network from days to minutes.
{% endhint %}

## Instructions

### Stop the service and reset the data

{% code overflow="wrap" %}
```bash
sudo systemctl stop provenanced
cp $HOME/.provenance/data/priv_validator_state.json $HOME/.provenance/priv_validator_state.json.backup
provenanced tendermint unsafe-reset-all --home $HOME/.provenance
```
{% endcode %}

### Get and configure the state sync information

{% code overflow="wrap" %}
```bash
STATE_SYNC_RPC=https://rpc-provenance.takeshi.team:443
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 2000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)
echo $LATEST_HEIGHT $SYNC_BLOCK_HEIGHT $SYNC_BLOCK_HASH

sed -i \
  -e "s|^enable *=.*|enable = true|" \
  -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.provenance/config/config.toml

mv $HOME/.provenance/priv_validator_state.json.backup $HOME/.provenance/data/priv_validator_state.json
```
{% endcode %}

### Download latest wasm

{% hint style="info" %}
Currently state sync does not support copy of the `wasm` folder. Therefore, you will have to download it manually.
{% endhint %}

{% code overflow="wrap" %}
```bash
curl -L https://snapshots.takeshi.team/provenance/wasm_latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.provenance
```
{% endcode %}

### Restart the service and check the log

```bash
sudo systemctl start provenanced && sudo journalctl -u provenanced -f 
```
