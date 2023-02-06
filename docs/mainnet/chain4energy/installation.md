---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/logo_C4E.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: perun-1 | **Latest Version Tag**: v1.1.0 | **Custom Port**: 19

### Setup validator name

{% hint style="info" %}
Replace **YOUR\_MONIKER\_GOES\_HERE** with your validator name
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
rm -rf teritori-chain
git clone https://github.com/TERITORI/teritori-chain.git
cd teritori-chain
git checkout v1.1.0

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.c4e-chain/cosmovisor/genesis/bin
mv build/c4ed $HOME/.c4e-chain/cosmovisor/genesis/bin/
rm -rf build

# Create application symlinks
ln -s $HOME/.c4e-chain/cosmovisor/genesis $HOME/.c4e-chain/cosmovisor/current
sudo ln -s $HOME/.c4e-chain/cosmovisor/current/bin/c4ed /usr/local/bin/c4ed
```

### Install Cosmovisor and create a service

```bash
# Download and install Cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.4.0

# Create service
sudo tee /etc/systemd/system/c4ed.service > /dev/null << EOF
[Unit]
Description=teritori node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
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
c4ed config node tcp://localhost:19657

# Initialize the node
c4ed init $MONIKER --chain-id perun-1

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/teritori/genesis.json > $HOME/.c4e-chain/config/genesis.json
curl -Ls https://snapshots.takeshi.team/teritori/addrbook.json > $HOME/.c4e-chain/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@rpc-c4e.takeshi.team:19659\"|" $HOME/.c4e-chain/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0utori\"|" $HOME/.c4e-chain/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.c4e-chain/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:19658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:19657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:19060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:19656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":19660\"%" $HOME/.c4e-chain/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:19317\"%; s%^address = \":8080\"%address = \":19080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:19090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:19091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:19545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:19546\"%" $HOME/.c4e-chain/config/app.toml
```

### Download latest chain snapshot

```bash
curl -L https://snapshots.takeshi.team/teritori/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.c4e-chain
[[ -f $HOME/.c4e-chain/data/upgrade-info.json ]] && cp $HOME/.c4e-chain/data/upgrade-info.json $HOME/.c4e-chain/cosmovisor/genesis/upgrade-info.json
```

### Start service and check the logs

```bash
sudo systemctl start c4ed && sudo journalctl -u c4ed -f --no-hostname -o cat
```
