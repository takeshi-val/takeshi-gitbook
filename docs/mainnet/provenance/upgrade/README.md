---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenance.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: pio-mainnet-1 | **Latest Version Tag**: v1.15.0 

{% hint style='info' %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade.
You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf provenance
git clone https://github.com/provenance-io/provenance.git
cd provenance
git checkout v1.15.0

# Build binaries
make install

# restart
sudo systemctl restart provenanced && journalctl -u provenanced -f
```


