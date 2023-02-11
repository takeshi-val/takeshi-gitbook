---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/chain4energy.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: perun-1 | **Latest Version Tag**: v1.1.0 

### Setup validator name

```bash
C4E_HOME="$HOME/.c4e-chain"
C4E_CHAIN="perun-1"
C4E_MONIKER="YOUR_MONIKER"
C4E_WALLET="WALLET_NAME"

echo 'export C4E_HOME='${C4E_HOME} >> $HOME/.bash_profile
echo 'export C4E_CHAIN='${C4E_CHAIN} >> $HOME/.bash_profile
echo 'export C4E_MONIKER='${C4E_MONIKER} >> $HOME/.bash_profile
echo 'export C4E_WALLET='${C4E_WALLET} >> $HOME/.bash_profile
source $HOME/.bash_profile
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
git clone --depth 1 --branch  v1.1.0  https://github.com/chain4energy/c4e-chain.git
cd c4e-chain
gti checkout v1.1.0

# Install binaries
make install

#check version
c4ed version --long

#server_name: c4ed
#version: 1.1.0
#commit: d67fd60d07b41c52977539b9fb9c0c67de23837e

```
### Initialize the node

```bash
# Set node configuration
c4ed config chain-id perun-1
c4ed config keyring-backend file
c4ed config node tcp://localhost:34657

# Initialize the node
c4ed init node --chain-id $C4E_CHAIN --home $C4E_HOME

# Download genesis
wget -O $C4E_HOME/config/genesis.json "https://raw.githubusercontent.com/chain4energy/c4e-chains/main/perun-1/genesis.json"

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"a85a651a3cf1746694560c5b6f76d566c04ca581@c4e-seed.takeshi.team:10256\"|" $HOME/.c4e-chain/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0uc4e\"|" $HOME/.c4e-chain/config/app.toml

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

### Create a service

```bash

# Create service
tee $HOME/c4ed.service > /dev/null <<EOF
[Unit]
Description=c4ed
After=network.target
[Service]
Type=simple
User=$USER
ExecStart=$(which c4ed) start 
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo mv $HOME/c4ed.service /etc/systemd/system/

sudo systemctl daemon-reload
sudo systemctl enable c4ed
```


### Use state sync for start 

```bash
STATE_SYNC_RPC=https://rpc-c4e.takeshi.team:443

LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 2000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i \
  -e "s|^enable *=.*|enable = true|" \
  -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.c4e-chain/config/config.toml
```

### Start service and check the logs

```bash
sudo systemctl start c4ed && sudo journalctl -u c4ed -f 
```
