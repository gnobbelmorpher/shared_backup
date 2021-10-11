# shared backup guide / cheat sheet


## Wireguard

We might have to change the subnet and ports!
Everyone needs his own ip suffix!

Packages: ```wireguard-tools iptables-persistent```

```
/etc/wireguard/wg0.conf

[Interface]
Address = 10.10.10.1/24
ListenPort = 51821
PrivateKey = XXXXX

PostUp = iptables -A INPUT -i %i -j wireguard
PostDown = iptables -D INPUT -i %i -j wireguard

[Peer]
PublicKey = XXXXX
PresharedKey = XXXXX
AllowedIPs = 10.10.10.2/32
Endpoint = 192.168.0.7:51821
```

Activate the interface with: ```sudo systemctl enable --now wg-quick@wg0```

Show active wireguard intefaces and peers: ```sudo wg```

```
/etc/iptables/rules.v4

*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:wireguard - [0:0]
-A wireguard -p tcp -m tcp --sport 22 --dport 1024:65535 -m state --state ESTABLISHED -j ACCEPT
-A wireguard -p tcp -m tcp --sport 1024:65535 --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
-A wireguard -j DROP
COMMIT
```
Show active iptable rules: ```sudo iptables -nvL```

## ecryptfs / fstab

## cron

## ssh 