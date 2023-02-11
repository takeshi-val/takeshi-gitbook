---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/dymension.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: 35-C | **Latest Version Tag**: dymension | **Custom Port**: 27

{% hint style='info' %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade.
You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf dymension
git clone https://github.com/dymension/dymension-sdk.git dymension
cd dymension
git checkout dymension

# Install and build dymension Javascript packages
yarn install && yarn build

# Install and build dymension Cosmos SDK support
(cd packages/cosmic-swingset && make)

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.dymension/cosmovisor/upgrades/dymensiontest-upgrade-8/bin
ln -s $HOME/dymension/packages/cosmic-swingset/bin/ag-chain-cosmos $HOME/.dymension/cosmovisor/upgrades/dymensiontest-upgrade-8/bin/ag-chain-cosmos
ln -s $HOME/dymension/packages/cosmic-swingset/bin/ag-nchainz $HOME/.dymension/cosmovisor/upgrades/dymensiontest-upgrade-8/bin/ag-nchainz
cp golang/cosmos/build/dymd $HOME/.dymension/cosmovisor/upgrades/dymensiontest-upgrade-8/bin/
cp golang/cosmos/build/ag-cosmos-helper $HOME/.dymension/cosmovisor/upgrades/dymensiontest-upgrade-8/bin/
```

*Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!*
