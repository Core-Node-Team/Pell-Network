


![image](https://github.com/user-attachments/assets/92f91289-34af-46ec-a9dc-54e45924d356)



 * [Topluluk kanalımız](https://t.me/corenodechat)<br>
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>
 * [Discord](https://discord.com/invite/0glabs)<br>


```
cd
wget -O pellcored https://github.com/0xPellNetwork/network-config/releases/download/v1.1.5/pellcored-v1.1.5-linux-amd64
chmod +x ./pellcored
```
```
mkdir -p $HOME/.pellcored/cosmovisor/upgrades/v1.1.5/bin
sudo mv ./pellcored $HOME/.pellcored/cosmovisor/upgrades/v1.1.5/bin/pellcored
```
```
sudo ln -sfn $HOME/.pellcored/cosmovisor/upgrades/v1.1.5 $HOME/.pellcored/cosmovisor/current
sudo ln -sfn $HOME/.pellcored/cosmovisor/current/bin/pellcored /usr/local/bin/pellcored
```

```
sudo tee /etc/systemd/system/pellcored.service > /dev/null <<EOF
[Unit]
Description=pellcore node service
After=network-online.target

[Service]
User=root
ExecStart=/root/go/bin/cosmovisor run start --chain-id ignite_186-1
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=/root/.pellcored"
Environment="DAEMON_NAME=pellcored"
Environment="UNSAFE_SKIP_BACKUP=true"
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/root/.pellcored/cosmovisor/current/bin"
Environment="LD_LIBRARY_PATH=/root/.pellcored/lib:$LD_LIBRARY_PATH"

[Install]
WantedBy=multi-user.target
EOF
```
```
systemctl daemon-reload
```
