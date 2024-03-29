# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/hypersign.png" width="150" alt=""><figcaption></figcaption></figure>

Hypersign is a decentralized identity layer for the internet, giving  users control of their personal data and identity whilst digital  enabling trust for businesses.

**Chain ID**: prajna-1 | **Latest Version Tag**: v0.2.0 | **Wasm**: OFF

[Website](https://hypersign.id) | [Discord](https://discord.gg/DmuUjMrHVw) | [Twitter](https://twitter.com/hypersignchain)




## Chain explorer
[https://explorer.takeshi.team/hypersign-testnet](https://explorer.takeshi.team/hypersign-testnet)

## Public endpoints

* api: [https://hypersign-testnet.api.takeshi.team](https://hypersign-testnet.api.takeshi.team)
* rpc: [https://hypersign-testnet.rpc.takeshi.team](https://hypersign-testnet.rpc.takeshi.team)
* grpc: [https://hypersign-testnet.grpc.takeshi.team](https://hypersign-testnet.grpc.takeshi.team)

## Peering

**state-sync**

```text
d5519e378247dfb61dfe90652d1fe3e2b3005a5b@hypersign-testnet.rpc.takeshi.team:31656
```

**seed-node**

```text
a85a651a3cf1746694560c5b6f76d566c04ca581@hypersign-testnet.rpc.takeshi.team:31659
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/hypersign-testnet/addrbook.json > $HOME/.hid-node/config/addrbook.json
```

**live-peers** (10)
```bash
peers="23eff008c88dcc60ef9a71f2fb469c472679c35e@136.243.88.91:5040,5b4482bfe02384184470070c3d3a4465cf0c18d4@144.91.82.61:31656,0c6758a3f4554bbc67da73993bbb697764c5c534@38.242.142.227:26656,3a9defcd334cefd6b8143ec1ecd8be5e51f1c1c5@95.214.53.46:46656,efcb16ec33d8e6233d1068fff679c6fd64bf5802@65.108.225.158:10956,c1b6d86f46eab9d0aa2e4399cddb9cf05d13621a@65.108.206.118:60556,2641ddcf28d8adf448edb573de1efba0b6971d9e@178.154.222.128:26656,ec5127072c252f7246fb66f7e7762423a23ff6bd@154.12.228.93:31656,d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:31656,610843eda2f0388cb8e75917e8c1f63350bd3bd1@154.26.131.130:16656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.hid-node/config/config.toml
```
