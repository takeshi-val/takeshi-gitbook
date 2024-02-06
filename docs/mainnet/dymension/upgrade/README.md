---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/dymension.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: dymension_1100-1 | **Latest Version Tag**: v3.0.0

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf dymension
git clone https://github.com/dymension/dymension-sdk.git dymension
cd dymension
git checkout v3.0.0
make install

sudo systemctl restart dymd && journalctl -u dymd -f
'''


