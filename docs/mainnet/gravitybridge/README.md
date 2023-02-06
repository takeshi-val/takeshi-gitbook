# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/gravitybridge.png" width="150" alt=""><figcaption></figcaption></figure>

An open, decentralized bridge that unlocks the power of  interoperability & liquidity between blockchain ecosystems.

**Chain ID**: gravity-bridge-3 | **Latest Version Tag**: v1.8.1 | **Wasm**: OFF

[Website](https://www.gravitybridge.net) | [Discord](https://discord.gg/ARV8dTSjAk) | [Twitter](https://twitter.com/gravity_bridge)

## Chain explorer
[https://explorer.takeshi.team/gravitybridge](https://explorer.takeshi.team/gravitybridge)

## Public endpoints

* api: [https://gravitybridge.api.takeshi.team](https://gravitybridge.api.takeshi.team)
* rpc: [https://gravitybridge.rpc.takeshi.team](https://gravitybridge.rpc.takeshi.team)
* grpc: [https://gravitybridge.grpc.takeshi.team](https://gravitybridge.grpc.takeshi.team)

## Peering

**state-sync**

```text
d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@gravitybridge.rpc.takeshi.team:26656
```

**seed-node**

```text
400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@gravitybridge.rpc.takeshi.team:26659
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/gravitybridge/addrbook.json > $HOME/.gravity/config/addrbook.json
```

**live-peers** (7)
```bash
peers="d20fb90c25dcd447fc574d20c3511a05b19aa9a5@35.215.12.41:26656,9a8c4af7574a5d1fcd5e89f755348c7b6df3b4be@142.132.158.93:14256,decc9e5b4f785a5b0b2cb6c0fe5b341ebc5d7211@136.244.112.224:26656,46374f308b7cbf6a8d8242bad8666760b433cb9d@62.171.164.145:26656,d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@65.109.88.38:26656,ca9d9d0605f178fbba3bdf92e13719ab9dce0fc7@23.88.59.82:26656,df243a4c65b436fb4c81bf71b83ce9de865fea5a@213.239.207.165:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.gravity/config/config.toml
```
