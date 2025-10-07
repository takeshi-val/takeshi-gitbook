---
description: >-
  Useful set of commands for node operators. From key management to chain
  governance.
---

# Useful commands

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/provenanced_logo_name.png" alt="" width="150"><figcaption></figcaption></figure>

## 🔑 Key management

#### Add new key

```bash
provenanced keys add wallet
```

#### Recover existing key

```bash
provenanced keys add wallet --recover
```

#### List all keys

```bash
provenanced keys list
```

#### Delete key

```bash
provenanced keys delete wallet
```

#### Export key to the file

```bash
provenanced keys export wallet
```

#### Import key from the file

```bash
provenanced keys import wallet wallet.backup
```

#### Query wallet balance

```bash
provenanced q bank balances $(provenanced keys show wallet -a)
```

## 👷 Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```bash
provenanced tx staking create-validator \
--amount=1000000uhash \
--pubkey=$(provenanced tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=pio-mainnet-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0.002uhash \
-y
```

#### Edit existing validator

```bash
provenanced tx staking edit-validator \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=pio-mainnet-1 \
--commission-rate=0.05 \
--from=wallet \
--gas-adjustment=1.4 \
--gas=auto \
--gas-prices=0.002uhash \
-y
```

#### Unjail validator

{% code overflow="wrap" %}
```bash
provenanced tx slashing unjail --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002uhash -y
```
{% endcode %}

#### Jail reason

```bash
provenanced query slashing signing-info $(provenanced tendermint show-validator)
```

#### List all active validators

{% code overflow="wrap" %}
```bash
provenanced q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```
{% endcode %}

#### List all inactive validators

{% code overflow="wrap" %}
```bash
provenanced q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```
{% endcode %}

#### View validator details

```bash
provenanced q staking validator $(provenanced keys show wallet --bech val -a)
```

## 💲 Token management

#### Withdraw rewards from all validators

{% code overflow="wrap" %}
```bash
provenanced tx distribution withdraw-all-rewards --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```
{% endcode %}

#### Withdraw commission and rewards from your validator

```bash
provenanced tx distribution withdraw-rewards $(provenanced keys show wallet --bech val -a) --commission --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```

#### Delegate tokens to yourself

```bash
provenanced tx staking delegate $(provenanced keys show wallet --bech val -a) 1000000ujkl --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```

#### Delegate tokens to validator

```bash
provenanced tx staking delegate <TO_VALOPER_ADDRESS> 1000000ujkl --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```

#### Redelegate tokens to another validator

```bash
provenanced tx staking redelegate $(provenanced keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ujkl --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```

#### Unbond tokens from your validator

```bash
provenanced tx staking unbond $(provenanced keys show wallet --bech val -a) 1000000ujkl --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```

#### Send tokens to the wallet

```bash
provenanced tx bank send wallet <TO_WALLET_ADDRESS> 1000000ujkl --from wallet --chain-id pio-mainnet-1
```

## 🗳 Governance

#### List all proposals

```bash
provenanced query gov proposals
```

#### View proposal by id

```bash
provenanced query gov proposal 1
```

#### Vote 'Yes'

{% code overflow="wrap" %}
```bash
provenanced tx gov vote 1 yes --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```
{% endcode %}

#### Vote 'No'

{% code overflow="wrap" %}
```bash
provenanced tx gov vote 1 no --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```
{% endcode %}

#### Vote 'Abstain'

{% code overflow="wrap" %}
```bash
provenanced tx gov vote 1 abstain --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```
{% endcode %}

#### Vote 'NoWithVeto'

{% code overflow="wrap" %}
```bash
provenanced tx gov vote 1 nowithveto --from wallet --chain-id pio-mainnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.002ujkl -y
```
{% endcode %}

## ⚡️ Utility

#### Update ports

{% code overflow="wrap" %}
```bash
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.provenance/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.provenance/config/app.toml
```
{% endcode %}

#### Update Indexer

**Disable indexer**

{% code overflow="wrap" %}
```bash
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.provenance/config/config.toml
```
{% endcode %}

**Enable indexer**

```bash
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.provenance/config/config.toml
```

#### Update pruning

{% code overflow="wrap" %}
```bash
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.provenance/config/app.toml
```
{% endcode %}

## 🚨 Maintenance

#### Get validator info

```bash
provenanced status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```bash
provenanced status 2>&1 | jq .SyncInfo
```

#### Get node peer

{% code overflow="wrap" %}
```bash
echo $(provenanced tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.provenance/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```
{% endcode %}

#### Check if validator key is correct

{% code overflow="wrap" %}
```bash
[[ $(provenanced q staking validator $(provenanced keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(provenanced status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```
{% endcode %}

#### Get live peers

{% code overflow="wrap" %}
```bash
curl -sS http://localhost:37657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```
{% endcode %}

#### Set minimum gas price

{% code overflow="wrap" %}
```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0ujkl\"/" $HOME/.provenance/config/app.toml
```
{% endcode %}

#### Enable prometheus

{% code overflow="wrap" %}
```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.provenance/config/config.toml
```
{% endcode %}

#### Reset chain data

```bash
provenanced tendermint unsafe-reset-all --home $HOME/.provenance --keep-addr-book
```

#### Remove node

{% hint style="danger" %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv\_validator\_key.json**!
{% endhint %}

```bash
cd $HOME
sudo systemctl stop provenanced
sudo systemctl disable provenanced
sudo rm /etc/systemd/system/provenanced.service
sudo systemctl daemon-reload
rm -f $(which provenanced)
rm -rf $HOME/.provenance
rm -rf $HOME/provenance-chain
```

## ⚙️ Service Management

#### Reload service configuration

```bash
sudo systemctl daemon-reload
```

#### Enable service

```bash
sudo systemctl enable provenanced
```

#### Disable service

```bash
sudo systemctl disable provenanced
```

#### Start service

```bash
sudo systemctl start provenanced
```

#### Stop service

```bash
sudo systemctl stop provenanced
```

#### Restart service

```bash
sudo systemctl restart provenanced
```

#### Check service status

```bash
sudo systemctl status provenanced
```

#### Check service logs

```bash
sudo journalctl -u provenanced -f --no-hostname -o cat
```
