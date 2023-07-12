# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/composable.png" alt=""><figcaption></figcaption></figure>

The complete infrastructure for cross-chain smart  contracts, applications, and modular functionality.

**Chain ID**: banksy-testnet-3 | **Latest Version Tag**: v3.0.3-testnet | **Wasm**: OFF

[Website](https://www.composable.finance) | [Discord](https://discord.gg/composable) | [Twitter](https://twitter.com/ComposableFin)



Subscribe to our free [🤖 Testnet Proposal Bot](https://t.me/kjnodes_testnet_proposal_bot) to never miss upcoming proposals


## Chain explorer
[https://explorer.takeshi.team/composable-testnet](https://explorer.takeshi.team/composable-testnet)

## Public endpoints

* api: [https://composable-testnet.api.takeshi.team](https://composable-testnet.api.takeshi.team)
* rpc: [https://composable-testnet.rpc.takeshi.team](https://composable-testnet.rpc.takeshi.team)
* grpc: composable-testnet.grpc.takeshi.team:15990

## Peering

**state-sync**

```text
d5519e378247dfb61dfe90652d1fe3e2b3005a5b@composable-testnet.rpc.takeshi.team:15956
```

**seed-node**

```text
3f472746f46493309650e5a033076689996c8881@composable-testnet.rpc.takeshi.team:15959
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/composable-testnet/addrbook.json > $HOME/.banksy/config/addrbook.json
```

**live-peers** (30)
```bash
peers="df49f4fee2fe62bc0ca8c27ee0dbae3f0abec98f@46.38.232.86:24656,33d01ca326bb21c3e02c6f05b9cb530eea93c39d@65.109.23.237:30536,1f3bc143690c465800406a7b6c2898d4f0adebe6@65.21.91.160:27111,783e682b38c0565082fe5d897b24feebf687c52b@65.108.13.154:37656,790b9221fd5e05957fba1fe186e3a0a6972ff7d6@65.109.99.216:15956,9ae49a070ea985784830da8050769ad6791caef5@164.92.64.61:15956,e9441db297752fb454f63d7f0f0c8eb5e067d528@34.124.143.97:26656,3f0727b11da4dc792fe2dfb34214cf45fadd4a15@95.216.67.178:26656,d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:15956,e083e1ee42159e3b57284d38530efc29c6f8a4c9@109.123.247.105:26656,4491f06f803252917d69d053ed85adba5ad17474@5.166.240.95:15956,8be7bfa6c270469971875cb6f23c957402654a14@207.180.194.162:26656,0a68e21ab47c15f634a97019c2a0b8d3bea09622@185.190.142.177:26656,a3ddd1ffc5d24bd12fc4b2af5d2769776f5ce67d@65.109.92.240:21206,2a9225e33a3cd40d4f9118a111a463e4c11bc6c2@31.220.85.1:26656,76bde904c1f177a2c8c1123150073be38c27ad5f@75.119.146.244:26656,b2ab46fe515d0ede14bbe37b16a24bfdf67c8a5b@167.235.7.34:56656,f4078136bacf232ff67c4ab0fdbe5c88fb1f2f94@31.220.72.179:26656,f6bdd60edcc84f2f02d582dc411cef80c5176df1@38.242.133.188:26656,d2deff06cf95c0d016d8f65822e1c74ce2af9def@95.217.58.111:26656,c241d021004ad9b0fe7fa2d967ff9f1f3b20c1f0@136.243.172.166:15956,3461731f09871909987fa3df99c9ac623ea303b3@207.180.241.219:26656,c866bd14649bb402dcb08c861add820b152e39e3@173.212.233.177:15956,5a331fc6afa9ae7cbd6c9ebf39358161052c962b@65.109.65.248:37656,3351847a55dd16faf533f3a02caba9610cc87320@158.220.100.228:27656,5fcb4e8ac8d621d165a6616ae56ef5d5fd4f57bf@84.54.23.37:15956,ca1d4fd9037ad49a37976fa4bdfcce7f4329857f@158.101.110.160:26656,65a491cae106ac91a2995af583214a02403218ef@51.81.155.97:22256,bc5c4e4d5d4b4a1ab157e5d6907b8ae335aa2183@95.216.213.192:26656,638ae5071bd03e35c90e90c11a57c580d80cde0c@81.5.117.14:15956"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.banksy/config/config.toml
```
