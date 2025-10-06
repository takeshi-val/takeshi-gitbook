---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenanced_logo_name.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: pio-mainnet-1 | **Latest Version Tag**: v1.25.0 | 

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
# Clone project repository
export PIO_HOME=~/.provenanced
git clone https://github.com/provenance-io/provenance
cd provenance
git checkout v1.25.0


# Build binaries
make install
```

### Initialize the node

```bash
# Set node configuration
provenanced config chain-id pio-mainnet-1

# Initialize the node
provenanced init $MONIKER --chain-id pio-mainnet-1

# Download genesis and addrbook
wget -O $HOME/.provenanced/config/addrbook.json "https://snapshots.takeshi.team/provenance/genesis.json"
wget -O $HOME/.provenanced/config/addrbook.json "https://snapshots.takeshi.team/provenance/addrbook.json"

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"a85a651a3cf1746694560c5b6f76d566c04ca581@provenance-seed.takeshi.team:10556\"|" $HOME/.provenance/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.002ujkl\"|" $HOME/.provenance/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.provenance/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:37658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:37657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:37060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:37656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":37660\"%" $HOME/.provenance/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:37317\"%; s%^address = \":8080\"%address = \":37080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:37090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:37091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:37545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:37546\"%" $HOME/.provenance/config/app.toml
```

### Create a service

```bash
# Create service
sudo tee /etc/systemd/system/provenanced.service > /dev/null << EOF
[Unit]
Description=provenance node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which provenanced) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.provenance"
Environment="DAEMON_NAME=provenanced"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable provenanced
```

### Download latest chain snapshot

```bash

# Backup validator
cp $HOME/.provenanced/data/priv_validator_state.json $HOME/.provenanced/priv_validator_state.json.backup

# Remove old data
rm -rf $HOME/.provenanced/data
rm -rf $HOME/.provenanced/wasm

# Download and extract the latest snapshot
curl -L https://snapshots.takeshi.team/provenance/provenance_latest.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.provenanced

# Restore validator
mv $HOME/.provenanced/priv_validator_state.json.backup $HOME/.provenanced/data/priv_validator_state.json

```

### Start service and check the logs

```bash
sudo systemctl start provenanced && sudo journalctl -u provenanced -f 
```
