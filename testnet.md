# Pipe Network Testnet

This guide provides detailed instructions on how to set up and configure the POP Cache Node on Linux systems using the provided binaries.
* You must be whitelisted and receive email with instructions to qualify for node's rewards.
* If you are a new user, Signup [here](https://airtable.com/apph9N7T0WlrPqnyc/pagSLmmUFNFbnKVZh/form) and wait until you receive an email with invite code.

## System Requirements
* Linux (Ubuntu 24.04 Only, lower version of ubunut is not supported)
* 16GB RAM (configurable), more the better for higher rewards
* SSD storage with 100GB+ available space
* Internet connectivity available 24/7

#

## Stop Old Node
if you have running a devnet node previously, you need to do these steps first.

### 1. Backup `Node Info File`
You will need to backup old `node_info.json` file which you can find in pipe folder in your `root` directory.

### 2. Stop old node
```bash
sudo systemctl stop pipe
sudo systemctl disable pipe
sudo rm /etc/systemd/system/pipe
sudo systemctl daemon-reload
```

### 3. Open Ports
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw reload
```

## Node Setup
### 1. Install Dependencies
```
sudo apt update
```
```
sudo apt install -y libssl-dev ca-certificates
```

### 2. Create System Configuration
- Copy the below command whole and run in your terminal
```
sudo bash -c 'cat > /etc/sysctl.d/99-popcache.conf << EOL
net.ipv4.ip_local_port_range = 1024 65535
net.core.somaxconn = 65535
net.ipv4.tcp_low_latency = 1
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.core.wmem_max = 16777216
net.core.rmem_max = 16777216
EOL'
sudo sysctl -p /etc/sysctl.d/99-popcache.conf
```
```
sudo bash -c 'cat > /etc/security/limits.d/popcache.conf << EOL
*    hard nofile 65535
*    soft nofile 65535
EOL'
```
Now close your VPS and log in again

### 3. Create directories
```
sudo mkdir -p /opt/popcache
sudo mkdir -p /opt/popcache/logs
```
```
cd /opt/popcache
```
### 4. Download Pipe binaries
1. Visit: [Download](https://download.pipe.network/) file (use invite code from email)
2. Once Downloaded use termius SFTP feature to drag and drop the downloaded file in `/opt/popcache` you created in step 3

```
sudo tar -xzf pop-v0.3.0-linux-*.tar.gz
```
```
chmod +x /opt/popcache/pop
```

3. Setup Config File
```
nano config.json
```
```
{
  "pop_name": "your-pop-name",
  "pop_location": "Your Location, Country",
  "server": {
    "host": "0.0.0.0",
    "port": 443,
    "http_port": 80,
    "workers": 0
  },
  "cache_config": {
    "memory_cache_size_mb": 4096,
    "disk_cache_path": "./cache",
    "disk_cache_size_gb": 100,
    "default_ttl_seconds": 86400,
    "respect_origin_headers": true,
    "max_cacheable_size_mb": 1024
  },
  "api_endpoints": {
    "base_url": "https://dataplane.pipenetwork.com"
  },
  "identity_config": {
    "node_name": "your-node-name",
    "name": "Your Name",
    "email": "your.email@example.com",
    "website": "https://your-website.com",
    "discord": "your_discord_username",
    "telegram": "your_telegram_handle",
    "solana_pubkey": "YOUR_SOLANA_WALLET_ADDRESS_FOR_REWARDS"
  }
}
```
Setup Configuration according to you
- `pop-location`: location of VPS --> Command to Check --> `realpath --relative-to /usr/share/zoneinfo /etc/localtime`
- `website`: Anything you prefer (can use github profile)

4. Create systemd file
```
sudo bash -c 'cat > /etc/systemd/system/popcache.service << EOL
[Unit]
Description=POP Cache Node
After=network.target

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/opt/popcache
ExecStart=/opt/popcache/pop
Restart=always
RestartSec=5
LimitNOFILE=65535
StandardOutput=append:/opt/popcache/logs/stdout.log
StandardError=append:/opt/popcache/logs/stderr.log
Environment=POP_CONFIG_PATH=/opt/popcache/config.json

[Install]
WantedBy=multi-user.target
EOL'
```
```
sudo systemctl daemon-reload
```
```
sudo systemctl enable popcache
```
```
sudo systemctl start popcache
```

### 5. Check health
**1. Check status**
```
sudo systemctl status popcache
```
**2. Check logs**
```
sudo journalctl -u popcache
```

### Optional: Restart or Stop Node
```console
# Stop
sudo systemctl stop popcache

# Restart
sudo systemctl daemon-reload
sudo systemctl enable popcache
sudo systemctl restart popcache
```

---
- Done !! Feel free to ask queries in telegram channel
- Telegram - https://t.me/colonyairdrops
- Youtube - https://www.youtube.com/@ColonyAirdrops
- Twitter - https://x.com/colony_airdrops
