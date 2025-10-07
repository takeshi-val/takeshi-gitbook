# Konstellation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/konstellation.png" alt=""><figcaption></figcaption></figure>

konstellation is a Layer 1 protocol built on Cosmos, which can be used for hosting community-selected projects.

**Chain ID**: darchub | **Latest Version Tag**: v0.6.2

[Website](https://konstellation.app) | [Discord](https://discord.gg/teamkonstellation) | [Twitter](https://twitter.com/Teamkonstellation)

## Chain explorer

[https://explorer.takeshi.team/konstellation](https://explorer.takeshi.team/konstellation)

## Public endpoints

* api: [https://konstellation.api.takeshi.team](https://konstellation.api.takeshi.team)
* rpc: [https://konstellation.rpc.takeshi.team](https://konstellation.rpc.takeshi.team)
* grpc: [https://konstellation.grpc.takeshi.team](https://konstellation.grpc.takeshi.team)

## Peering

**state-sync**

```
d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@konstellation.rpc.takeshi.team:13656
```

**seed-node**

```
400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@konstellation.rpc.takeshi.team:13659
```

**addrbook**

{% code overflow="wrap" %}
```xml
curl -Ls https://snapshots.takeshi.team/konstellation/addrbook.json > $HOME/.knstld/config/addrbook.json
```
{% endcode %}

**live-peers** (10)

{% code overflow="wrap" %}
```bash
peers="ccffabe81f2de8a81e171f93fe1209392bf9993f@65.108.234.59:26656,d6f2eee997d108d4fde5683e31d678427376dfce@77.68.27.75:26656,935c1065ad23338a5e6a75f08fb650f9f46dbd3e@65.108.201.167:26656,213dbb8301ce1c0f5662a9b723bd613f15e1dd4e@75.119.157.167:30656,b80cf7882c8cab4894d41ccd4f5a00406d8b5f7d@146.59.52.48:30095,d3427d444b6909529d73025fe32a73dfea7b90d1@148.251.85.115:26656,129771a48f43b83c6144c7d282ad1da62434cc07@15.204.197.12:26656,c124ce0b508e8b9ed1c5b6957f362225659b5343@136.243.248.190:26656,d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@65.109.88.38:13656,04b384fd77f70082a9a6e4d8fb3db827340f4e74@148.251.13.186:11856"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.knstld/config/config.toml
```
{% endcode %}
