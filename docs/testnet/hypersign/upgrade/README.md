---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/hypersign.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: prajna-1 | **Latest Version Tag**: v0.2.0 

{% hint style='info' %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade.
You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf hid-node
git clone https://github.com/hypersign-protocol/hid-node.git
cd hid-node
git checkout v0.2.0

# Install binaries
make install

# Restart service
sudo systemctl restart hid-noded && journalctl -u hid-noded -f
```

*Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!*
