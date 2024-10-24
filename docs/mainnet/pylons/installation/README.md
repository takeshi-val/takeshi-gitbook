---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/pylons.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: reb\_1111-1 | **Latest Version Tag**: v0.2.0 | **Custom Port**: 21

### Setup validator name

{% hint style="info" %}
Replace **YOUR\_MONIKER** with your validator name
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
rm -rf pylons
git clone https://github.com/pylonschain/pylons.git
cd pylons
git checkout v0.2.0

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.pylons/cosmovisor/genesis/bin
mv build/pylonsd $HOME/.pylons/cosmovisor/genesis/bin/
rm -rf build

# Create application symlinks
ln -s $HOME/.pylons/cosmovisor/genesis $HOME/.pylons/cosmovisor/current
sudo ln -s $HOME/.pylons/cosmovisor/current/bin/pylonsd /usr/local/bin/pylonsd
```

### Install Cosmovisor and create a service

```bash
# Download and install Cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.4.0

# Create service
sudo tee /etc/systemd/system/pylonsd.service > /dev/null << EOF
[Unit]
Description=pylons node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.pylons"
Environment="DAEMON_NAME=pylonsd"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable pylonsd
```

### Initialize the node

```bash
# Set node configuration
pylonsd config chain-id reb_1111-1
pylonsd config keyring-backend file
pylonsd config node tcp://localhost:21657

# Initialize the node
pylonsd init $MONIKER --chain-id reb_1111-1

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/pylons/genesis.json > $HOME/.pylons/config/genesis.json
curl -Ls https://snapshots.takeshi.team/pylons/addrbook.json > $HOME/.pylons/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@pylons.rpc.takeshi.team:21659\"|" $HOME/.pylons/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0apylons\"|" $HOME/.pylons/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.pylons/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:21658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:21657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:21060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:21656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":21660\"%" $HOME/.pylons/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:21317\"%; s%^address = \":8080\"%address = \":21080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:21090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:21091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:21545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:21546\"%" $HOME/.pylons/config/app.toml
```

### Download latest chain snapshot

```bash
curl -L https://snapshots.takeshi.team/pylons/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.pylons
[[ -f $HOME/.pylons/data/upgrade-info.json ]] && cp $HOME/.pylons/data/upgrade-info.json $HOME/.pylons/cosmovisor/genesis/upgrade-info.json
```

### Start service and check the logs

```bash
sudo systemctl start pylonsd && sudo journalctl -u pylonsd -f --no-hostname -o cat
```
