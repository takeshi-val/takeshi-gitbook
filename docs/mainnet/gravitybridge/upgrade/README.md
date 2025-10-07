---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/gravitybridge.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: gravity-bridge-3 | **Latest Version Tag**: v1.13.3 |

## Download and build upgrade binaries

```bash
cd $HOME
git clone https://github.com/Gravity-Bridge/Gravity-Bridge
mv Gravity-Bridge gravity

cd $HOME/gravity/module
git pull
git reset --hard
git checkout v1.13.3
make install
```

# Upgrade gbt 

```bash
cd $HOME
mkdir -p gravity-bin
cd gravity-bin

wget -O gbt "https://github.com/Gravity-Bridge/Gravity-Bridge/releases/download/v1.13.3/gbt"
chmod +x gbt
sudo mv gbt /usr/bin/
```

# Restart node and orchestrator

```bash
sudo systemctl restart gravity-node && journalctl -u gravity-node -f -o cat
sudo systemctl restart orchestrator && journalctl -u orchestrator -f -o cat
```
