---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenance.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: pio-mainnet-1 | **Latest Version Tag**: v1.15.0 | **Custom Port**: 37

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
rm -rf provenance-chain
git clone https://github.com/provenance-io/provenance.git
cd provenance-chain
git checkout v1.15.0

# Build binaries
make install
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

### Initialize the node

```bash
# Set node configuration
provenanced config chain-id pio-mainnet-1

# Initialize the node
provenanced init $MONIKER --chain-id pio-mainnet-1

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/provenance/genesis.json > $HOME/.provenance/config/genesis.json
curl -Ls https://snapshots.takeshi.team/provenance/addrbook.json > $HOME/.provenance/config/addrbook.json

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

### Download latest chain snapshot

```bash
curl -L https://snapshots.takeshi.team/provenance/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.provenance
[[ -f $HOME/.provenance/data/upgrade-info.json ]] && cp $HOME/.provenance/data/upgrade-info.json $HOME/.provenance/cosmovisor/genesis/upgrade-info.json
```

### Start service and check the logs

```bash
sudo systemctl start provenanced && sudo journalctl -u provenanced -f 
```
