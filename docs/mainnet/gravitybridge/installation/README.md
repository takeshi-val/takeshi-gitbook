---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/gravitybridge.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: gravity-bridge-3 | **Latest Version Tag**: v1.13.3 | 

### Install dependencies

#### Update system and install build tools

```bash
sudo apt -q update
sudo apt -qy install curl git jq lz4 build-essential
sudo apt -qy upgrade
```

#### Install Go

```bash
cd $HOME
ver="1.23.3"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Download and build binaries

```bash
cd $HOME
git clone https://github.com/Gravity-Bridge/Gravity-Bridge
mv Gravity-Bridge gravity
cd $HOME/gravity/module
git pull
git reset --hard
git checkout v1.13.3
make install
```
### Initialize the node

```bash
# Initialize the node
gravityd init $MONIKER --chain-id gravity-bridge-3

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/gravitybridge/genesis.json > $HOME/.gravity/config/genesis.json
curl -Ls https://snapshots.takeshi.team/gravitybridge/addrbook.json > $HOME/.gravity/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@gravitybridge.rpc.takeshi.team:26659\"|" $HOME/.gravity/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0ugraviton\"|" $HOME/.gravity/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.gravity/config/app.toml

```

### Create a service

```bash

# Create service
sudo tee /etc/systemd/system/gravityd.service > /dev/null << EOF
[Unit]
Description=gravitybridge node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.gravity"
Environment="DAEMON_NAME=gravityd"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable gravityd
```



### Download latest chain snapshot

```bash
curl -L https://snapshots.takeshi.team/gravitybridge/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.gravity
[[ -f $HOME/.gravity/data/upgrade-info.json ]] && cp $HOME/.gravity/data/upgrade-info.json $HOME/.gravity/cosmovisor/genesis/upgrade-info.json
```

### Start service and check the logs

```bash
sudo systemctl start gravityd && sudo journalctl -u gravityd -f --no-hostname -o cat
```
