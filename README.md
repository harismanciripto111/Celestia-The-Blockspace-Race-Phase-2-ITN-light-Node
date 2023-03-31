# Celestia-The-Blockspace-Race-Phase-2-ITN-light-Node
Phase 2

![Bundlr](https://github.com/harismanciripto111/Celestia-The-Blockspace-Race-Phase-2-ITN-light-Node/blob/main/phase%202%20staging.jpeg)


# Official Links
### [Official Document](https://docs.celestia.org/nodes/blockspace-race/#phase-2-staging)
### [Celestia Official Discord](https://discord.gg/celestiacommunity)

# Explorer
### [Explorer](https://tiascan.com/light-nodes)

## Minimum Requirements 
- I don't know, but I tried running on 4 CPUs and 8 RAM can run


# Update Package

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```

# Install Golang
You can find the version of Golang that suits you both for Linux and macOS
[Download List](https://golang.org/dl/)
```bash
here i use golang 19.1
wget https://go.dev/dl/go1.19.1.linux-amd64.tar.gz
sudo tar -xvf go1.19.1.linux-amd64.tar.gz
sudo mv go /usr/local  
export GOROOT=/usr/local/go 
export GOPATH=$HOME/Projects/Proj1 
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH 
```


# Check Go Version

```bash
go version go1.19.4 linux/amd64
```

# Set Bash Profile

```bash
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile

source $HOME/.bash_profile

```


# Install Celestia Blockspace Race

```bash
cd $HOME 
rm -rf celestia-node 
git clone https://github.com/celestiaorg/celestia-node.git 
cd celestia-node/ 
git checkout tags/v0.8.0 
make build 
make install 
make cel-key
```

# Check Celestia Version

```bash
celestia version
```

# Inittialize Light Node

```bash
celestia light init --p2p.network blockspacerace

```

# Save Mnemonic sama addressnya, terus ambil faucet di discordnya Celestia pake command $request walletklean

```bash
./cel-key list --node.type light --p2p.network blockspacerace
```

# Create Services 

```bash
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-lightd.service
[Unit]
Description=celestia-lightd Light Node
After=network-online.target

[Service]
User=$USER
ExecStart=/usr/local/bin/celestia light start --core.ip https://rpc-blockspacerace.pops.one --core.rpc.port 26657 --core.grpc.port 9090 --keyring.accname my_celes_key --metrics.tls=false --metrics --metrics.endpoint otel.celestia.tools:4318 --gateway --gateway.addr localhost --gateway.port 26659 --p2p.network blockspacerace
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

# Start Services Celestia

```bash
systemctl enable celestia-lightd
systemctl start celestia-lightd
```

# Check Logs

```bash
journalctl -u celestia-lightd.service -f
```


# Take the Node ID to submit it in the task

```bash
curl -X POST \
     -H "Authorization: Bearer $AUTH_TOKEN" \
     -H 'Content-Type: application/json' \
     -d '{"jsonrpc":"2.0","id":0,"method":"p2p.Info","params":[]}' \
     http://localhost:26658
```

# Input Metric Flags on Node

## Opened Services Celestia

```bash
nano /etc/systemd/system/celestia-lightd.service
```

## Copy this command

```bash
--metrics.tls=false --metrics --metrics.endpoint otel.celestia.tools:4318
```

Paste Command Above next to blockspacerace, see image below

![Bundlr](https://github.com/harismanciripto111/Celestia-The-Blockspace-Race-Phase-2-ITN-light-Node/blob/main/Metriks%20redflage.jpg)

## Reload and Restart Your Node

```bash
systemctl daemon-reload
systemctl restart celestia-lightd
```

## And Check Your Logs

```bash
journalctl -u celestia-lightd.service -f
```

# Cheatseet

## Restart Node

```bash
systemctl restart celestia-lightd
```

## Stop Node

```bash
systemctl stop celestia-lightd
```

## Check Balance

```bash
curl -X GET http://127.0.0.1:26658/balance

```

## Check Block

```bash
curl -X GET http://127.0.0.1:26658/header/1
```

# Delete Node
```bash
systemctl stop celestia-lightd
rm -rf /etc/systemd/system/celestia-lightd.service
rm -rf celestia-node
rm -rf .celestia-app
rm -rf .celestia-light-blockspacerace-0
rm -f $(which celestia-lightd)
rm -rf $HOME/.celestia-lightd
rm -rf $HOME/celestia-lightd
```
