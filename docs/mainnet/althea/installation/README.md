---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/althea.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: althea_258432-1 | **Latest Version Tag**: v1.4.0


### Install dependencies

#### Update system and install build tools

```bash
sudo apt update && sudo apt upgrade
sudo apt -qy install curl git jq lz4 build-essential
```

#### Install Go

```bash
# Install GO 1.21.4
ver="1.21.4"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Download and build binaries

```bash
# set vars
ALTHEA_CHAIN="althea_258432-1"
ALTHEA_HOME="$HOME/.althea"

# save vars
echo "
export ALTHEA_CHAIN=${ALTHEA_CHAIN}
export ALTHEA_HOME=${ALTHEA_HOME}
" >> $HOME/.bash_profile

source $HOME/.bash_profile
```

```bash
# Clone project repository
cd $HOME
wget https://github.com/althea-net/althea-L1/releases/download/v1.4.0/althea-linux-amd64
chmod +x althea-linux-amd64
sudo mv althea-linux-amd64 /usr/sbin/althea

```

### Create a service

```bash
# Create service
tee $HOME/althead.service > /dev/null <<EOF
[Unit]
Description=Althea
After=network-online.target
[Service]
User=$USER
ExecStart=$(which althea) start --home $ALTHEA_HOME
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo mv $HOME/althead.service /etc/systemd/system/

sudo systemctl daemon-reload
sudo systemctl enable althead
```

### Initialize the node

```bash
# Set node configuration
althea config chain-id $ALTHEA_CHAIN

# Initialize the node
althea init node --chain-id $ALTHEA_CHAIN

# Download genesis 
wget -O $HOME/.althea/config/genesis.json "https://github.com/AltheaFoundation/althea-L1-docs/blob/main/althea-l1-mainnet-genesis.json"

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"a85a651a3cf1746694560c5b6f76d566c04ca581@althea-mainnet.rpc.takeshi.team:15259\"|" $HOME/.althea/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0aalthea\"|" $HOME/.althea/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.althea/config/app.toml

```

### Start service and check the logs

```bash
sudo systemctl start althead && sudo journalctl -u althead -f 
```
