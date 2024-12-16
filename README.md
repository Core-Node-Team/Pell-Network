

![image](https://github.com/user-attachments/assets/c17a215f-a427-4d17-a580-00be0f180c6c)



 * [Topluluk kanalımız](https://t.me/corenodechat)<br>
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>
 * [Discord](https://discord.com/invite/0glabs)<br>

#### lib hatası alıyorsanız
```
echo 'export LD_LIBRARY_PATH=/root/.pellcored/lib:$LD_LIBRARY_PATH' >> ~/.bash_profile
source $HOME/.bash_profile
```
### ➡️ Explorer

https://explorer.corenodehq.com/Pell-Testnet

### 1️⃣ Installation packages and dependencies
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```
### ➡️ Go Installation
```
cd $HOME
VER="1.23.0"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```
### 2️⃣ Install node
```
echo "export PELL_PORT="57"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
```
mkdir -p $HOME/.pellcored/cosmovisor/genesis/bin/
mkdir -p $HOME/.pellcored/cosmovisor/upgrades/v1.0.20/bin
```
```
rm -rf ./pellcored-v1.0.0-linux-amd64
wget https://github.com/0xPellNetwork/network-config/releases/download/v1.0.0-ignite-186-genesis/pellcored-v1.0.0-linux-amd64
chmod +x ./pellcored-v1.0.0-linux-amd64
sudo mv ./pellcored-v1.0.0-linux-amd64 /root/.pellcored/cosmovisor/genesis/bin/pellcored
```
```
rm -rf ./pellcored-v1.0.20-linux-amd64
wget https://github.com/0xPellNetwork/network-config/releases/download/v1.0.20-ignite/pellcored-v1.0.20-linux-amd64
chmod +x ./pellcored-v1.0.20-linux-amd64
sudo mv ./pellcored-v1.0.20-linux-amd64 /root/.pellcored/cosmovisor/upgrades/v1.0.20/bin/pellcored
```
```
sudo ln -sfn $HOME/.pellcored/cosmovisor/upgrades/v1.0.20 $HOME/.pellcored/cosmovisor/current
sudo ln -sfn $HOME/.pellcored/cosmovisor/current/bin/pellcored /usr/local/bin/pellcored
```
```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.7.0
cd
```
```
mkdir -p /root/.pellcored/lib
wget "https://github.com/CosmWasm/wasmvm/releases/download/v2.1.2/libwasmvm.$(uname -m).so" -O "/root/.pellcored/lib/libwasmvm.$(uname -m).so"
```
```
echo 'export LD_LIBRARY_PATH=/root/.pellcored/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```
```
echo "/root/.pellcored/lib/" | sudo tee /etc/ld.so.conf.d/pellcored-libs.conf
sudo ldconfig
```
###  CHECK⬇️

```
sudo ldconfig -p | grep libwasmvm
```
###  ➡️ Create a service
```
sudo tee /etc/systemd/system/pellcored.service > /dev/null <<EOF
[Unit]
Description=pellcore node service
After=network-online.target

[Service]
User=root
ExecStart=/root/go/bin/cosmovisor run start
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
###  ➡️ Let's activate it
```
sudo systemctl daemon-reload
sudo systemctl enable pellcored
```
###  ➡️ Initialize the node
```
pellcored config set client chain-id ignite_186-1
pellcored config set client keyring-backend test
pellcored config set client node tcp://localhost:${PELL_PORT}657
pellcored init "Moniker-Name" --chain-id ignite_186-1
```
###  ➡️ Genesis addrbook and others
```
rm -rf /root/.pellcored/config/app.toml
wget https://raw.githubusercontent.com/0xPellNetwork/network-config/refs/heads/main/testnet/app.toml -O /root/.pellcored/config/app.toml
```
```
rm -rf /root/.pellcored/config/config.toml
wget https://raw.githubusercontent.com/0xPellNetwork/network-config/refs/heads/main/testnet/config.toml -O /root/.pellcored/config/config.toml
```
```
sed -i 's/^moniker = .*/moniker = "Moniker-Name"/' /root/.pellcored/config/config.toml
```
```
rm -rf /root/.pellcored/config/genesis.json
wget https://raw.githubusercontent.com/0xPellNetwork/network-config/refs/heads/main/testnet/genesis.json -O /root/.pellcored/config/genesis.json
```
```
curl https://raw.githubusercontent.com/MictoNode/pellnetwork/refs/heads/main/addrbook.json -o ~/.pellcored/config/addrbook.json
```
###  ➡️ Port
```
sed -i.bak -e "s%:26658%:${PELL_PORT}658%g;
s%:26657%:${PELL_PORT}657%g;
s%:6060%:${PELL_PORT}060%g;
s%:26656%:${PELL_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${PELL_PORT}656\"%;
s%:26660%:${PELL_PORT}660%g" $HOME/.pellcored/config/config.toml
```
```
sed -i.bak -e "s%:1317%:${PELL_PORT}317%g;
s%:8080%:${PELL_PORT}080%g;
s%:9090%:${PELL_PORT}090%g;
s%:9091%:${PELL_PORT}091%g;
s%:8545%:${PELL_PORT}545%g;
s%:8546%:${PELL_PORT}546%g;
s%:6065%:${PELL_PORT}065%g" $HOME/.pellcored/config/app.toml
```
###  ➡️ Peers and Seeds
```
SEEDS="d52c32a6a8510bdf0d33909008041b96d95c8408@34.87.39.12:57656,9b955d07f05b02b3d622f9cb7a0e6cfecd719985@34.87.47.193:57656"
PEERS="6f204d45ddb4749fb1d265ab359ff2a6ff29ad29@10.84.1.148:26656,9b955d07f05b02b3d622f9cb7a0e6cfecd719985@34.87.47.193:26656,1d3fe72a84c15b4217722304ab162d44d4720bea@10.84.0.70:26656,2e386817d414c4b91f9b2f19cf19ce205d343c81@10.148.0.47:26656,c2bb903f085b74dd0742a51d5d736f0502b844bc@10.148.0.52:26656,28c0fcd184c31ac7f3e2b3a91ae60dedc086b0c3@94.130.204.227:26656,48c48532950e51fba80a1031d5a58c627737ed84@65.109.82.111:25656,3d231585b8f737d507208e2f2122f24e53aefdb3@10.148.15.199:26656,31853c271baa99f36eab7cdb1071d4fb5cba4ffc@10.148.15.202:26656,0b871f15d2f3047f87fdeaed39b30f9ab1f0599c@10.148.0.58:56656,420793a7ceecb8a656b881af566f460446397942@217.86.243.84:26656,c8739dfe6da741429c8ecdac27aab3afc1ee1ed2@16.62.152.77:26656,969e6b8df14860c3f5176cba6af317e3666f721d@82.223.17.254:26656,677ec699d545d8d29aed0250f45c5690799c6ee8@37.60.237.51:26656,6764a6dd299715d814060514b125fc140cfb54c8@124.156.216.149:26656,0d2937c86387342c4c13cbef19b8766335750680@185.252.232.87:26656,67a2c7996352dac1d12e1ad0176ca36d06f53555@171.247.105.229:26656,f9decf3cb78d88581d6c3c14828cb906a75fd060@95.216.97.238:26656,103b755578215a240d2e70a4294643e35ecfd6fa@152.53.66.0:21656,be843b3784f43fd3425e9111a6884d05002bc705@10.148.15.197:26656,c9a5d341547e06441e30e07db289fc337ec36f79@152.53.87.97:26656,d1dd899a5174ee33c30a4cdf78a57d0e8a24e417@185.148.241.23:26656,3064968f093bbae72e3a44a9364f4e5d1b21c2c1@10.148.0.48:26656,1bcc2348282d98636bb73eff3fc39f5f06d4a3c2@65.109.53.24:58656,f7f1f3a9e1fdd42c3e597b3ff556f39c8edc5834@152.53.102.226:26656,407c7229a207a0c932a56ab3a979fc2312751390@162.19.240.7:28656,1c6bdf6076953167fbffb8e34454ffa14e2fc6df@151.115.77.175:26656,6acaeb73550facc3f8cc6f67134f7f5afed7f714@184.107.169.193:26656,ef5a3ea90b49ed74bf7a8a51be190a86cd306ef4@5.75.243.19:26656,12f81307f95329e905100e50e84a0ac051e97389@67.217.58.230:26656,7cc16ef8eea8fd6087d172192b784d6a8d36aa0b@207.180.204.107:26656,5dd008e07046b7aa4d83d6261c895a53d20bed6b@15.235.225.32:26656,5c714ab8ac83822d9d43d8fcc73396e3b0ecf0b8@37.27.132.125:26656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.pellcored/config/config.toml
```
### ➡️ Pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.pellcored/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.pellcored/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.pellcored/config/app.toml
```
### ➡️ Indexer
```
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.pellcored/config/config.toml
```
### ➡️ Gas Settings
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0apell"|g' $HOME/.pellcored/config/app.toml
```
### ➡️ Prometheus
```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.pellcored/config/config.toml
```
### ➡️ Starter Snap (thx josephtran)
```
pellcored tendermint unsafe-reset-all --home $HOME/.pellcored
if curl -s --head curl http://37.120.189.81/pell_testnet/pell_snap.lz4 | head -n 1 | grep "200" > /dev/null; then
  curl http://37.120.189.81/pell_testnet/pell_snap.lz4 | lz4 -dc - | tar -xf - -C $HOME/.pellcored
    else
  echo no have snap
fi
```
### ➡️ Let's get started
```
sudo systemctl restart pellcored
journalctl -fu pellcored -o cat
```
### ➡️ Log Command
```
sudo journalctl -fu pellcored -o cat
```
### ➡️ Create wallet
```
pellcored keys add wallet-name --keyring-backend=test
```
➡️ don't forget to backup the wallet words!

### ➡️ Import wallet
```
pellcored keys add wallet-name --recover --keyring-backend=test
```
### ➡️ Create Validator
Reminder: You can't create a validator without Sync. You must have to catch the latest block.

Save your pubkey

```
rm -rf /root/validator.json
```
```
cat > ./validator.json << EOF
{
	"pubkey": $(pellcored tendermint show-validator),
	"amount": "1000000000000000000apell",
	"moniker": "<your_node_name>",
	"identity": "optional identity signature (ex. UPort or Keybase)",
	"website": "validator's (optional) website",
	"security": "validator's (optional) security contact email",
	"details": "validator's (optional) details",
	"commission-rate": "0.1",
	"commission-max-rate": "0.2",
	"commission-max-change-rate": "0.01",
	"min-self-delegation": "1"
}
EOF
```
```
pellcored tx staking create-validator ./validator.json --chain-id=ignite_186-1 --fees=0.000001pell --gas=1000000 --from=wallet-name --keyring-backend=test -y
```
### ➡️ Delegate to Yourself
```
pellcored tx staking delegate $(pellcored keys show wallet-name --bech val -a) 1000000000000000000apell --from wallet-name --chain-id ignite_186-1 --fees=0.000001pell --gas=1000000 --keyring-backend=test -y 
```
### ➡️ Edit Validator
```
pellcored tx staking edit-validator \
--chain-id ignite_186-1 \
--commission-rate 0.05 \
--new-moniker "validator-name" \
--identity "" \
--details "" \
--website "" \
--security-contact "" \
--from "wallet-name" \
--node http://localhost:${PELL_PORT}657 \
--fees=0.000001pell \
--gas=1000000 \
--keyring-backend=test
-y
```
### ➡️ Complete deletion
```
cd $HOME
sudo systemctl stop pellcored
sudo systemctl disable pellcored
sudo rm -rf /etc/systemd/system/pellcored.service
sudo systemctl daemon-reload
sudo rm -f /usr/local/bin/pellcored
sudo rm -f $(which pellcored)
sudo rm -rf $HOME/.pellcored
sed -i "/PELL_PORT_/d" $HOME/.bash_profile
```
### ➡️ Block check
```
local_height=$(curl -s localhost:${PELL_PORT}657/status | jq -r .result.sync_info.latest_block_height); network_height=$(curl -s https://pell-testnet-rpc.mictonode.com/status | jq -r .result.sync_info.latest_block_height); blocks_left=$((network_height - local_height)); echo "Your node height: $local_height"; echo "Network height: $network_height"; echo "Blocks left: $blocks_left"
```
