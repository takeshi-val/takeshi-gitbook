---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://raw.githubusercontent.com/kj89/testnet_manuals/main/pingpub/logos/quicksilver.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: innuendo-5 | **Latest Version Tag**: v1.3.2 | **Custom Port**: 11

{% hint style='info' %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade.
You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf quicksilver
git clone https://github.com/ingenuity-build/quicksilver.git
cd quicksilver
git checkout v1.3.2

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.quicksilverd/cosmovisor/upgrades/v1.3.2/bin
mv build/quicksilverd $HOME/.quicksilverd/cosmovisor/upgrades/v1.3.2/bin/
rm -rf build
```

*Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!*
