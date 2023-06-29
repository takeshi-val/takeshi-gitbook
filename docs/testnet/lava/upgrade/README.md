---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/lava.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: lava-testnet-1 | **Latest Version Tag**: v0.15.1 | **Custom Port**: 44

{% hint style='info' %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade.
You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and install upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf lava
git clone https://github.com/lavanet/lava.git
cd lava
git checkout v0.15.1

# Install binaries
make install
```

```bash
#Start 
sudo systemctl restart lavad && journalctl -u lavad -f
```

*Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!*
