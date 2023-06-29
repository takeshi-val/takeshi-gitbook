---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/gravitybridge.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: gravity-bridge-3 | **Latest Version Tag**: v1.10.0 | **Custom Port**: 26

## Download and build upgrade binaries

```bash
# Download project binaries
mkdir -p $HOME/.gravity/cosmovisor/upgrades/pleiades2/bin
wget -O $HOME/.gravity/cosmovisor/upgrades/pleiades2/bin/gravityd https://github.com/Gravity-Bridge/Gravity-Bridge/releases/download/v1.8.1/gravity-linux-amd64
wget -O $HOME/.gravity/cosmovisor/upgrades/pleiades2/bin/gbt https://github.com/Gravity-Bridge/Gravity-Bridge/releases/download/v1.8.1/gbt
chmod +x $HOME/.gravity/cosmovisor/upgrades/pleiades2/bin/*
```

*Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!*
