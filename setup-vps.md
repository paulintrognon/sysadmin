# Aide mémoire setup vps

Ressources :
- https://docs.ovh.com/fr/vps/conseils-securisation-vps/
- https://docs.ovh.com/fr/dedicated/firewall-iptables/
- https://www.nginx.com/blog/using-free-ssltls-certificates-from-lets-encrypt-with-nginx/

## ip-tables

`sudo aptitude install iptables`

### Rules

Règles de bases :

Save in `/etc/iptables/iptables.sh`
```sh
#!/bin/sh

# Flush
iptables -F

# Drop all non authorized trafic
iptables -P INPUT DROP
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

# Allow established connections
iptables -A INPUT -m conntrack --ctstate ESTABLISHED -j ACCEPT

# Allow loopback (internal apps connecting to each other via localhost)
iptables -A INPUT -i lo -j ACCEPT

# Allow HTTP
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow SSH
iptables -A INPUT -p tcp --dport [YOUR SSH PORT] -j ACCEPT
```

### Persist after reboot

`sudo aptitude install iptables-persistent`

Save rules:
```sh
sh /etc/iptables/iptables.sh
sudo sh -c "iptables-save > /etc/iptables/rules.v4"
```

## Certbot auto renew

Add new crontab `0 12 * * * /usr/bin/certbot renew --quiet` using command `crontab -e`
