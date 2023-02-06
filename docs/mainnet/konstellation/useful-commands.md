---
description: >-
  Useful set of commands for node operators. From key management to chain
  governance.
---

# Useful commands

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/konstellation.png" alt=""><figcaption></figcaption></figure>

## 🔑 Key management

#### Add new key

```bash
knstld keys add wallet
```

#### Recover existing key

```bash
knstld keys add wallet --recover
```

#### List all keys

```bash
knstld keys list
```

#### Delete key

```bash
knstld keys delete wallet
```

#### Export key to the file

```bash
knstld keys export wallet
```

#### Import key from the file

```bash
knstld keys import wallet wallet.backup
```

#### Query wallet balance

```bash
knstld q bank balances $(knstld keys show wallet -a)
```

## 👷 Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```bash
knstld tx staking create-validator \
--amount=1000000ukuji \
--pubkey=$(knstld tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=darchub \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0.00119ukuji \
-y
```

#### Edit existing validator

```bash
knstld tx staking edit-validator \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=darchub \
--commission-rate=0.05 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0.00119ukuji \
-y
```

#### Unjail validator

```bash
knstld tx slashing unjail --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Jail reason

```bash
knstld query slashing signing-info $(knstld tendermint show-validator)
```

#### List all active validators

```bash
knstld q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```bash
knstld q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```bash
knstld q staking validator $(knstld keys show wallet --bech val -a)
```

## 💲 Token management

#### Withdraw rewards from all validators

```bash
knstld tx distribution withdraw-all-rewards --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Withdraw commission and rewards from your validator

```bash
knstld tx distribution withdraw-rewards $(knstld keys show wallet --bech val -a) --commission --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Delegate tokens to yourself

```bash
knstld tx staking delegate $(knstld keys show wallet --bech val -a) 1000000ukuji --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Delegate tokens to validator

```bash
knstld tx staking delegate <TO_VALOPER_ADDRESS> 1000000ukuji --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Redelegate tokens to another validator

```bash
knstld tx staking redelegate $(knstld keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ukuji --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Unbond tokens from your validator

```bash
knstld tx staking unbond $(knstld keys show wallet --bech val -a) 1000000ukuji --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Send tokens to the wallet

```bash
knstld tx bank send wallet <TO_WALLET_ADDRESS> 1000000ukuji --from wallet --chain-id darchub
```

## 🗳 Governance

#### List all proposals

```bash
knstld query gov proposals
```

#### View proposal by id

```bash
knstld query gov proposal 1
```

#### Vote 'Yes'

```bash
knstld tx gov vote 1 yes --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Vote 'No'

```bash
knstld tx gov vote 1 no --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Vote 'Abstain'

```bash
knstld tx gov vote 1 abstain --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

#### Vote 'NoWithVeto'

```bash
knstld tx gov vote 1 nowithveto --from wallet --chain-id darchub --gas-adjustment 1.4 --gas auto --gas-prices 0.00119ukuji -y
```

## ⚡️ Utility

#### Update ports

```bash
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.knstld/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.knstld/config/app.toml
```

#### Update Indexer

**Disable indexer**

```bash
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.knstld/config/config.toml
```

**Enable indexer**

```bash
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.knstld/config/config.toml
```

#### Update pruning

```bash
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.knstld/config/app.toml
```

## 🚨 Maintenance

#### Get validator info

```bash
knstld status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```bash
knstld status 2>&1 | jq .SyncInfo
```

#### Get node peer

```bash
echo $(knstld tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.knstld/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```bash
[[ $(knstld q staking validator $(knstld keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(knstld status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```bash
curl -sS http://localhost:13657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0ukuji\"/" $HOME/.knstld/config/app.toml
```

#### Enable prometheus

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.knstld/config/config.toml
```

#### Reset chain data

```bash
knstld tendermint unsafe-reset-all --home $HOME/.knstld --keep-addr-book
```

#### Remove node

{% hint style="danger" %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv\_validator\_key.json**!
{% endhint %}

```bash
cd $HOME
sudo systemctl stop knstld
sudo systemctl disable knstld
sudo rm /etc/systemd/system/knstld.service
sudo systemctl daemon-reload
rm -f $(which knstld)
rm -rf $HOME/.knstld
rm -rf $HOME/konstellation
```

## ⚙️ Service Management

#### Reload service configuration

```bash
sudo systemctl daemon-reload
```

#### Enable service

```bash
sudo systemctl enable knstld
```

#### Disable service

```bash
sudo systemctl disable knstld
```

#### Start service

```bash
sudo systemctl start knstld
```

#### Stop service

```bash
sudo systemctl stop knstld
```

#### Restart service

```bash
sudo systemctl restart knstld
```

#### Check service status

```bash
sudo systemctl status knstld
```

#### Check service logs

```bash
sudo journalctl -u knstld -f --no-hostname -o cat
```
