---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/kichain.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: kichain-2 | **Latest Version Tag**: v4.2.0 | 

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf ki-tools
git clone https://github.com/kichain/kichain-sdk.git pismoA
cd ki-tools
git checkout v4.2.0
make install

sudo systemctl start kid && journalctl -u kid -f 
