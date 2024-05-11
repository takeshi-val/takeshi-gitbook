---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/konstellation.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: darchub | **Latest Version Tag**: v0.6.2 | **Custom Port**: 13

{% hint style="info" %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade. You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf konstellation
git clone https://github.com/Team-konstellation/konstellation.git
cd konstellation
git checkout v0.6.2

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.knstld/cosmovisor/upgrades/v0.6.2/bin
mv build/knstld $HOME/.knstld/cosmovisor/upgrades/v0.6.2/bin/
rm -rf build
```

_Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!_
