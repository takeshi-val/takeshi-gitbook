---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/kichain.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: kichain-2 | **Latest Version Tag**: v4.2.0 | **Custom Port**: 27

### Setup your VARS

```bash
# Set VARS
KI_CHAIN=kichain-2
KI_NODENAME="YOUR_NODE_NAME"
KI_WALLET="YOUR_WALLET_NAME"

echo 'export KI_CHAIN='$KI_CHAIN >> $HOME/.bash_profile
echo 'export KI_NODENAME='${KI_NODENAME} >> $HOME/.bash_profile
echo 'export KI_WALLET='${KI_WALLET} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

#### Update system and install build tools

```bash
sudo apt -q update && sudo apt -qy upgrade
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
git clone https://github.com/KiFoundation/ki-tools.git
git checkout main
git pull
git checkout 4.2.0
make install
cd $HOME

kid version
#Mainnet-4.2.0

```

### Сreate a service

```bash
# Create service
sudo tee /etc/systemd/system/kid.service > /dev/null << EOF
[Unit]
Description=kichain node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which kid) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.kid"
Environment="DAEMON_NAME=kid"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable kid
```

### Initialize the node

```bash
# Set node configuration
kid config chain-id $KI_CHAIN
kid config keyring-backend file
kid config node tcp://localhost:27657

# Initialize the node
kid init $KI_NODENAME --chain-id $KI_CHAIN

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/kichain/genesis.json > $HOME/.kid/config/genesis.json
curl -Ls https://snapshots.takeshi.team/kichain/addrbook.json > $HOME/.kid/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@kichain.rpc.takeshi.team:27659\"|" $HOME/.kid/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.025uxki\"|" $HOME/.kid/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.kid/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:27658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:27657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:27060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:27656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":27660\"%" $HOME/.kid/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:27317\"%; s%^address = \":8080\"%address = \":27080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:27090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:27091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:27545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:27546\"%" $HOME/.kid/config/app.toml
```

### Download latest chain snapshot

```bash
curl -L https://snapshots.takeshi.team/kichain/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.kid
mv $HOME/.kid/priv_validator_state.json.backup $HOME/.kid/data/priv_validator_state.json
```

### Start service and check the logs

```bash
sudo systemctl start kid && sudo journalctl -u kid -f --no-hostname -o cat
```
