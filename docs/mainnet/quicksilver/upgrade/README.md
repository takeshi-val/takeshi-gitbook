---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/quicksilver.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: quicksilver-2 | **Latest Version Tag**: v1.2.14


## Download and build upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf quicksilver
git clone https://github.com/ingenuity-build/quicksilver.git
cd quicksilver
git checkout v1.2.14

# Build binaries
make install

# restart 
sudo systemctl restart quicksilverd && journalctl -u quicksilverd -f
```

*Thats it! Now when upgrade block height is reached, Cosmovisor will handle it automatically!*
