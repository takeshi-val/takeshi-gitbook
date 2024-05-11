---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/pylons.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: reb\_1111-1 | **Latest Version Tag**: v0.2.0 | **Custom Port**: 21

{% hint style="info" %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade. You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf pylons
git clone https://github.com/pylonschain/pylons.git
cd pylons
git checkout v0.2.0

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.pylons/cosmovisor/upgrades/v0.2.0/bin
mv build/pylonsd $HOME/.pylons/cosmovisor/upgrades/v0.2.0/bin/
rm -rf build
```

_Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!_
