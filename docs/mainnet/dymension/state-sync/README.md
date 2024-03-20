---
description: With our state sync services you will be able to catch up latest chain block in matter of minutes
---

# State sync

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/dymension.png" width="150" alt=""><figcaption></figcaption></figure>

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop dymd
cp $HOME/.dymension/data/priv_validator_state.json $HOME/.dymension/priv_validator_state.json.backup
dymd tendermint unsafe-reset-all --keep-addr-book --home $HOME/.dymension
```

### Get and configure the state sync information

```bash
STATE_SYNC_RPC=https://dymension-rpc.takeshi.team:443
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 1000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)
echo $LATEST_HEIGHT $SYNC_BLOCK_HEIGHT $SYNC_BLOCK_HASH

sed -i \
  -e "s|^enable *=.*|enable = true|" \
  -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.dymension/config/config.toml

mv $HOME/.dymension/priv_validator_state.json.backup $HOME/.dymension/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start dymd && sudo journalctl -u dymd -f 
```