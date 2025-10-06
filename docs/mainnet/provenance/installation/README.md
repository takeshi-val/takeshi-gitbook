
<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenanced_logo_name.png" alt="" width="150"><figcaption></figcaption></figure>

**Chain ID**: pio-mainnet-1 | **Latest Version Tag**: v1.25.0 |

### Install dependencies

**Update system and install build tools**

```bash
sudo apt -q update
sudo apt -qy install curl git jq lz4 build-essential
sudo apt -qy upgrade
```

**Install Go**

{% code overflow="wrap" %}
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
{% endcode %}

**Download and build binaries**

```bash
export PIO_HOME=~/.provenanced
git clone https://github.com/provenance-io/provenance
cd provenance
git checkout v1.25.0
make install
```

**Initialize the node**

{% code overflow="wrap" fullWidth="true" %}
```bash
# Set node configuration
provenanced config chain-id pio-mainnet-1

# Initialize the node
provenanced init MONIKER --chain-id pio-mainnet-1

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
```
{% endcode %}

**Download genesis and addrbook**

{% code overflow="wrap" %}
```bash
wget -O $HOME/.provenanced/config/genesis.json "https://snapshots.takeshi.team/provenance/genesis.json"
wget -O $HOME/.provenanced/config/addrbook.json "https://snapshots.takeshi.team/provenance/addrbook.json"
```
{% endcode %}

**Create a service**

```bash
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
```

**Start service and check the logs**

```bash
sudo systemctl daemon-reload
sudo systemctl enable provenanced
sudo systemctl start provenanced && sudo journalctl -u provenanced -f 
```
