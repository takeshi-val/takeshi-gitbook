# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/chain4energy.png" width="150" alt=""><figcaption></figcaption></figure>

Chain4Energy is a revolutionary Web 3.0 Energy Marketplace for the global energy economy.

**Chain ID**: perun-1 | **Latest Version Tag**: v1.1.0 | **Wasm**: OFF

[Website](https://c4e.io/) | [Discord](https://discord.gg/chain4energy) | [Twitter](https://twitter.com/Chain4Energy)


## Public endpoints

* API: [https://api-c4e.takeshi.team](https://api-c4e.takeshi.team)
* RPC: [https://rpc-c4e.takeshi.team](https://rpc-c4e.takeshi.team)
* GRPC: [grpc-c4e.takeshi.team](grpc://grpc-c4e.takeshi.team)



## Peering

**state-sync**

```text
https://rpc-c4e.takeshi.team:443
```

**seed-node**

```text
a85a651a3cf1746694560c5b6f76d566c04ca581@c4e-seed.takeshi.team:10256
```
**peer-node**

```text
07bcb7b02e5f20c868db5ed114d60defb67d3dcf@c4e-peer.takeshi.team:32656
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/chain4energy/addrbook.json > $HOME/.c4e-chain/config/addrbook.json
```

**live-peers** 
```bash
SEEDS="a85a651a3cf1746694560c5b6f76d566c04ca581@c4e-seed.takeshi.team:10256"
PEERS="07bcb7b02e5f20c868db5ed114d60defb67d3dcf@c4e-peer.takeshi.team:32656"
sed -i "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $C4E_HOME/config/config.toml

```
