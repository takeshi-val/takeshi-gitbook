# Canto

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/kujira.png" alt=""><figcaption></figcaption></figure>

Kujira is a Layer 1 protocol built on Cosmos, which can be used for hosting community-selected projects.

**Chain ID**: kaiyo-1 | **Latest Version Tag**: v0.7.1 | **Wasm**: ON

[Website](https://kujira.app) | [Discord](https://discord.gg/teamkujira) | [Twitter](https://twitter.com/TeamKujira)

[![Stake with kjnodes](https://i.ibb.co/cr44Q8j/button-stake-with-kjnodes.png)](https://restake.app/kujira/kujiravaloper1tnuqj73jfn3724lqz34c27tuv80nv336sadqym)

## Restake

[Restake with kjnodes](https://restake.app/kujira/kujiravaloper1tnuqj73jfn3724lqz34c27tuv80nv336sadqym) (every day at 08:00, 20:00)

## Chain explorer

[https://explorer.takeshi.team/kujira](https://explorer.takeshi.team/kujira)

## Public endpoints

* api: [https://kujira.api.takeshi.team](https://kujira.api.takeshi.team)
* rpc: [https://kujira.rpc.takeshi.team](https://kujira.rpc.takeshi.team)
* grpc: [https://kujira.grpc.takeshi.team](https://kujira.grpc.takeshi.team)

## Peering

**state-sync**

```
d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@kujira.rpc.takeshi.team:13656
```

**seed-node**

```
400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@kujira.rpc.takeshi.team:13659
```

**addrbook**

```bash
curl -Ls https://snapshots.takeshi.team/kujira/addrbook.json > $HOME/.kujira/config/addrbook.json
```

**live-peers** (10)

```bash
peers="ccffabe81f2de8a81e171f93fe1209392bf9993f@65.108.234.59:26656,d6f2eee997d108d4fde5683e31d678427376dfce@77.68.27.75:26656,935c1065ad23338a5e6a75f08fb650f9f46dbd3e@65.108.201.167:26656,213dbb8301ce1c0f5662a9b723bd613f15e1dd4e@75.119.157.167:30656,b80cf7882c8cab4894d41ccd4f5a00406d8b5f7d@146.59.52.48:30095,d3427d444b6909529d73025fe32a73dfea7b90d1@148.251.85.115:26656,129771a48f43b83c6144c7d282ad1da62434cc07@15.204.197.12:26656,c124ce0b508e8b9ed1c5b6957f362225659b5343@136.243.248.190:26656,d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@65.109.88.38:13656,04b384fd77f70082a9a6e4d8fb3db827340f4e74@148.251.13.186:11856"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.kujira/config/config.toml
```
