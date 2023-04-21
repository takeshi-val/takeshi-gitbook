---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/lava.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: lava-testnet-1 | **Latest Version Tag**: v0.9.8 | **Custom Port**: 44

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
rm -rf lava
git clone https://github.com/lavanet/lava.git
cd lava
git checkout v0.9.8

# Install binaries
make install

```

### Create a service

```bash
# Create service
sudo tee /etc/systemd/system/lavad.service > /dev/null <<EOF
[Unit]
Description=lava
After=network-online.target
[Service]
User=$USER
ExecStart=$(which lavad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable lavad
```

### Initialize the node

```bash
# Set node configuration
lavad config chain-id lava-testnet-1
lavad config keyring-backend test

# Initialize the node
lavad init $MONIKER --chain-id lava-testnet-1

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/lava-testnet/genesis.json > $HOME/.lava/config/genesis.json
curl -Ls https://snapshots.takeshi.team/lava-testnet/addrbook.json > $HOME/.lava/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"3f472746f46493309650e5a033076689996c8881@lava-rpc.takeshi.team:44659\"|" $HOME/.lava/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0ulava\"|" $HOME/.lava/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.lava/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:44658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:44657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:44060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:44656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":44660\"%" $HOME/.lava/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:44317\"%; s%^address = \":8080\"%address = \":44080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:44090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:44091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:44545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:44546\"%" $HOME/.lava/config/app.toml
```

### Update chain specific configuration
```bash
sed -i 's/create_empty_blocks = .*/create_empty_blocks = true/g' $HOME/.lava/config/config.toml
sed -i 's/create_empty_blocks_interval = ".*s"/create_empty_blocks_interval = "60s"/g' $HOME/.lava/config/config.toml
sed -i 's/timeout_propose = ".*s"/timeout_propose = "60s"/g' $HOME/.lava/config/config.toml
sed -i 's/timeout_commit = ".*s"/timeout_commit = "60s"/g' $HOME/.lava/config/config.toml
sed -i 's/timeout_broadcast_tx_commit = ".*s"/timeout_broadcast_tx_commit = "601s"/g' $HOME/.lava/config/config.toml
```

### Start service and check the logs

```bash
sudo systemctl start lavad && sudo journalctl -u lavad -f --no-hostname -o cat
```
