---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/haqq.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: haqq_54211-3 | **Latest Version Tag**: v1.1.0 | **Custom Port**: 35

{% hint style='info' %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade.
You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf haqq
git clone https://github.com/haqq-network/haqq.git
cd haqq
git checkout v1.1.0

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.haqqd/cosmovisor/upgrades/v1.1.0/bin
mv build/haqqd $HOME/.haqqd/cosmovisor/upgrades/v1.1.0/bin/
rm -rf build
```

*Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!*
