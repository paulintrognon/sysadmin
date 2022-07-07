# Setup vpn

Open VPN Docker documentation (docker-compose): https://github.com/kylemanna/docker-openvpn/blob/master/docs/docker-compose.md

## Fix route (Etienne)

Ajoute une route vers le réseau du VPN (192.168.255.0/24), en utilisant la gateway 172.30.0.8 (l'IP du container qui porte le serveur vpn)

```
sudo ip r a 192.168.255.0/24 via 172.30.0.8
```
