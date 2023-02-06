---
description: Useful set of commands for node operators. From key management to chain governance.
---

# Useful commands

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/beezee.png" width="150" alt=""><figcaption></figcaption></figure>

## 🔑 Key management

#### Add new key

```bash
bzed keys add wallet
```

#### Recover existing key

```bash
bzed keys add wallet --recover
```

#### List all keys

```bash
bzed keys list
```

#### Delete key

```bash
bzed keys delete wallet
```

#### Export key to the file

```bash
bzed keys export wallet
```

#### Import key from the file

```bash
bzed keys import wallet wallet.backup
```

#### Query wallet balance

```bash
bzed q bank balances $(bzed keys show wallet -a)
```

## 👷 Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```bash
bzed tx staking create-validator \
--amount=1000000ubeezee \
--pubkey=$(bzed tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=beezee-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0ubeezee \
-y
```

#### Edit existing validator

```bash
bzed tx staking edit-validator \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=beezee-1 \
--commission-rate=0.05 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0ubeezee \
-y
```

#### Unjail validator

```bash
bzed tx slashing unjail --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Jail reason

```bash
bzed query slashing signing-info $(bzed tendermint show-validator)
```

#### List all active validators

```bash
bzed q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```bash
bzed q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```bash
bzed q staking validator $(bzed keys show wallet --bech val -a)
```

## 💲 Token management

#### Withdraw rewards from all validators

```bash
bzed tx distribution withdraw-all-rewards --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Withdraw commission and rewards from your validator

```bash
bzed tx distribution withdraw-rewards $(bzed keys show wallet --bech val -a) --commission --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Delegate tokens to yourself

```bash
bzed tx staking delegate $(bzed keys show wallet --bech val -a) 1000000ubeezee --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Delegate tokens to validator

```bash
bzed tx staking delegate <TO_VALOPER_ADDRESS> 1000000ubeezee --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Redelegate tokens to another validator

```bash
bzed tx staking redelegate $(bzed keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ubeezee --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Unbond tokens from your validator

```bash
bzed tx staking unbond $(bzed keys show wallet --bech val -a) 1000000ubeezee --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Send tokens to the wallet

```bash
bzed tx bank send wallet <TO_WALLET_ADDRESS> 1000000ubeezee --from wallet --chain-id beezee-1
```

## 🗳 Governance

#### List all proposals

```bash
bzed query gov proposals
```

#### View proposal by id

```bash
bzed query gov proposal 1
```

#### Vote 'Yes'

```bash
bzed tx gov vote 1 yes --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Vote 'No'

```bash
bzed tx gov vote 1 no --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Vote 'Abstain'

```bash
bzed tx gov vote 1 abstain --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

#### Vote 'NoWithVeto'

```bash
bzed tx gov vote 1 nowithveto --from wallet --chain-id beezee-1 --gas-adjustment 1.4 --gas auto --gas-prices 0ubeezee -y
```

## ⚡️ Utility

#### Update ports

```bash
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.bze/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.bze/config/app.toml
```

#### Update Indexer

##### Disable indexer

```bash
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.bze/config/config.toml
```

##### Enable indexer

```bash
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.bze/config/config.toml
```

#### Update pruning

```bash
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.bze/config/app.toml
```

## 🚨 Maintenance

#### Get validator info

```bash
bzed status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```bash
bzed status 2>&1 | jq .SyncInfo
```

#### Get node peer

```bash
echo $(bzed tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.bze/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```bash
[[ $(bzed q staking validator $(bzed keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(bzed status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```bash
curl -sS http://localhost:45657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0ubeezee\"/" $HOME/.bze/config/app.toml
```

#### Enable prometheus

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.bze/config/config.toml
```

#### Reset chain data

```bash
bzed tendermint unsafe-reset-all --home $HOME/.bze --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```bash
cd $HOME
sudo systemctl stop bzed
sudo systemctl disable bzed
sudo rm /etc/systemd/system/bzed.service
sudo systemctl daemon-reload
rm -f $(which bzed)
rm -rf $HOME/.bze
rm -rf $HOME/hub
```

## ⚙️ Service Management

#### Reload service configuration

```bash
sudo systemctl daemon-reload
```

#### Enable service

```bash
sudo systemctl enable bzed
```

#### Disable service

```bash
sudo systemctl disable bzed
```

#### Start service

```bash
sudo systemctl start bzed
```

#### Stop service

```bash
sudo systemctl stop bzed
```

#### Restart service

```bash
sudo systemctl restart bzed
```

#### Check service status

```bash
sudo systemctl status bzed
```

#### Check service logs

```bash
sudo journalctl -u bzed -f --no-hostname -o cat
```
