# Dymension

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/dymension.png" alt="" width="150"><figcaption></figcaption></figure>

Dymension is a home for easily deployable and lightning fast app-chains, called RollApps.

**Chain ID**: dymension_1100-1 | **Latest Version Tag**: v3.1.0 | **Wasm**: OFF

[Website](https://dymension.xyz/) | [Discord](https://discord.gg/dymension) | [Twitter](https://twitter.com/dymensionXYZ)

## Chain explorer

[https://explorer.takeshi.team/dymension](https://explorer.takeshi.team/dymension)

## Public endpoints

* api: [https://dymension-api.takeshi.team](https://dymension-api.takeshi.team)
* rpc: [https://dymension-rpc.takeshi.team](https://dymension-rpc.takeshi.team)
* grpc: [https://dymension-grpc.takeshi.team](https://dymension-grpc.takeshi.team)

## Peering

**state-sync**

```
d5519e378247dfb61dfe90652d1fe3e2b3005a5b@dymension-rpc.takeshi.team:443
```

**seed-node**

```
a85a651a3cf1746694560c5b6f76d566c04ca581@dymension-seed.takeshi.team:443
```

**addrbook**

{% code overflow="wrap" %}
```bash
curl -Ls https://snapshots.takeshi.team/dymension-testnet/addrbook.json > $HOME/.dymension/config/addrbook.json
```
{% endcode %}

**live-peers**

{% code overflow="wrap" %}
```bash
peers="09b1a88148c16a3cc629b7cfc12fb369d7a3399a@65.108.233.90:26656,39c335604e9e9323eb177ef8c33f8ab4a4317498@85.215.125.37:26656,fb7a8f69270a7de8a3c1b1e79e194a407d305c63@84.203.117.234:26691"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.dymension/config/config.toml
```
{% endcode %}
