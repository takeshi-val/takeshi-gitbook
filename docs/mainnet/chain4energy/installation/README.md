---
<figure><img src="https://github.com/takeshi-val/Logo/raw/main/chain4energy.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: perun-1 | **Latest Version Tag**: v1.3.1 

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
sudo apt update && sudo apt upgrade
sudo apt install curl git jq lz4 build-essential
```

#### Install GO 1.21.4

```bash
ver="1.21.4"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```

### Download and build binaries

```bash
cd $HOME
git clone --depth 1 --branch  v1.3.1  https://github.com/chain4energy/c4e-chain.git
cd c4e-chain
git checkout v1.3.1

# Install binaries
make install

#check version
c4ed version 

#version: 1.2.0


```
### Initialize the node

```bash
# Set node configuration
c4ed config chain-id perun-1

# Initialize the node
c4ed init node --chain-id $C4E_CHAIN --home $C4E_HOME

# Download genesis
wget -O $C4E_HOME/config/genesis.json "https://raw.githubusercontent.com/chain4energy/c4e-chains/main/perun-1/genesis.json"

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"a85a651a3cf1746694560c5b6f76d566c04ca581@c4e-seed.takeshi.team:10256\"|" $HOME/.c4e-chain/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.25uc4e\"|" $HOME/.c4e-chain/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.c4e-chain/config/app.toml

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
  $HOME/.c4e-chain/config/config.toml
```

### Start service and check the logs

```bash
sudo systemctl start c4ed && sudo journalctl -u c4ed -f 
```
