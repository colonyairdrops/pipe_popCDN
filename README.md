# Pipe Network POP CDN by @ColonyAirdrops
---
- Offical Docs - [POP_CDN](https://docs.pipe.network/getting-started/quickstart)
- Watch Video - [Youtube]()

## Node Registration Process
1. Log In to Generate Access Token
```bash
/opt/dcdn/pipe-tool login --node-registry-url="https://rpc.pipedev.network"
```
2. Generate Registration Token
```bash
/opt/dcdn/pipe-tool generate-registration-token --node-registry-url="https://rpc.pipedev.network"
```
- Save your registration token
3. Authentication Process
- Go to the website link provided in terminal
- Create an account or sign in with your Google credentials.
- Enter your unique registration token created in step 2 to confirm

## Open Ports
```bash
sudo ufw allow 8002/tcp
```
```bash
sudo ufw allow 8003/tcp
```
```bash
sudo ufw reload
```

## Pipe Network Wallet Management Guide
1. Generate and Register Wallet
```bash
/opt/dcdn/pipe-tool generate-wallet --node-registry-url="https://rpc.pipedev.network"
```
- Enter your passphrase (like a passwd)
- Save your wallet seedphrase, pub_key and any information given

  ---

# Final Setup Instructions
## Download the Node Client Binary
1. Create Directory
```bash
sudo mkdir -p /opt/dcdn
```
2. Transfer your binary files (pipe-tool and dcdnd) downloaded from email to the above directory you just created (from local machine to VPS using termius SFTP: Watch Video to understand better)
3. Make Binary Executable
```bash
sudo chmod +x /opt/dcdn/dcdnd
```
```bash
sudo chmod +x /opt/dcdn/pipe-tool
```

## Setup the dcdnd node systemd service
1. Create the Service File
```bash
sudo cat > /etc/systemd/system/dcdnd.service << 'EOF'
[Unit]
Description=DCDN Node Service
After=network.target
Wants=network-online.target

[Service]
# Path to the executable and its arguments
ExecStart=/opt/dcdn/dcdnd \
                --grpc-server-url=0.0.0.0:8002 \
                --http-server-url=0.0.0.0:8003 \
                --node-registry-url="https://rpc.pipedev.network" \
                --cache-max-capacity-mb=1024 \
                --credentials-dir=/root/.permissionless \
                --allow-origin=*

# Restart policy
Restart=always
RestartSec=5

# Resource and file descriptor limits
LimitNOFILE=65536
LimitNPROC=4096

# Logging
StandardOutput=journal
StandardError=journal
SyslogIdentifier=dcdn-node


# Working directory
WorkingDirectory=/opt/dcdn

[Install]
WantedBy=multi-user.target
EOF
```
2. load, enable and start the new service
```bash
sudo systemctl daemon-reload
```
```bash
sudo systemctl enable dcdnd
```
```bash
sudo systemctl start dcdnd
```
3. Managing the dcdnd Service
- Check Logs
```bash
sudo journalctl -f -u dcdnd.service
```
- Check Status
```bash
systemctl status dcdnd
```

---

- Done !! Feel free to ask queries in telegram channel
- Telegram - https://t.me/colonyairdrops
- Youtube - https://www.youtube.com/@ColonyAirdrops
