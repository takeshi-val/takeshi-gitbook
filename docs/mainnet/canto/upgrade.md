---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/canto.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: canto_7700-1 | **Latest Version Tag**: v1.5.3 | **Custom Port**: 42

{% hint style="info" %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade. You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf bcna
git clone https://github.com/BitCannaGlobal/bcna.git
cd bcna
git checkout v1.5.3

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.cantod/cosmovisor/upgrades/trichomemonster-ica/bin
mv build/cantod $HOME/.cantod/cosmovisor/upgrades/trichomemonster-ica/bin/
rm -rf build
```

_Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!_
