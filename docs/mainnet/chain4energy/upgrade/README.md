---
<figure><img src="https://github.com/takeshi-val/Logo/raw/main/chain4energy.png" width="150" alt=""><figcaption></figcaption></figure>

**Chain ID**: perun-1 | **Latest Version Tag**: v1.1.0 | **Custom Port**: 34

## Download and build upgrade binaries

```bash
# Clone project repository 

cd $HOME
git clone --depth 1 --branch  v1.1.0  https://github.com/chain4energy/c4e-chain.git
cd c4e-chain
gti checkout v1.1.0
make install



sudo systemctl restart c4ed && journalctl -u c4ed -f

```