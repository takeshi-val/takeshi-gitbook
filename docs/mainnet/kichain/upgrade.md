---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/agoric.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: agoric-3 | **Latest Version Tag**: pismoA | **Custom Port**: 27

{% hint style="info" %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade. You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf pismoA
git clone https://github.com/Agoric/agoric-sdk.git pismoA
cd pismoA
git checkout pismoA

# Install and build Agoric Javascript packages
yarn install && yarn build

# Install and build Agoric Cosmos SDK support
(cd packages/cosmic-swingset && make)

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.agoric/cosmovisor/upgrades/agoric-upgrade-8/bin
ln -s $HOME/pismoA/packages/cosmic-swingset/bin/ag-chain-cosmos $HOME/.agoric/cosmovisor/upgrades/agoric-upgrade-8/bin/ag-chain-cosmos
ln -s $HOME/pismoA/packages/cosmic-swingset/bin/ag-nchainz $HOME/.agoric/cosmovisor/upgrades/agoric-upgrade-8/bin/ag-nchainz
cp golang/cosmos/build/agd $HOME/.agoric/cosmovisor/upgrades/agoric-upgrade-8/bin/
cp golang/cosmos/build/ag-cosmos-helper $HOME/.agoric/cosmovisor/upgrades/agoric-upgrade-8/bin/
```

_Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!_
