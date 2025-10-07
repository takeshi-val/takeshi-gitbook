# Provenance

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenanced_logo_name.png" alt=""><figcaption></figcaption></figure>

The provenance Protocol is a fast, scalable, and secure blockchain that empowers individuals, developers, and enterprises to increase their data privacy and cybersecurity posture without sacrificing ease of use. This protocol strives to offer world-class applications to protect the planet's most important data–your data.

**Chain ID**: pio-mainnet-1 | **Latest Version Tag**: v1.25.0 | **Wasm**: ON

[Website](https://provenance.io) | [Discord](https://discord.gg/kNZC8nwCFP) | [Twitter](https://twitter.com/provenancefdn)

## Chain explorer

[https://explorer.provenance.io/dashboard](https://explorer.provenance.io/dashboard)

## Public endpoints

* api: [https://api-provenance.takeshi.team](https://api-provenance.takeshi.team)
* rpc: [https://rpc-provenance.takeshi.team](https://rpc-provenance.takeshi.team)
* grpc:  grpc-provenance.takeshi.team

## Peering

**seed-node**

```bash
1fcb1fe676876087d0c641ad6476417dcccb396f@provenance-seed.takeshi.team:10656
```

**addrbook**

{% code overflow="wrap" %}
```bash
curl -Ls https://snapshots.takeshi.team/provenance/addrbook.json > $HOME/.provenance/config/addrbook.json
```
{% endcode %}

**live-peers** (21)

{% code overflow="wrap" fullWidth="true" %}
```bash
peers="919b4d9257da3c0d0880881758bc43e3c07bae00@142.132.205.169:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.provenance/config/config.toml
```
{% endcode %}
