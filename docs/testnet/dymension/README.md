# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/dymension.png" width="150" alt=""><figcaption></figcaption></figure>

Dymension is a home for easily deployable and lightning fast app-chains, called RollApps.

**Chain ID**: 35-C | **Latest Version Tag**: v0.2.0-beta | **Wasm**: OFF

[Website](https://dymension.xyz/) | [Discord](https://discord.gg/dymension) | [Twitter](https://twitter.com/dymensionXYZ)


## Chain explorer
[https://explorer.takeshi.team/dymension-testnet](https://explorer.takeshi.team/dymension-testnet)

## Public endpoints

* api: [https://dymension-testnet.api.takeshi.team](https://dym-api.takeshi.team)
* rpc: [https://dymension-testnet.rpc.takeshi.team](https://dym-rpc.takeshi.team)
* grpc: [https://dymension-testnet.grpc.takeshi.team](https://dym-grpc.takeshi.team)

## Peering

**state-sync**

```text
d5519e378247dfb61dfe90652d1fe3e2b3005a5b@dym-rpc.takeshi.team:27656
```

**seed-node**

```text
a85a651a3cf1746694560c5b6f76d566c04ca581@dym-rpc.takeshi.team:27659
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/dymension-testnet/addrbook.json > $HOME/.dymension/config/addrbook.json
```

**live-peers** 
```bash
peers="b473a649e58b49bc62b557e94d35a2c8c0ee9375@95.214.53.46:36656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.dymension/config/config.toml
```
