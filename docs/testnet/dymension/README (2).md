# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/dymension.png" alt=""><figcaption></figcaption></figure>

Dymension is a network of modular blockchains called RollApps  and at the center of it all is the Dymension Hub. Dymension  allows anyone to build and deploy their own consensus-free blockchain, a RollApp.

**Chain ID**: 35-C | **Latest Version Tag**: v0.2.0-beta | **Wasm**: OFF

[Website](https://dymension.xyz/) | [Discord](https://discord.gg/dymension) | [Twitter](https://twitter.com/dymensionXYZ)






## Chain explorer
[https://explorer.takeshi.team/dymension-testnet](https://explorer.takeshi.team/dymension-testnet)

## Public endpoints

* api: [https://dymension-testnet.api.takeshi.team](https://dymension-testnet.api.takeshi.team)
* rpc: [https://dymension-testnet.rpc.takeshi.team](https://dymension-testnet.rpc.takeshi.team)
* grpc: dymension-testnet.grpc.takeshi.team:14690

## Peering

**state-sync**

```text
d5519e378247dfb61dfe90652d1fe3e2b3005a5b@dymension-testnet.rpc.takeshi.team:14656
```

**seed-node**

```text
a85a651a3cf1746694560c5b6f76d566c04ca581@dymension-testnet.rpc.takeshi.team:14659
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/dymension-testnet/addrbook.json > $HOME/.dymension/config/addrbook.json
```

**live-peers** (10)
```bash
peers="f4be55edab4b5cb40464aa50def5d2cd39359e67@185.182.185.101:26656,5e3d7708c1d00baf343721150da703ece03491a3@194.163.189.122:46656,d2b841acdcabb622e9033fe685a395eef091f2fe@65.108.199.62:46656,48bdb78c51e56b651c938d075e1077dab2c6197c@43.157.22.223:26656,e678f78d3250fef1e6e0afcdb1ebdc5fe0d7138c@5.161.76.147:46656,fc827d9c55d49f0adb31720c134bd8f675ca7b27@95.216.193.163:26656,a0cb3fba615a75460360fd894922e8b53ba6686b@176.57.189.36:11656,77791ee9b1eb56682335c451c296f450ee649c01@44.209.89.17:26656,94fb8e69a7e6628ce0371aaaaf791c97c9fecc07@161.97.133.54:26656,d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:14656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.dymension/config/config.toml
```
