wget -O pellcored https://github.com/0xPellNetwork/network-config/releases/download/v1.3.0/pellcored-1.3.0-linux-amd64

mkdir -p $HOME/.pellcored/cosmovisor/upgrades/v1.3.0/bin

sudo mv ./pellcored $HOME/.pellcored/cosmovisor/upgrades/v1.3.0/bin/pellcored


sudo ln -sfn $HOME/.pellcored/cosmovisor/upgrades/v1.3.0 $HOME/.pellcored/cosmovisor/current

sudo ln -sfn $HOME/.pellcored/cosmovisor/current/bin/pellcored /usr/local/bin/pellcored

sudo systemctl restart pellcored

 sudo journalctl -fu pellcored -o cat
