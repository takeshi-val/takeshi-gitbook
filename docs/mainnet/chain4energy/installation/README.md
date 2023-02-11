---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/chain4energy.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: perun-1 | **Latest Version Tag**: v1.1.0 | **Custom Port**: 34

### Setup validator name

{% hint style='info' %}
Replace **YOUR_MONIKER** with your validator name
{% endhint %}

```bash
MONIKER="YOUR_MONIKER"
```

### Install dependencies

#### Update system and install build tools

```bash
sudo apt -q update
sudo apt -qy install curl git jq lz4 build-essential
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
rm -rf c4e-chain
git clone https://github.com/cosmos/c4e-chain.git
cd c4e-chain
git checkout v1.1.0

# Build binaries
make build

```

### Create a service

```bash

# Create service
sudo tee /etc/systemd/system/c4ed.service > /dev/null << EOF
[Unit]
Description=chain4energy node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which c4ed) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.c4e-chain"
Environment="DAEMON_NAME=c4ed"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable c4ed
```

### Initialize the node

```bash
# Set node configuration
c4ed config chain-id perun-1
c4ed config keyring-backend file
c4ed config node tcp://localhost:34657

# Initialize the node
c4ed init $MONIKER --chain-id perun-1

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/chain4energy/genesis.json > $HOME/.c4e-chain/config/genesis.json
curl -Ls https://snapshots.takeshi.team/chain4energy/addrbook.json > $HOME/.c4e-chain/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@chain4energy.rpc.takeshi.team:34659\"|" $HOME/.c4e-chain/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0uatom\"|" $HOME/.c4e-chain/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.c4e-chain/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:34658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:34657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:34060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:34656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":34660\"%" $HOME/.c4e-chain/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:34317\"%; s%^address = \":8080\"%address = \":34080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:34090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:34091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:34545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:34546\"%" $HOME/.c4e-chain/config/app.toml
```

### Download latest chain snapshot

```bash
curl -L https://snapshots.takeshi.team/chain4energy/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.c4e-chain
[[ -f $HOME/.c4e-chain/data/upgrade-info.json ]] && cp $HOME/.c4e-chain/data/upgrade-info.json $HOME/.c4e-chain/cosmovisor/genesis/upgrade-info.json
```

### Start service and check the logs

```bash
sudo systemctl start c4ed && sudo journalctl -u c4ed -f --no-hostname -o cat
```
