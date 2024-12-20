# Story Protocol Validator Node Setup Guide

Story raised $140M from Tier1 investors. Story is a blockchain making IP protection and licensing programmable and efficient. It automates IP management, allowing creators to easily license, remix, and monetize their work. With Story, traditional legal complexities are replaced by on-chain smart contracts and off-chain legal agreements, simplifying the entire process.

## System Requirements

| **Hardware** | **Minimum Requirement** |
|--------------|-------------------------|
| **CPU**      | 4 Cores                 |
| **RAM**      | 8 GB                    |
| **Disk**     | 200 GB                  |
| **Bandwidth**| 10 MBit/s               |


---

# Join Crypto Console Community

Join TG : [Crypto Console Telegram](https://t.me/cryptoconsol) 

Follow X : [Crypto Console Twitter](https://www.x.com/cryptoconsol) 

Subscribe : [Crypto Console Youtube](https://www.youtube.com/@cryptoconsole)


---

# VPS Options

Credit Card/Paypal/Crypto credit card : 

VPS 1 : [Contabo: Cloud VPS 1](https://www.jdoqocy.com/click-101278318-15692486) 

VPS 2 : [Contabo: Cloud VPS 2](https://www.tkqlhce.com/click-101278318-13796472)

VPS 3 : [Contabo: Cloud VPS 3](https://www.dpbolvw.net/click-101278318-13796474)

VPS 4 : [Contabo: Cloud VPS 4](https://www.anrdoezrs.net/click-101278318-13796476)

---

## Install dependencies

```
sudo apt update
sudo apt-get update
sudo apt install curl git make jq build-essential gcc unzip wget lz4 aria2 pv -y
```

## Install Go

```
cd $HOME && \
ver="1.22.0" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile && \
source ~/.bash_profile && \
go version
```

## Download Story-Geth binary

```
cd $HOME
wget https://github.com/piplabs/story-geth/releases/download/v0.10.0/geth-linux-amd64
[ ! -d "$HOME/go/bin" ] && mkdir -p $HOME/go/bin
if ! grep -q "$HOME/go/bin" $HOME/.bash_profile; then
  echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
fi
chmod +x geth-linux-amd64
mv $HOME/geth-linux-amd64 $HOME/go/bin/story-geth
source $HOME/.bash_profile
story-geth version
```

## Download Story binary

```
cd $HOME
rm -rf story-linux-amd64
wget https://github.com/piplabs/story/releases/download/v0.12.0/story-linux-amd64
[ ! -d "$HOME/go/bin" ] && mkdir -p $HOME/go/bin
if ! grep -q "$HOME/go/bin" $HOME/.bash_profile; then
  echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
fi
chmod +x story-linux-amd64
sudo cp $HOME/story-linux-amd64 $HOME/go/bin/story
source $HOME/.bash_profile
story version
```

## Initiate Iliad node

Replace "Your_moniker_name" with any name you want 
(Ex: story init --network odyssey --moniker cryptoconsole)

```
story init --network odyssey --moniker "Your_moniker_name"
```
### Peers setup
```
PEERS="a320f8a15892bddd7b5502527e0d11c5b5b9d0e3@69.67.150.107:29931,2f372238bf86835e8ad68c0db12351833c40e8ad@story-testnet-rpc.itrocket.net:26656,d4c5dcfbec11d80399bcf18d83a157259ca3efc7@138.201.200.100:26656,15c7e2b630c04ee11b2c3cfbfb1ede0379df9407@52.74.117.64:26656,359e4420e63db005d8e39c490ad1c1c329a68df3@3.222.216.118:26656,eeb7d2096a887f8ff8fdde2695c394fcf5a19273@194.238.30.192:36656,0b512c9a4421c0259813aaa05c865f82365fa7c0@3.1.137.11:26656,f4d96bf0dc67a05a48287ca2c821bc8e1d2b2023@63.35.134.129:26656,5e4f9ce2d20f2d3ef7f5c92796b1b954384cbfe1@34.234.176.168:26656,371ee318d105b0239b3997c287068ccbbcd46a91@3.248.113.42:26656,df40eee673df8eb3eddd10b359539f0e86ecaee3@207.244.230.111:36656,c82d2b5fe79e3159768a77f25eee4f22e3841f56@3.209.222.59:26656,960278d079a111b44c207dca7c2ffac640b477d1@44.223.234.211:26656,1708afbf73e2fbbb5a943aa2d97c976bf8e0d25c@52.9.183.131:26656,1cceccb08bae25a0f91fe85b0ca562fa791f47aa@184.169.154.204:26656,8876a2351818d73c73d97dcf53333e6b7a58c114@3.225.157.207:26656,0a2bc2ce69b7292deaf0b6a33af76bf9b2e25ec6@88.198.46.55:41656,6bb4ed28b08a186fc1373cfc2e96b83165c1e882@162.55.245.254:33656,a2fe3dfd6396212e8b4210708e878de99307843c@54.209.160.71:26656,aac5871efa351872789eef15c2da7a55a68abdad@88.218.226.79:26656"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
```

## Create story-geth service file
```
sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
[Unit]
Description=Story Geth Client
After=network.target

[Service]
User=root
ExecStart=/root/go/bin/story-geth --odyssey --syncmode full
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```
## Create story service file

```
sudo tee /etc/systemd/system/story.service > /dev/null <<EOF
[Unit]
Description=Story Consensus Client
After=network.target

[Service]
User=root
ExecStart=/root/go/bin/story run
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

## Reload and start story-geth
```
sudo systemctl daemon-reload && \
sudo systemctl enable story-geth && \
sudo systemctl enable story && \
sudo systemctl start story-geth && \
sudo systemctl start story && \
sudo systemctl status story-geth
```

# Check logs

### Geth logs
```
sudo journalctl -u story-geth -f -o cat
```
### Story logs
```
sudo journalctl -u story -f -o cat
```
### Check sync status

```
curl localhost:26657/status | jq
```

# SYNC using snapshot File

**Credits to Joseph Tran**

Snapshot : https://service.josephtran.xyz/testnet/story/snapshot/


# Register your Validator

### 1. Export wallet:
```
story validator export --export-evm-key
```
### 2. Private key preview
```
sudo nano ~/.story/story/config/private_key.txt
```
### 3. Import Key to Metamask 

Get the wallet address for faucet

### 4. You need at least have 1 IP on wallet

Get it from faucet : https://faucet.story.foundation/

Check the sync, the catching up must be 'false'
```
curl -s localhost:26657/status | jq
```
Stake only after "catching_up": false

### 5. Validator registering

Replace "your_private_key" with your key from the step2

```
story validator create --stake 1000000000000000000 --private-key "your_private_key"
```

### 6. Check your validator INFO
```
curl -s localhost:26657/status | jq -r '.result.validator_info' 
```

### 7. check your validator

Explorer: https://testnet.story.explorers.guru/

## BACK UP FILE

### 1. Wallet private key:
```
sudo nano ~/.story/story/config/private_key.txt
```
### 2. Validator key:

```
sudo nano ~/.story/story/config/priv_validator_key.json
```

Join our TG : https://t.me/cryptoconsol
