


![image](https://github.com/user-attachments/assets/92f91289-34af-46ec-a9dc-54e45924d356)



 * [Topluluk kanalımız](https://t.me/corenodechat)<br>
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>
 * [Discord](https://discord.com/invite/0glabs)<br>


```
cd
wget -O pellcored https://github.com/0xPellNetwork/network-config/releases/download/v1.1.5-ignite/pellcored-v1.1.5-linux-amd64
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
