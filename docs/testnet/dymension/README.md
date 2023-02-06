# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/kichain.png" width="150" alt=""><figcaption></figcaption></figure>

kichain is an interoperable Proof-of-Stake chain in the Cosmos ecosystem.  kichain JavaScript smart contract platform enables 15M+ developers across the  globe to rapidly build and deploy dapps on-chain.

**Chain ID**: kichain-emerynet-5 | **Latest Version Tag**: pismoA | **Wasm**: OFF

[Website](https://foundation.ki/) | [Discord](https://discord.gg/vqWc7MGd87) | [Twitter](https://twitter.com/Ki_Foundation)




## Chain explorer
[https://explorer.takeshi.team/kichain-testnet](https://explorer.takeshi.team/kichain-testnet)

## Public endpoints

* api: [https://kichain-testnet.api.takeshi.team](https://kichain-testnet.api.takeshi.team)
* rpc: [https://kichain-testnet.rpc.takeshi.team](https://kichain-testnet.rpc.takeshi.team)
* grpc: [https://kichain-testnet.grpc.takeshi.team](https://kichain-testnet.grpc.takeshi.team)

## Peering

**state-sync**

```text
d5519e378247dfb61dfe90652d1fe3e2b3005a5b@kichain-testnet.rpc.takeshi.team:27656
```

**seed-node**

```text
3f472746f46493309650e5a033076689996c8881@kichain-testnet.rpc.takeshi.team:27659
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/kichain-testnet/addrbook.json > $HOME/.kid/config/addrbook.json
```

**live-peers** (20)
```bash
peers="d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:27656,4dee5e4456307469d037c35eb0157f1f252b3f99@135.181.35.255:26656,3f4e87ddb2e61fdd01398c071fa986259f096334@209.34.205.57:26656,42084028a65c5d609793ffc618d1dcbf374fc301@65.109.28.219:14456,e5d3db7a51d3fb40a4855d6677318944faf7d5f2@142.132.191.166:26656,a3a1e6c7a9ceec632c22769a9e369d05a796dc24@65.108.79.246:26709,a5b991654d0723e038d3723b1345b2a288d49146@38.242.156.28:26656,fd9d8063921531990cfebb72d5adadf276484e8d@13.215.217.74:26656,fa171a30e3118fb2b92a5afb4bb6e661ad6e6aa0@35.238.67.135:26656,7b1cafa0879374125c623d854bcc0cb9cd98729e@185.213.25.151:26656,98e1069b1cfc445e377eda6a0eadd94f7877065d@162.55.169.76:26656,a73444541956b994f804f6fcf2a26d2c3c9865a3@35.224.177.96:26656,c72d05f83b53dc7f6c55d7d3e67c304716d27d80@116.202.227.117:27656,793955daf95ad29f003cc4ec7e6c60c00677b2f7@5.9.81.187:30656,32f7fbecd40b420d592ac460703c4ac647875566@65.109.23.238:26656,fb86a0993c694c981a28fa1ebd1fd692f345348b@34.171.162.87:26656,a875ef614b3902dd567be2076f18239681f24e35@185.146.148.112:26656,3c2abc308efdc63be1801bbb1b40900ada13349b@35.184.189.155:26656,8dfb920cdc2eba42b688f44fdd26e12dabfbb6a9@95.217.130.111:27656,d238a541e480e06269107449a70b1178ef49aba7@34.67.113.184:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.kid/config/config.toml
```