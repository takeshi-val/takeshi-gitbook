# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/${PROJECT_NAME}.png" width="150" alt=""><figcaption></figcaption></figure>

${CHAIN_SHORT_DESCRIPTION}
**Chain ID**: ${CHAIN_ID} | **Latest Version Tag**: ${LATEST_VERSION_TAG} | **Wasm**: ${CHAIN_WASM}

[Website](${CHAIN_WEBSITE}) | [Discord](${CHAIN_DISCORD}) | [Twitter](${CHAIN_TWITTER})

${STAKE_BUTTON}

${RESTAKE}
## Chain explorer
[https://explorer.takeshi.team/${CHAIN_SERVICE}](https://explorer.takeshi.team/${CHAIN_SERVICE})

## Public endpoints

* api: [https://${CHAIN_NAME}.api.takeshi.team](https://${CHAIN_NAME}.api.takeshi.team)
* rpc: [https://${CHAIN_NAME}.rpc.takeshi.team](https://${CHAIN_NAME}.rpc.takeshi.team)
* grpc: [https://${CHAIN_NAME}.grpc.takeshi.team](https://${CHAIN_NAME}.grpc.takeshi.team)

## Peering

**state-sync**

```text
${CHAIN_PEER}@${CHAIN_NAME}.rpc.takeshi.team:${CHAIN_PORT}656
```

**seed-node**

```text
${CHAIN_TENDERSEED_PEER}@${CHAIN_NAME}.rpc.takeshi.team:${CHAIN_PORT}659
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/${CHAIN_NAME}/addrbook.json > $HOME/${CHAIN_DIR}/config/addrbook.json
```

**live-peers** (${CHAIN_LIVE_PEERS_COUNT})
```bash
peers="${CHAIN_LIVE_PEERS}"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/${CHAIN_DIR}/config/config.toml
```
