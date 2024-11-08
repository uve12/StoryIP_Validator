## Update Node

### Stop node
```
sudo systemctl stop story
```

### Download binary

```
cd $HOME
wget https://github.com/piplabs/story/releases/download/v0.12.1/story-linux-amd64

```

### Copy binary to $HOME/go/bin

```
chmod +x story-linux-amd64
sudo cp $HOME/story-linux-amd64 $HOME/go/bin/story
source $HOME/.bash_profile
story version
```

### Restart node
```
sudo systemctl daemon-reload
sudo systemctl start story
sudo systemctl status story
```

### Check Logs

```
sudo journalctl -u story -f -o cat
```

Credits to JosephTran
---

**Snapshot** : https://service.josephtran.xyz/testnet/story/snapshot/
**Dashboard** : https://service.josephtran.xyz/testnet/story/tools/story-dashboard/
