# Services

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/warden.png" alt=""><figcaption></figcaption></figure>

warden is an application platform layer that connects all  public blockchains in the Cosmos ecosystem. Through our vast  library of no-code smart contracts, users can harness the power of web3.

**Chain ID**: buenavista-1 | **Latest Version Tag**: v0.3.0 

[Website](https://wardenprotocol.org) | [Discord](https://discord.com/invite/wardenprotocol) | [Twitter](https://twitter.com/wardenprotocol)






## Chain explorer
[https://explorer.takeshi.team/warden-testnet](https://explorer.takeshi.team/warden)

## Public endpoints

* api: [https://warden-testnet.api.takeshi.team](https://warden-api.takeshi.team)
* rpc: [https://warden-testnet.rpc.takeshi.team](https://warden-rpc.takeshi.team)
* grpc: warden-grpc.takeshi.team:9024

## Peering

**state-sync**

```text
d5519e378247dfb61dfe90652d1fe3e2b3005a5b@warden-rpc.takeshi.team:24456
```

**seed-node**

```text
a85a651a3cf1746694560c5b6f76d566c04ca581@warden-rpc.takeshi.team:24459
```

**addrbook**
```bash
curl -Ls https://snapshots.takeshi.team/warden/addrbook.json > $HOME/.warden/config/addrbook.json
```

**live-peers** (11)
```bash
peers="6a8de92a3bb422c10f764fe8b0ab32e1e334d0bd@sentry-1.alfama.wardenprotocol.org:26656,7560460b016ee0867cae5642adace5d011c6c0ae@sentry-2.alfama.wardenprotocol.org:26656,24ad598e2f3fc82630554d98418d26cc3edf28b9@sentry-3.alfama.wardenprotocol.org:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.warden/config/config.toml
```
