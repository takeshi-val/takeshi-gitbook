---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/canto.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: canto_7700-1 | **Latest Version Tag**: v1.5.3 | **Custom Port**: 42

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
rm -rf bcna
git clone https://github.com/cantoGlobal/bcna.git
cd bcna
git checkout v1.5.3

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.cantod/cosmovisor/genesis/bin
mv build/cantod $HOME/.cantod/cosmovisor/genesis/bin/
rm -rf build

# Create application symlinks
ln -s $HOME/.cantod/cosmovisor/genesis $HOME/.cantod/cosmovisor/current
sudo ln -s $HOME/.cantod/cosmovisor/current/bin/cantod /usr/local/bin/cantod
```

### Install Cosmovisor and create a service

```bash
# Download and install Cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.4.0

# Create service
sudo tee /etc/systemd/system/cantod.service > /dev/null << EOF
[Unit]
Description=canto node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.cantod"
Environment="DAEMON_NAME=cantod"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable cantod
```

### Initialize the node

```bash
# Set node configuration
cantod config chain-id canto_7700-1
cantod config keyring-backend file
cantod config node tcp://localhost:42657

# Initialize the node
cantod init $MONIKER --chain-id canto_7700-1

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/canto/genesis.json > $HOME/.cantod/config/genesis.json
curl -Ls https://snapshots.takeshi.team/canto/addrbook.json > $HOME/.cantod/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@canto.rpc.takeshi.team:42659\"|" $HOME/.cantod/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0ubcna\"|" $HOME/.cantod/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.cantod/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:42658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:42657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:42060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:42656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":42660\"%" $HOME/.cantod/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:42317\"%; s%^address = \":8080\"%address = \":42080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:42090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:42091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:42545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:42546\"%" $HOME/.cantod/config/app.toml
```

### Download latest chain snapshot

```bash
curl -L https://snapshots.takeshi.team/canto/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.cantod
[[ -f $HOME/.cantod/data/upgrade-info.json ]] && cp $HOME/.cantod/data/upgrade-info.json $HOME/.cantod/cosmovisor/genesis/upgrade-info.json
```

### Start service and check the logs

```bash
sudo systemctl start cantod && sudo journalctl -u cantod -f --no-hostname -o cat
```
