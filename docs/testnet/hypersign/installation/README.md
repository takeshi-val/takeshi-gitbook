---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/hypersign.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: prajna-1 | **Latest Version Tag**: v0.2.0 

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
rm -rf hid-node
git clone https://github.com/hypersign-protocol/hid-node.git
cd hid-node
git checkout v0.2.0

# Install binaries
make install
```

### Create a service

```bash
sudo tee /etc/systemd/system/hid-noded.service > /dev/null << EOF
[Unit]
Description=hypersign-testnet node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which hid-noded) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.hid-node"
Environment="DAEMON_NAME=hid-noded"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable hid-noded
```

### Initialize the node

```bash
# Initialize the node
hid-noded init $MONIKER --chain-id prajna-1

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/hypersign-testnet/genesis.json > $HOME/.hid-node/config/genesis.json
curl -Ls https://snapshots.takeshi.team/hypersign-testnet/addrbook.json > $HOME/.hid-node/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"a85a651a3cf1746694560c5b6f76d566c04ca581@hypersign-testnet.rpc.takeshi.team:31659\"|" $HOME/.hid-node/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0uhid\"|" $HOME/.hid-node/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.hid-node/config/app.toml
```

### Start service and check the logs

```bash
sudo systemctl start hid-noded && sudo journalctl -u hid-noded -f 
```
