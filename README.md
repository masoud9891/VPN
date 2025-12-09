# VPN

# Edge Server (Hetzner) – Secure Setup & Xray (VLESS + REALITY)

## 1. Base System Hardening

- OS: Ubuntu 22.04 LTS (fresh reinstall)
- Create non-root user:

  ```bash
  adduser masoud
  usermod -aG sudo masoud


Enable UFW and open SSH + service ports:

sudo ufw allow 2003/tcp
sudo ufw allow 443/tcp
sudo ufw enable


Harden SSH (/etc/ssh/sshd_config):

Port 2003
PermitRootLogin no
PasswordAuthentication no

sudo systemctl restart ssh


Block outbound SSH scan ports (Hetzner abuse protection):

sudo ufw deny out to any port 22
sudo ufw deny out to any port 2222
sudo ufw deny out to any port 10022
sudo ufw deny out to any port 2022
sudo ufw deny out to any port 2212
sudo ufw reload

2. Install Xray (Official Script)
sudo apt update && sudo apt install -y curl unzip
bash <(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)
xray -version

3. Generate REALITY keys
xray x25519
# note: store PrivateKey securely, PublicKey will be used on IR server / clients

4. Generate UUID for tunnel user
cat /proc/sys/kernel/random/uuid

5. Xray Configuration

Path: /usr/local/etc/xray/config.json



Apply config:

sudo systemctl restart xray
sudo systemctl status xray
sudo ss -tulpn | grep xray

systemctl stop xray
cp /usr/local/etc/xray/config-YYYY-MM-DD-HHMM.json /usr/local/etc/xray/config.json
systemctl restart xray

