---
description: >-
  Useful set of commands for node operators. From key management to chain
  governance.
---

# Useful commands

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/kichain.png" alt=""><figcaption></figcaption></figure>

## 🔑 Key management

#### Add new key

```bash
kid keys add wallet
```

#### Recover existing key

```bash
kid keys add wallet --recover
```

#### List all keys

```bash
kid keys list
```

#### Delete key

```bash
kid keys delete wallet
```

#### Export key to the file

```bash
kid keys export wallet
```

#### Import key from the file

```bash
kid keys import wallet wallet.backup
```

#### Query wallet balance

```bash
kid q bank balances $(kid keys show wallet -a)
```

## 👷 Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```bash
kid tx staking create-validator \
--amount=1000000uxki \
--pubkey=$(kid tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=kichain-2 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0.025uxki \
-y
```

#### Edit existing validator

```bash
kid tx staking edit-validator \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=kichain-2 \
--commission-rate=0.05 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0.025uxki \
-y
```

#### Unjail validator

```bash
kid tx slashing unjail --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Jail reason

```bash
kid query slashing signing-info $(kid tendermint show-validator)
```

#### List all active validators

```bash
kid q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```bash
kid q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```bash
kid q staking validator $(kid keys show wallet --bech val -a)
```

## 💲 Token management

#### Withdraw rewards from all validators

```bash
kid tx distribution withdraw-all-rewards --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Withdraw commission and rewards from your validator

```bash
kid tx distribution withdraw-rewards $(kid keys show wallet --bech val -a) --commission --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Delegate tokens to yourself

```bash
kid tx staking delegate $(kid keys show wallet --bech val -a) 1000000uxki --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Delegate tokens to validator

```bash
kid tx staking delegate <TO_VALOPER_ADDRESS> 1000000uxki --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Redelegate tokens to another validator

```bash
kid tx staking redelegate $(kid keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uxki --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Unbond tokens from your validator

```bash
kid tx staking unbond $(kid keys show wallet --bech val -a) 1000000uxki --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Send tokens to the wallet

```bash
kid tx bank send wallet <TO_WALLET_ADDRESS> 1000000uxki --from wallet --chain-id kichain-2
```

## 🗳 Governance

#### List all proposals

```bash
kid query gov proposals
```

#### View proposal by id

```bash
kid query gov proposal 1
```

#### Vote 'Yes'

```bash
kid tx gov vote 1 yes --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Vote 'No'

```bash
kid tx gov vote 1 no --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Vote 'Abstain'

```bash
kid tx gov vote 1 abstain --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

#### Vote 'NoWithVeto'

```bash
kid tx gov vote 1 nowithveto --from wallet --chain-id kichain-2 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uxki -y
```

## ⚡️ Utility

#### Update ports

```bash
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.kid/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.kid/config/app.toml
```

#### Update Indexer

**Disable indexer**

```bash
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.kid/config/config.toml
```

**Enable indexer**

```bash
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.kid/config/config.toml
```

#### Update pruning

```bash
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.kid/config/app.toml
```

## 🚨 Maintenance

#### Get validator info

```bash
kid status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```bash
kid status 2>&1 | jq .SyncInfo
```

#### Get node peer

```bash
echo $(kid tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.kid/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```bash
[[ $(kid q staking validator $(kid keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(kid status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```bash
curl -sS http://localhost:27657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0uxki\"/" $HOME/.kid/config/app.toml
```

#### Enable prometheus

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.kid/config/config.toml
```

#### Reset chain data

```bash
kid tendermint unsafe-reset-all --home $HOME/.kid --keep-addr-book
```

#### Remove node

{% hint style="danger" %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv\_validator\_key.json**!
{% endhint %}

```bash
cd $HOME
sudo systemctl stop kid
sudo systemctl disable kid
sudo rm /etc/systemd/system/kid.service
sudo systemctl daemon-reload
rm -f $(which kid)
rm -rf $HOME/.kid
rm -rf $HOME/kichain-sdk
```

## ⚙️ Service Management

#### Reload service configuration

```bash
sudo systemctl daemon-reload
```

#### Enable service

```bash
sudo systemctl enable kid
```

#### Disable service

```bash
sudo systemctl disable kid
```

#### Start service

```bash
sudo systemctl start kid
```

#### Stop service

```bash
sudo systemctl stop kid
```

#### Restart service

```bash
sudo systemctl restart kid
```

#### Check service status

```bash
sudo systemctl status kid
```

#### Check service logs

```bash
sudo journalctl -u kid -f --no-hostname -o cat
```
