# BeeZee

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/beezee.png" alt=""><figcaption></figcaption></figure>

beezee is a credit protocol for the future: non-custodial, open-source, transparent, algorithmic and community-governed.

**Chain ID**: beezee-1 | **Latest Version Tag**: v7.2.1 | **Wasm**: OFF

[Website](https://beezeeprotocol.io) | [Discord](https://discord.gg/beezeeprotocol) | [Twitter](https://twitter.com/beezee_protocol)

## Chain explorer

[https://explorer.takeshi.team/beezee](https://explorer.takeshi.team/beezee)

## Public endpoints

* api: [https://beezee.api.takeshi.team](https://beezee.api.takeshi.team)
* rpc: [https://beezee.rpc.takeshi.team](https://beezee.rpc.takeshi.team)
* grpc: [https://beezee.grpc.takeshi.team](https://beezee.grpc.takeshi.team)

## Peering

**state-sync**

```
d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@beezee.rpc.takeshi.team:45656
```

**seed-node**

```
400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@beezee.rpc.takeshi.team:45659
```

**addrbook**

{% code overflow="wrap" %}
```svg
curl -Ls https://snapshots.takeshi.team/beezee/addrbook.json > $HOME/.bze/config/addrbook.json
```
{% endcode %}

**live-peers** (10)

{% code overflow="wrap" %}
```bash
peers="d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@65.109.88.38:45656,d10e5704f3c8e9dd6ef42445e4b88bb57d0a8289@65.108.8.247:18556,ced9f0d84104ce5fad53e91548ed9f7f16599d10@176.9.22.117:54656,76969af1bccdd4dcc511741b171c3d4ccb837ba6@146.59.85.223:18556,84f821d36d45cc0cdaa4ff05297e888bb0d9de8f@85.237.193.111:26656,7a6e1490d4b2d32b7e37d1e1cb35e143d2492517@51.79.159.79:16656,9cb92702727bc5f3d40154e625b9553a04f4d649@65.109.104.72:18556,9ab42d56b7cfd78eeed997b276dc7aec27374e42@65.109.52.156:10656,305d93229a89ae46265ef08536aa962d4a0dee67@65.108.131.18:26656,5ffee90e41903f6fba29dc75446d536a02d626fe@65.108.232.150:18095"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.bze/config/config.toml
```
{% endcode %}
