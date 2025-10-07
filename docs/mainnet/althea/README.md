# Althea

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/althea.png" alt=""><figcaption></figcaption></figure>

Althea's unique cooperative vision for the internet brings peering from the data center to the field

**Chain ID**: althea\_258432-1 | **Latest Version Tag**: v1.4.0 | **Wasm**: OFF

[Website](https://www.althea.net) | [Discord](https://discord.gg/ZTKWfpDs) | [Twitter](https://twitter.com/altheanetwork)

## Chain explorer

[https://explorer.takeshi.team/althea-mainnet](https://explorer.takeshi.team/althea-mainnet)

## Public endpoints

* api: [https://althea-api.takeshi.team](https://althea-api.takeshi.team)
* rpc: [https://althea-rpc.takeshi.team](https://althea-rpc.takeshi.team)
* grpc: althea-grpc.takeshi.team:443

## Peering

**seed-node**

```
a85a651a3cf1746694560c5b6f76d566c04ca581@althea-mainnet.rpc.takeshi.team:15259
```

**addrbook**

```bash
curl -Ls https://snapshots.takeshi.team/althea-testnet/addrbook.json > $HOME/.althea/config/addrbook.json
```

**live-peers** (38)

{% code overflow="wrap" %}
```bash
peers="54ab55ca3c527c26abd208ec53cbaceb0fd14799@168.119.226.107:43556,0168344b6baa76490b0ce4f7260f7211d115bfcd@167.235.180.105:26656,4d9c73a9e541453b56add8fadf0839fd1442d979@15.235.115.155:17200,a0eca501485cc74e0568973ef502d05023f6500d@158.247.226.255:17200,ab9a9e6ea747839652dfe4480e66a5eb78a385e8@51.81.167.60:17200,46ad21a616527181ea3d992339268a5a25c771fa@95.216.38.96:14656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.althea/config/config.toml
```
{% endcode %}
