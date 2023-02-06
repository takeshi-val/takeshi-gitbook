---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://raw.githubusercontent.com/kj89/testnet_manuals/main/pingpub/logos/stride.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: stride-1 | **Latest Version Tag**: v5.1.1 | **Custom Port**: 16

{% hint style='info' %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade.
You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf stride
git clone https://github.com/Stride-Labs/stride.git
cd stride
git checkout v5.1.1

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.stride/cosmovisor/upgrades/v5/bin
mv build/strided $HOME/.stride/cosmovisor/upgrades/v5/bin/
rm -rf build
```

*Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!*
