---

---

# Installation

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/warden.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: buenavista-1 | **Latest Version Tag**: v0.3.0 

### Prepare vars

```bash
# set vars
WARDEN_CHAIN="buenavista-1"
WARDEN_HOME="$HOME/.warden"
WARDEN_MONIKER="YOUR_MONIKER"
WARDEN_WALLET="YOUR_WALLET"

# save vars
echo 'export WARDEN_CHAIN='${WARDEN_CHAIN} >> $HOME/.bash_profile
echo 'export WARDEN_HOME='${WARDEN_HOME} >> $HOME/.bash_profile
echo 'export WARDEN_MONIKER='${WARDEN_MONIKER} >> $HOME/.bash_profile
echo 'export WARDEN_WALLET='${WARDEN_WALLET} >> $HOME/.bash_profile

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
cd $HOME
ver="1.21.6"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile

go version
```

### Download and install binaries

```bash
# Install
cd $HOME
git clone https://github.com/warden-protocol/wardenprotocol.git
cd wardenprotocol
git checkout v0.3.0
make install

```

```bash
# Create service
tee $HOME/wardend.service > /dev/null <<EOF
[Unit]
Description=wardend
After=network-online.target
[Service]
User=$USER
ExecStart=$(which wardend) start --home $WARDEN_HOME
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo mv $HOME/wardend.service /etc/systemd/system/

sudo systemctl enable wardend
sudo systemctl daemon-reload
```

### Initialize the node

```bash
# Set node configuration
wardend config chain-id $WARDEN_CHAIN

# Initialize the node
wardend init $WARDEN_MONIKER --chain-id $WARDEN_CHAIN

# Download genesis and addrbook
curl -Ls https://snapshots.takeshi.team/warden/genesis.json > $HOME/.warden/config/genesis.json
curl -Ls https://snapshots.takeshi.team/warden/addrbook.json > $HOME/.warden/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"a85a651a3cf1746694560c5b6f76d566c04ca581@warden-rpc.takeshi.team:24459\"|" $HOME/.wardend/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.025uward\"|" $HOME/.wardend/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.wardend/config/app.toml

```
### Start service and check the logs

```bash
sudo systemctl start wardend && sudo journalctl -u wardend -f
```
