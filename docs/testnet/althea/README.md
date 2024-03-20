# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/althea.png" alt=""><figcaption></figcaption></figure>

Althea's unique cooperative vision for the internet  brings peering from the data center to the field

**Chain ID**: althea_417834-4 | **Latest Version Tag**: v1.0.0-rc2 | **Wasm**: OFF

[Website](https://www.althea.net) | [Discord](https://discord.gg/ZTKWfpDs) | [Twitter](https://twitter.com/altheanetwork)



## Chain explorer
[https://explorer.takeshi.team/althea-testnet](https://explorer.takeshi.team/althea-testnet)

## Public endpoints

* api: [https://althea-api.takeshi.team](https://althea-api.takeshi.team)
* rpc: [https://althea-rpc.takeshi.team](https://althea-rpc.takeshi.team)
* grpc: althea-grpc.takeshi.team:443

## Peering

**seed-node**

```text
a85a651a3cf1746694560c5b6f76d566c04ca581@althea-testnet.rpc.takeshi.team:15259
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/althea-testnet/addrbook.json > $HOME/.althea/config/addrbook.json
```

**live-peers** (38)
```bash
peers="ab9a9e6ea747839652dfe4480e66a5eb78a385e8@51.81.167.60:17200,5e7de4ca4f915ae00de3417129bd08ad04aa6bdc@65.21.172.60:36656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.althea/config/config.toml
```
