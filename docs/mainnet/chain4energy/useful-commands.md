---
description: >-
  Useful set of commands for node operators. From key management to chain
  governance.
---

# Useful commands

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/logo_C4E.png" alt=""><figcaption></figcaption></figure>

## 🔑 Key management

#### Add new key

```bash
c4ed keys add wallet
```

#### Recover existing key

```bash
c4ed keys add wallet --recover
```

#### List all keys

```bash
c4ed keys list
```

#### Delete key

```bash
c4ed keys delete wallet
```

#### Export key to the file

```bash
c4ed keys export wallet
```

#### Import key from the file

```bash
c4ed keys import wallet wallet.backup
```

#### Query wallet balance

```bash
c4ed q bank balances $(c4ed keys show wallet -a)
```

## 👷 Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```bash
c4ed tx staking create-validator \
--amount=1000000utori \
--pubkey=$(c4ed tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=perun-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0utori \
-y
```

#### Edit existing validator

```bash
c4ed tx staking edit-validator \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=perun-1 \
--commission-rate=0.05 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0utori \
-y
```

#### Unjail validator

```bash
c4ed tx slashing unjail --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Jail reason

```bash
c4ed query slashing signing-info $(c4ed tendermint show-validator)
```

#### List all active validators

```bash
c4ed q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```bash
c4ed q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```bash
c4ed q staking validator $(c4ed keys show wallet --bech val -a)
```

## 💲 Token management

#### Withdraw rewards from all validators

```bash
c4ed tx distribution withdraw-all-rewards --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Withdraw commission and rewards from your validator

```bash
c4ed tx distribution withdraw-rewards $(c4ed keys show wallet --bech val -a) --commission --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Delegate tokens to yourself

```bash
c4ed tx staking delegate $(c4ed keys show wallet --bech val -a) 1000000utori --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Delegate tokens to validator

```bash
c4ed tx staking delegate <TO_VALOPER_ADDRESS> 1000000utori --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Redelegate tokens to another validator

```bash
c4ed tx staking redelegate $(c4ed keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000utori --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Unbond tokens from your validator

```bash
c4ed tx staking unbond $(c4ed keys show wallet --bech val -a) 1000000utori --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Send tokens to the wallet

```bash
c4ed tx bank send wallet <TO_WALLET_ADDRESS> 1000000utori --from wallet --chain-id perun-1
```

## 🗳 Governance

#### List all proposals

```bash
c4ed query gov proposals
```

#### View proposal by id

```bash
c4ed query gov proposal 1
```

#### Vote 'Yes'

```bash
c4ed tx gov vote 1 yes --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Vote 'No'

```bash
c4ed tx gov vote 1 no --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Vote 'Abstain'

```bash
c4ed tx gov vote 1 abstain --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

#### Vote 'NoWithVeto'

```bash
c4ed tx gov vote 1 nowithveto --from wallet --chain-id perun-1 --gas-adjustment 1.4 --gas auto --gas-prices 0utori -y
```

## ⚡️ Utility

#### Update ports

```bash
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.c4e-chain/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.c4e-chain/config/app.toml
```

#### Update Indexer

**Disable indexer**

```bash
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.c4e-chain/config/config.toml
```

**Enable indexer**

```bash
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.c4e-chain/config/config.toml
```

#### Update pruning

```bash
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.c4e-chain/config/app.toml
```

## 🚨 Maintenance

#### Get validator info

```bash
c4ed status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```bash
c4ed status 2>&1 | jq .SyncInfo
```

#### Get node peer

```bash
echo $(c4ed tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.c4e-chain/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```bash
[[ $(c4ed q staking validator $(c4ed keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(c4ed status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```bash
curl -sS http://localhost:19657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0utori\"/" $HOME/.c4e-chain/config/app.toml
```

#### Enable prometheus

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.c4e-chain/config/config.toml
```

#### Reset chain data

```bash
c4ed tendermint unsafe-reset-all --home $HOME/.c4e-chain --keep-addr-book
```

#### Remove node

{% hint style="danger" %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv\_validator\_key.json**!
{% endhint %}

```bash
cd $HOME
sudo systemctl stop c4ed
sudo systemctl disable c4ed
sudo rm /etc/systemd/system/c4ed.service
sudo systemctl daemon-reload
rm -f $(which c4ed)
rm -rf $HOME/.c4e-chain
rm -rf $HOME/teritori-chain
```

## ⚙️ Service Management

#### Reload service configuration

```bash
sudo systemctl daemon-reload
```

#### Enable service

```bash
sudo systemctl enable c4ed
```

#### Disable service

```bash
sudo systemctl disable c4ed
```

#### Start service

```bash
sudo systemctl start c4ed
```

#### Stop service

```bash
sudo systemctl stop c4ed
```

#### Restart service

```bash
sudo systemctl restart c4ed
```

#### Check service status

```bash
sudo systemctl status c4ed
```

#### Check service logs

```bash
sudo journalctl -u c4ed -f --no-hostname -o cat
```
