---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenanced_logo_name.png" alt="" width="150"><figcaption></figcaption></figure>

**Chain ID**: pio-mainnet-1 | **Latest Version Tag**: v1.25.0

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf provenance
git clone https://github.com/provenance-io/provenance.git
cd provenance
git reset --hard
git checkout v1.25.1

# Build binaries
make install

# restart
sudo systemctl restart provenanced && journalctl -u provenanced -f
```
