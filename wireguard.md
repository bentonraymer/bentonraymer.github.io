# Create & Connect to DigitalOcean Droplet

- Options:
    - Ubuntu
    - 24.10 x64
    - Basic
    - Premium Intel
    - $8 / month
    - Add Password
- Connect:
    - Open SSH program
    - Initiate SSH connection to `146.190.69.88` with password set earlier

# Setup Wireguard

Install Docker:

`apt install docker-compose`

```bash
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml
```

Paste the Following:

```yml
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
      - SERVERURL=146.190.69.88
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```

Start WireGuard:

```
cd ~/wireguard/
docker-compose up -d
```

Get WireGuard QR Code:

`docker-compose logs -f wireguard`

# Test Connections

## On Phone

- Download WireGuard App
- Add new connection via QR code
- Scan QR code
- Name Configuration
- Approve Adding VPN Profile
- Connect

## On Laptop

- Download WireGuard App
    
    https://www.wireguard.com/install/
    
- Download Configuration File:
    
    `scp root@146.190.69.88:/root/wireguard/config/peer_pc1/peer_pc1.conf ~/Documents/`
    
    (Run from Local Machine Terminal)
    
- Click `Import Tunnel from File` in WireGuard
- Select downloaded `.conf` file
- Approve Adding VPN Profile
- Click “Activate
