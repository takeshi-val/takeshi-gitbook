# Services

<figure><img src="https://raw.githubusercontent.com/kj89/testnet_manuals/main/pingpub/logos/teritori.png" width="150" alt=""><figcaption></figcaption></figure>

Teritori is a multi-chain hub designed to allow IBC and non IBC communities  to connect, trade services & NFTs, launch new projects & build further existing ones.

**Chain ID**: teritori-testnet-v3 | **Latest Version Tag**: v1.3.0 | **Wasm**: ON

[Website](https://teritori.com) | [Discord](https://discord.gg/teritori) | [Twitter](https://twitter.com/TeritoriNetwork)




## Chain explorer
[https://explorer.kjnodes.com/teritori-testnet](https://explorer.kjnodes.com/teritori-testnet)

## Public endpoints

* api: [https://teritori-testnet.api.kjnodes.com](https://teritori-testnet.api.kjnodes.com)
* rpc: [https://teritori-testnet.rpc.kjnodes.com](https://teritori-testnet.rpc.kjnodes.com)
* grpc: [https://teritori-testnet.grpc.kjnodes.com](https://teritori-testnet.grpc.kjnodes.com)

## Peering

**state-sync**

```text
d5519e378247dfb61dfe90652d1fe3e2b3005a5b@teritori-testnet.rpc.kjnodes.com:19656
```

**seed-node**

```text
3f472746f46493309650e5a033076689996c8881@teritori-testnet.rpc.kjnodes.com:19659
```

**addrbook**
```bash
curl -Ls https://snapshots.kjnodes.com/teritori-testnet/addrbook.json > $HOME/.teritorid/config/addrbook.json
```

**live-peers** (28)
```bash
peers="07d196ccefcadc548c6cd06cfea425f1544b1495@213.239.217.52:41656,6bc9f80a5123d62c23aadb7b5d68b740a794b0c6@207.180.194.156:36656,3614bc766d73bebf6b73737b6690af60e7f0683e@65.108.206.118:46656,4ebfdac0d496be2407c02202e5ad6f226a11b37a@65.21.134.202:26736,3b539b6cff93fb3631d0a600a56ade3c6ca6bea3@51.79.28.170:26656,8ef4ef39a887861744717feacc350403387c4c56@65.109.38.54:21096,303666c503cd27161529692de701f5b2d3a2f043@65.109.23.114:15956,39a4dbd5a4199187bf4f6b30ac03156b3e3d7b29@65.21.139.170:20026,d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:19656,6a94690aa76f7ffbfa1ee93c50dddfb571f159b6@5.189.130.43:19656,e1c50c477202e2f37643d044a6cde3c913f42230@65.108.71.92:54256,69012ce642095e15f588ddb154327633bb2ecb9c@65.109.39.223:26656,ac94097daec8a32d4ed3f074f26f214cedfbb541@85.173.112.154:26656,ec0c58dbfe67a12ea16951134e29a6566ac05add@185.217.125.98:26656,0e51ebd10636b48b69625677a5154b839ff3f557@65.108.43.116:56107,c89ecc57dc30addb7e9032684916725c25b2a6c5@162.55.103.44:26656,e78cee0e46927e483212e0313a35da6cc9151ed5@65.109.28.219:15956,bf100c1b6b44a6e96ab5691f3023cec3c27747fd@144.126.142.78:46656,a97eb7a4f3d857f1ff82265d2905fc0762a6bfd4@135.125.5.31:54256,31413c99357d0cfc48a46767ade171db2ea0205e@135.181.138.160:46656,5ae1012f9b0f4672d8152de903d115dd2f1a3ee3@65.21.170.3:27656,c56b132be41b247c9f8fa1f2addaca57f9946e29@75.119.159.159:44656,ec8faa221a99f5c6d8f647cd08f60f2ace0ed1e2@65.109.112.20:11044,e1b331c1f3cba509960c65d6c6bc9b49532bcbaa@65.109.85.170:27656,53f69cd52a4b633179b9e762cf8d51f6696a27f6@51.159.141.148:26656,15dd94f68c450da2c3b7c60b6364e3dce6f0cbf2@185.193.66.68:26641,625b814af9f535b91a92727138838fde0174faff@65.108.124.172:27656,b33ebb4672f929dddde1365c9678a39abfd881fb@54.202.144.51:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.teritorid/config/config.toml
```
