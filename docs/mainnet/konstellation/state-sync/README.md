---
description: >-
  With our state sync services you will be able to catch up latest chain block
  in matter of minutes
---

# State sync

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/konstellation.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
State Sync allows a new node to join the network by fetching a snapshot of the application state at a recent height instead of fetching and replaying all historical blocks. Since the application state is generally much smaller than the blocks, and restoring it is much faster than replaying blocks, this can reduce the time to sync with the network from days to minutes.
{% endhint %}

## Instructions

### Stop the service and reset the data

{% code overflow="wrap" %}
```bash
sudo systemctl stop knstld
cp $HOME/.knstld/data/priv_validator_state.json $HOME/.knstld/priv_validator_state.json.backup
knstld tendermint unsafe-reset-all --home $HOME/.knstld
```
{% endcode %}

### Get and configure the state sync information

{% code overflow="wrap" %}
```bash
STATE_SYNC_RPC=https://konstellation.rpc.takeshi.team:443
STATE_SYNC_PEER=d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@konstellation.rpc.takeshi.team:13656
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 2000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i \
  -e "s|^enable *=.*|enable = true|" \
  -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.knstld/config/config.toml

mv $HOME/.knstld/priv_validator_state.json.backup $HOME/.knstld/data/priv_validator_state.json
```
{% endcode %}

### Download latest wasm

{% hint style="info" %}
Currently state sync does not support copy of the `wasm` folder. Therefore, you will have to download it manually.
{% endhint %}

{% code overflow="wrap" %}
```bash
curl -L https://snapshots.takeshi.team/konstellation/wasm_latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.knstld
```
{% endcode %}

### Restart the service and check the log

```bash
sudo systemctl start knstld && sudo journalctl -u knstld -f --no-hostname -o cat
```
