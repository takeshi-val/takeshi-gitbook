# Services

<figure><img src="https://raw.githubusercontent.com/kj89/testnet_manuals/main/pingpub/logos/${PROJECT_NAME}.png" width="150" alt=""><figcaption></figcaption></figure>

${CHAIN_SHORT_DESCRIPTION}
**Chain ID**: ${CHAIN_ID} | **Latest Version Tag**: ${LATEST_VERSION_TAG} | **Wasm**: ${CHAIN_WASM}

[Website](${CHAIN_WEBSITE}) | [Discord](${CHAIN_DISCORD}) | [Twitter](${CHAIN_TWITTER})

${STAKE_BUTTON}

${RESTAKE}
## Chain explorer
[https://explorer.kjnodes.com/${CHAIN_SERVICE}](https://explorer.kjnodes.com/${CHAIN_SERVICE})

## Public endpoints

* api: [https://${CHAIN_NAME}.api.kjnodes.com](https://${CHAIN_NAME}.api.kjnodes.com)
* rpc: [https://${CHAIN_NAME}.rpc.kjnodes.com](https://${CHAIN_NAME}.rpc.kjnodes.com)
* grpc: [https://${CHAIN_NAME}.grpc.kjnodes.com](https://${CHAIN_NAME}.grpc.kjnodes.com)

## Peering

**state-sync**

```text
${CHAIN_PEER}@${CHAIN_NAME}.rpc.kjnodes.com:${CHAIN_PORT}656
```

**seed-node**

```text
${CHAIN_TENDERSEED_PEER}@${CHAIN_NAME}.rpc.kjnodes.com:${CHAIN_PORT}659
```

**addrbook**
```bash
curl -Ls https://snapshots.kjnodes.com/${CHAIN_NAME}/addrbook.json > $HOME/${CHAIN_DIR}/config/addrbook.json
```

**live-peers** (${CHAIN_LIVE_PEERS_COUNT})
```bash
peers="${CHAIN_LIVE_PEERS}"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/${CHAIN_DIR}/config/config.toml
```
