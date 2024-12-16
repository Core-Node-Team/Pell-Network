```
sudo apt install liblz4-tool
systemctl stop pellcored
cp $HOME/.pellcored/data/priv_validator_state.json $HOME/.pellcored/priv_validator_state.json.backup
cp $HOME/.pellcored/config/priv_validator_key.json $HOME/.pellcored/priv_validator_key.json.backup
pellcored tendermint unsafe-reset-all --home $HOME/.pellcored --keep-addr-book
curl -L http://37.120.189.81/pell_testnet/pell_snapshot.lz4 | tar -I lz4 -xf - -C $HOME/.pellcored
mv $HOME/.pellcored/priv_validator_state.json.backup $HOME/.pellcored/data/priv_validator_state.json
sudo systemctl daemon-reload
sudo systemctl restart pellcored
sudo journalctl -u pellcored.service -f --no-hostname -o cat
```
