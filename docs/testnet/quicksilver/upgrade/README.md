---
description: Prepare for and the upcomming chain upgrade.
---

# Upgrade

<figure><img src="https://github.com/takeshi-val/Logo/raw/main/quicksilver.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: rhye-1 | **Latest Version Tag**: v1.4.4-rc.3 | **Custom Port**: 11



## Download and install upgrade binaries

```bash
# Clone project repository
cd $HOME
rm -rf quicksilver
git clone https://github.com/ingenuity-build/quicksilver.git
cd quicksilver
git checkout v1.4.4-rc.3

# Build binaries
make install

# Restart
sudo systemctl restart quicksilverd && journalctl -u quicksilverd -f

```
