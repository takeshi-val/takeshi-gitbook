---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/beezee.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: beezee-1 | **Latest Version Tag**: v1.0.0 | **Custom Port**: 45

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
rm -rf hub
git clone https://github.com/beezee-protocol/hub.git
cd hub
git checkout v1.0.0

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.bze/cosmovisor/genesis/bin
mv build/bzed $HOME/.bze/cosmovisor/genesis/bin/
rm -rf build

# Create application symlinks
ln -s $HOME/.bze/cosmovisor/genesis $HOME/.bze/cosmovisor/current
sudo ln -s $HOME/.bze/cosmovisor/current/bin/bzed /usr/local/bin/bzed
```

### Install Cosmovisor and create a service

```bash
# Download and install Cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.4.0

# Create service
sudo tee /etc/systemd/system/bzed.service > /dev/null << EOF
[Unit]
Description=beezee node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.bze"
Environment="DAEMON_NAME=bzed"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable bzed
```

### Initialize the node

```bash
# Set node configuration
bzed config chain-id beezee-1
bzed config keyring-backend file
bzed config node tcp://localhost:45657

# Initialize the node
bzed init $MONIKER --chain-id beezee-1

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/beezee/genesis.json > $HOME/.bze/config/genesis.json
curl -Ls https://snapshots.takeshi.team/beezee/addrbook.json > $HOME/.bze/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@beezee.rpc.takeshi.team:45659\"|" $HOME/.bze/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0ubeezee\"|" $HOME/.bze/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.bze/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:45658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:45657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:45060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:45656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":45660\"%" $HOME/.bze/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:45317\"%; s%^address = \":8080\"%address = \":45080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:45090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:45091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:45545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:45546\"%" $HOME/.bze/config/app.toml
```

### Download latest chain snapshot

```bash
curl -L https://snapshots.takeshi.team/beezee/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.bze
[[ -f $HOME/.bze/data/upgrade-info.json ]] && cp $HOME/.bze/data/upgrade-info.json $HOME/.bze/cosmovisor/genesis/upgrade-info.json
```

### Start service and check the logs

```bash
sudo systemctl start bzed && sudo journalctl -u bzed -f --no-hostname -o cat
```
