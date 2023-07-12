# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/lava.png" width="150" alt=""><figcaption></figcaption></figure>

Lava powers a trustless market for RPC data access. The protocol  governs over peer to peer and private Provider-Application pairings,  ensuring high quality RPC service while creating consensus around data served.

**Chain ID**: lava-testnet-1 | **Latest Version Tag**: v0.15.1 | **Wasm**: OFF

[Website](https://lavanet.xyz) | [Discord](https://discord.com/invite/Tbk5NxTCdA) | [Twitter](https://twitter.com/lavanetxyz)




## Chain explorer
[https://lava.explorers.guru](https://lava.explorers.guru/)

## Public endpoints

* api: [https://lava-api.takeshi.team](https://lava-api.takeshi.team)
* rpc: [https://lava-rpc.takeshi.team](https://lava-rpc.takeshi.team)
* grpc: [https://lava-grpc.takeshi.team](https://lava-grpc.takeshi.team)

## Peering

**state-sync**

```text
f19e1a6973b0f8e43189e618eb8e61693e892231@lava-rpc.takeshi.team:443```

**seed-node**

```text
a85a651a3cf1746694560c5b6f76d566c04ca581@lava-rpc.takeshi.team:44659
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/lava-testnet/addrbook.json > $HOME/.lava/config/addrbook.json
```

**live-peers** (24)
```bash
peers="c32d101819cedf78ea986e6d832e2306fb6d0649@185.248.24.224:16656,ed780f77754e8c4657b145144f0f95225d43bb03@65.108.224.156:27656,013f0163d37428ed99eacd8ee84059da5c243981@5.161.132.217:26656,9a151159039fd8abce61ddb21e5342605787792b@5.75.228.39:26656,5b337f7ba27e2fdd27918be18af93f8728034267@65.108.41.168:26656,25da069c4dca143029ddae47bf2b7de69c2a8678@65.108.9.164:21156,92f8e4caaadb2f00c95e03068933f2045a93e910@65.109.65.163:21156,6ba3b6ec03839afffa64c83e18ff80a681f4968d@65.108.194.40:21756,e1383b216c42acc842193c5ac7321ce6c0d73db0@78.47.37.142:26656,370ae92bd28701e0c1d8dc912ccf0d40fe0db3d5@157.90.245.166:26656,3a445bfdbe2d0c8ee82461633aa3af31bc2b4dc0@3.252.219.158:26656,3456c9ba0df46cbb526717d73fa51ff0ed9a53a1@95.216.14.58:60756,2a588e5ddcfd8c9095cc6f34b5b6966e31020cfd@65.21.123.172:11656,c0efea9152aed75fcf3022b8af45243818c59d6a@49.12.13.104:26656,3173b2d34ce415ee9a1bf08646d85688bf49e299@5.189.186.222:36656,e593c7a9ca61f5616119d6beb5bd8ef5dd28d62d@34.246.190.1:26656,2c419186cd96b59fe8b3307c54c27d6805414aba@65.108.8.28:60756,4732ed188fbe7603f81d9f4c825397277bb72217@5.75.235.195:26656,cb722cc36541920d3907cd67743db5444f53e80b@95.70.184.178:24656,8bb931d994a19c6647e6165cae98b14bcc2e22c2@144.76.99.105:38656,5c2a752c9b1952dbed075c56c600c3a79b58c395@185.16.39.172:27066,4ad3f3731073a016fa0c99118b2a5a2d313928f5@207.180.233.148:26656,8b154033143fdedf4835dfc7b030c7d781bfd54e@195.201.219.227:26656,b1a9277efbd2634979b8bf90ebfde19f3af830bd@75.119.146.252:44656,99327e5cf0f31ac3bb1ca8e39cc9f17c823b7ec1@109.236.88.8:26656,f1bb78a30c9381bed392fda141a5c1f6fa4d25e6@144.76.114.49:26656,4e0a2772bb3672e54c2ea655c30abdac62191f14@45.84.138.66:18656,3b18b1dc95e02a36327b13fc45c225b23fb08ed8@78.47.187.72:26656,5c107bb2b72c930a5ab3406a1f7c7345b7229b49@148.251.11.99:11656,d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:44656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.lava/config/config.toml
```
