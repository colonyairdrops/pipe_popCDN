# Upgrade POP Node to Devnet v2.7

- If your are starting new node follow this guide ---> [Devnet_2](https://github.com/colonyairdrops/pipe_popCDN/blob/main/Devnet_2.md)
- For Users who are already running version lower than 2.6 and missed the last upgrade (2.6) first make sure to upgrade to 2.6 then to 2.7 to avoid any confusion alternatively if you know how to do it you can directly jump to 2.7
---

## Upgrade
```bash
sudo systemctl stop pipe && cd $HOME/pipe && wget -O pop "https://dl.pipecdn.app/v0.2.7/pop" && chmod +x pop && sudo systemctl daemon-reload && sudo systemctl restart pipe
```
```bash
./pop --enable-80-443
```

## Check health
**1. Check status**
```
sudo systemctl status pipe
```
**2. Check logs**
```
journalctl -u pipe -f
```
