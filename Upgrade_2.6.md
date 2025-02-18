# Upgrade POP Node to Devnet v2.6

- If your are starting new node follow this guide ---> [Devnet_2](https://github.com/colonyairdrops/pipe_popCDN/blob/main/Devnet_2.md)

---

### 1. Open Ports
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw reload
```

### 2. Edit systemd file
```bash
nano /etc/systemd/system/pipe.service
```
- Copy the below configuration to this file and do the necessary changes, then use "CTRL X Y Enter" to exit the file
* `--ram 4`: Replace `4` with your favorite Ram you want to allocate. (e.g., `4` for 8GB).
* `--max-disk 200`: Replace `200` with the hard disk you want to allocate. (e.g., `200` for 200GB).
* `--pubKey SOLADDRESS`: Replace `SOLADDRESS` with your Solana Public Address. (You can use your old node generated Sol address)
```
[Unit]
Description=Pipe Node Service
After=network.target
Wants=network-online.target

[Service]
AmbientCapabilities=CAP_NET_BIND_SERVICE
CapabilityBoundingSet=CAP_NET_BIND_SERVICE
User=root
Group=root
WorkingDirectory=/root/pipe
ExecStart=/root/pipe/pop \
    --ram 4 \
    --max-disk 200 \
    --cache-dir /root/pipe/download_cache \
    --pubKey SOLADDRESS \
Restart=always
RestartSec=5
LimitNOFILE=65536
LimitNPROC=4096
StandardOutput=journal
StandardError=journal
SyslogIdentifier=dcdn-node

[Install]
WantedBy=multi-user.target
EOF
```

### 3. Run the Node
```bash
sudo systemctl stop pipe && cd $HOME/pipe && wget -O pop "https://dl.pipecdn.app/v0.2.6/pop" && chmod +x pop && sudo systemctl daemon-reload && sudo systemctl restart pipe && journalctl -u pipe -f
```
