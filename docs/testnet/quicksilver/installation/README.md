---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/quicksilver.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: rhye-1 | **Latest Version Tag**: v1.4.4-rc.3 

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
rm -rf quicksilver
git clone https://github.com/ingenuity-build/quicksilver.git
cd quicksilver
git checkout v1.4.4-rc.3

# Install binaries
make install

```

### Create a service

```bash

# Create service
sudo tee /etc/systemd/system/quicksilverd.service > /dev/null << EOF
[Unit]
Description=quicksilver node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which quicksilverd) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.quicksilverd"
Environment="DAEMON_NAME=quicksilverd"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable quicksilverd
```

### Initialize the node

```bash
# Set node configuration
quicksilverd config chain-id rhye-1

# Initialize the node
quicksilverd init $MONIKER --chain-id rhye-1

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/quicksilver/genesis.json > $HOME/.quicksilverd/config/genesis.json
curl -Ls https://snapshots.takeshi.team/quicksilver/addrbook.json > $HOME/.quicksilverd/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@quicksilver.rpc.takeshi.team:11659\"|" $HOME/.quicksilverd/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.0001uqck\"|" $HOME/.quicksilverd/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.quicksilverd/config/app.toml

```
### Start service and check the logs

```bash
sudo systemctl start quicksilverd && sudo journalctl -u quicksilverd -f 
```
