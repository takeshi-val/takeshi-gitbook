---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/kichain.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: kichain-emerynet-5 | **Latest Version Tag**: pismoA | **Custom Port**: 27

### Setup validator name

{% hint style='info' %}
Replace **YOUR_MONIKER** with your validator name
{% endhint %}

```bash
MONIKER="YOUR_MONIKER"
```

### Install dependencies

#### Add package repository for Node.js

```bash
curl -Ls https://deb.nodesource.com/setup_16.x | sudo bash
```

#### Add package repository for Yarn

```bash
curl -Ls https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```

#### Update system and install build tools

```bash
sudo apt -q update
sudo apt -qy install curl git jq lz4 build-essential nodejs=16.* yarn
sudo apt -qy upgrade
```

#### Install Go

```bash
sudo rm -rf /usr/local/go
curl -Ls https://go.dev/dl/go1.19.5.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
eval $(echo 'export PATH=$PATH:/usr/local/go/bin' | sudo tee /etc/profile.d/golang.sh)
eval $(echo 'export PATH=$PATH:$HOME/go/bin' | tee -a $HOME/.profile)
```

### Download and build binaries

```bash
# Clone project repository
cd $HOME
rm -rf pismoA
git clone https://github.com/kichain/kichain-sdk.git pismoA
cd pismoA
git checkout pismoA

# Install and build kichain Javascript packages
yarn install && yarn build

# Install and build kichain Cosmos SDK support
(cd packages/cosmic-swingset && make)

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.kid/cosmovisor/genesis/bin
ln -s $HOME/pismoA/packages/cosmic-swingset/bin/ag-chain-cosmos $HOME/.kid/cosmovisor/genesis/bin/ag-chain-cosmos
ln -s $HOME/pismoA/packages/cosmic-swingset/bin/ag-nchainz $HOME/.kid/cosmovisor/genesis/bin/ag-nchainz
cp golang/cosmos/build/kid $HOME/.kid/cosmovisor/genesis/bin/
cp golang/cosmos/build/ag-cosmos-helper $HOME/.kid/cosmovisor/genesis/bin/

# Create application symlinks
ln -s $HOME/.kid/cosmovisor/genesis $HOME/.kid/cosmovisor/current
sudo ln -s $HOME/.kid/cosmovisor/current/bin/kid /usr/local/bin/kid
```

### Install Cosmovisor and create a service

```bash
# Download and install Cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.4.0

# Create service
sudo tee /etc/systemd/system/kid.service > /dev/null << EOF
[Unit]
Description=kichain-testnet node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.kid"
Environment="DAEMON_NAME=kid"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable kid
```

### Initialize the node

```bash
# Set node configuration
kid config chain-id kichain-emerynet-5
kid config keyring-backend test
kid config node tcp://localhost:27657

# Initialize the node
kid init $MONIKER --chain-id kichain-emerynet-5

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/kichain-testnet/genesis.json > $HOME/.kid/config/genesis.json
curl -Ls https://snapshots.takeshi.team/kichain-testnet/addrbook.json > $HOME/.kid/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"3f472746f46493309650e5a033076689996c8881@kichain-testnet.rpc.takeshi.team:27659\"|" $HOME/.kid/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.025ubld\"|" $HOME/.kid/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.kid/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:27658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:27657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:27060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:27656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":27660\"%" $HOME/.kid/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:27317\"%; s%^address = \":8080\"%address = \":27080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:27090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:27091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:27545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:27546\"%" $HOME/.kid/config/app.toml
```

### Download latest chain snapshot

```bash
curl -L https://snapshots.takeshi.team/kichain-testnet/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.kid
[[ -f $HOME/.kid/data/upgrade-info.json ]] && cp $HOME/.kid/data/upgrade-info.json $HOME/.kid/cosmovisor/genesis/upgrade-info.json
```

### Start service and check the logs

```bash
sudo systemctl start kid && sudo journalctl -u kid -f --no-hostname -o cat
```
