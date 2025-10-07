# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/quicksilver.png" width="150" alt=""><figcaption></figcaption></figure>

Quicksilver is a permissionless, sovereign Cosmos SDK zone providing liquid staking for the entire Cosmos Ecosystem.

**Chain ID**: quicksilver-2 | **Latest Version Tag**: v1.8.1 | **Wasm**: OFF

[Website](https://quicksilver.zone) | [Discord](https://discord.gg/quicksilverprotocol) | [Twitter](https://twitter.com/quicksilverzone)


## Chain explorer
[https://explorer.takeshi.team/quicksilver](https://explorer.takeshi.team/quicksilver)

## Public endpoints

* api: [https://api-quicksilver.takeshi.team](https://api-quicksilver.takeshi.team)
* rpc: [https://rpc-quicksilver.takeshi.team/](https://rpc-quicksilver.takeshi.team)
* grpc: [https://grpc-quicksilver.takeshi.team](https://grpc-quicksilver.takeshi.team)

## Peering

**seed-node**

```text
a85a651a3cf1746694560c5b6f76d566c04ca581@quicksilver-seed.takeshi.team:10456
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/quicksilver/addrbook.json > $HOME/.quicksilverd/config/addrbook.json
```

**live-peers** (82)
```bash
peers="161f453c9ff27f3120ec5078f56b505316fbc720@65.108.6.45:61156,e1b058e5cfa2b836ddaa496b10911da62dcf182e@138.201.8.248:26656,cbc2c7a7cd39750abee0dcd5dd2832feddbde20e@50.21.173.76:26656,26d23125db7493486dc9931b4181425d725e4ac6@65.109.55.186:20656,0914b21ef0c3b325a82a37e58107d1271f201258@162.55.194.205:11656,2c64f16113722e29c14db3bba555128ad3f713a7@95.217.202.49:31656,5fe7dc208641e3e730867c49b396cc7e248969fc@88.208.34.134:26656,8af9b9d86faaa41e5036b8ea143e63acb88a4a59@95.217.109.223:36656,149a25417349d70f5e5127a5eb634dbfaf6e6c3a@142.165.207.19:56656,ee14b4bbeb436056952c8e4e7c84826dfb92143b@65.109.105.17:26656,03f8f542594292401d2378cc8dffb8ec92ab9b07@74.96.207.60:26656,e1a24aaba30a8ff21e52fed92b96b36156b52e80@51.161.208.88:26656,3bd708547317e9efd8d63d8a51c5bc32d11f4840@138.201.32.103:26056,9bd8172552086e445ae72386568ec6b452d6ef23@144.91.80.32:11656,0865ef3e5a613f75f17a0092bd47e71d8c171124@51.222.44.116:15656,602700ce2ed57b2176514ec2ecbda079caa7a536@178.170.40.28:15620,64112911cda67dd6566763c49bddadfee2631bd1@188.165.205.120:11656,51070ba609ede6d7eb334b8cf0ed585f2b1ab66b@135.181.76.99:26656,c0beca70dbd3ef5bb433f7aa280d56d2a150bbd3@95.214.52.144:26656,e64a4e480a2971c339fa06a58293e8e060082ad5@185.16.36.134:26656,09f16a08fb0da3a20a7bc0212e3bc4645b04918c@65.21.142.30:28656,b7444c08fe588eac9a68e0fabb2328a1386e9a3b@193.34.212.34:11244,e50848e299c7909245a9af690341ff27e21f7b69@65.109.49.111:56656,a7d96dc929824613315dcc1c90fee119f28cc51f@169.155.168.83:26656,3394976851c8a06002989572119925f6d839a980@51.195.234.250:26656,3308d9078fcca016fbd8dc8f3b19666326f41a6f@138.201.121.185:26672,2c658378f5356e39ecea6947eb312f45a8ccfde1@142.132.199.211:26654,b4bcce87121963e1e97619dc135f2eb1a9fd5dfc@88.198.32.17:36656,443ad7c991b2915b620673b10206c92e2b4040e0@173.67.177.120:26656,6785dbb8a0138600e0e0faaa77baa375451b38bb@162.55.132.48:15620,f644e9f9229ab7c9c70907b134b3b96b18163935@146.19.24.195:22656,ae44851a5d63d70382c1621bc7727db2a40d10d0@88.99.164.158:21026,3b3c0037090a1b5ef9f7ac58ff79f33dffdd188a@65.108.231.124:15656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.quicksilverd/config/config.toml
```
