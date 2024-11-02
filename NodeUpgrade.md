## Update Node
### Stop node
```
sudo systemctl stop story
```

### Download binary

```
cd $HOME
wget https://github.com/piplabs/story/releases/download/v0.12.0/story-linux-amd64

```

### Copy binary to $HOME/go/bin

```
chmod +x story-linux-amd64
sudo cp $HOME/story-linux-amd64 $(which story)
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

