---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/chain4energy.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: perun-1 | **Latest Version Tag**: v1.1.0 | **Custom Port**: 34

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf c4e-chain
git clone https://github.com/cosmos/c4e-chain.git
cd c4e-chain
git checkout v1.1.0

# Build binaries
make build

