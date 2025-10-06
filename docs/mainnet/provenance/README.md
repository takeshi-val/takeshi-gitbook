# Konstellation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/PB Logo_Color_Vertical.png" alt=""><figcaption></figcaption></figure>

The provenance Protocol is a fast, scalable, and secure blockchain that empowers individuals, developers, and enterprises to increase their data privacy and cybersecurity posture without sacrificing ease of use. This protocol strives to offer world-class applications to protect the planet's most important data–your data.

**Chain ID**: pio-mainnet-1 | **Latest Version Tag**: v1.25.0 | **Wasm**: ON

[Website](https://provenance.io) | [Discord](https://discord.gg/kNZC8nwCFP) | [Twitter](https://twitter.com/provenancefdn)

## Chain explorer

[https://explorer.takeshi.team/provenance](https://explorer.takeshi.team/provenance)

## Public endpoints

* api: [https://api-provenance.takeshi.team](https://api-provenance.takeshi.team)
* rpc: [https://rpc-provenance.takeshi.team](https://rpc-provenance.takeshi.team)
* grpc: [https://grpc-provenance.takeshi.team](https://grpc-provenance.takeshi.team)

## Peering

**state-sync**

```
a85a651a3cf1746694560c5b6f76d566c04ca581@provenance-seed.takeshi.team:10556
```

**seed-node**

```
a85a651a3cf1746694560c5b6f76d566c04ca581@provenance-seed.takeshi.team:10556
```

**addrbook**

```bash
curl -Ls https://snapshots.takeshi.team/provenance/addrbook.json > $HOME/.provenance/config/addrbook.json
```

**live-peers** (21)

```bash
peers="83d66a37202785b09aee4e3ae1b50d2ddfbf860c@162.19.89.8:10856,46d4495643f2579573a61e181a88de3b8f0acc4f@2.139.23.24:36656,5745d29dd5b49009f405e21913a474a23f1e40ec@131.153.57.226:43656,d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@65.109.88.38:37656,11c23c5341d0ac69f9ebb3be9afa7fe0e134ece0@94.79.54.137:28656,ff94a29e02de8369faf37c76d3c97684bbd51bd6@185.16.38.165:17556,399068f8371dce4ae5d7cd7da2c965e765e68f4b@65.108.238.102:17556,0985977a794b298e7ef990fe344d572c60c453b1@172.105.72.158:26656,dd3cab79ffae0aed4f519503b66e9403c69eeb14@85.237.193.101:25565,d39fecbc409541de13fa644d90066d4dabe08262@95.165.89.222:24475,ebc272824924ea1a27ea3183dd0b9ba713494f83@95.214.52.139:26906,ff7ab7fdac43752163f141809b61c67eba837cb4@65.108.97.58:37656,9bcaee1ad957fa75f60a6dd9d8870e53220794a9@104.37.187.214:60756,0faa7f1099de2e02deebe09fcb52863056333265@144.202.72.17:26616,039a1c4f438c1ecc2dd901e7316d16fdafadfdab@104.193.254.36:27656,e08efc0b0e15e4d8eacf0f4ed5e52f6e9bdc312d@144.76.97.251:36156,26b6255375a592c3b0664bd474a6975f468c3785@88.99.164.158:11126,c2842c76779913e05fa4256e3caab852e1782951@202.61.194.254:60756,a79da224ad9d4501dbf1d547986ebec55d56b951@135.181.128.114:17556,7adbbe1a5f867a0befcf1fd94f395dd8257d718f@73.40.151.121:57656,289c3e984194ac2ccaa74e201147010648e90970@195.3.223.108:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.provenance/config/config.toml
```
