# Wireguard
## Wireguard Example Configs
### Server Config
```
[Interface]
Address = [Wireguard Server IP]
SaveConfig = true

Postup = iptables -t nat -A PREROUTING -i [external network interface] -p tcp -j DNAT --to-destination [Wireguard Client IP]
Postup = iptables -A FORWARD -i %i -j ACCEPT
Postup = iptables -t nat -A POSTROUTING -j MASQUERADE

PostDown = iptables -t nat -D PREROUTING -i [external network interface] -p tcp -j DNAT --to-destination [Wireguard Client IP]
PostDown = iptables -D FORWARD -i %i -j ACCEPT
PostDown = iptables -t nat -D POSTROUTING -j MASQUERADE

ListenPort = 51820
PrivateKey = [Server's Private Key]

[Peer]
PublicKey = [Client's Public Key]
AllowedIPs = [Wireguard Client IP]
```

### Client Config
```
[Interface]  
Address = [Wireguard Client IP]  
SaveConfig = true  
ListenPort = 51820  
FwMark = 0xca6c  
PrivateKey = [Client's Private Key]  
  
[Peer]  
PublicKey = [Server's Public Key]
AllowedIPs = 0.0.0.0/0  
Endpoint = [Wireguard Server Public IP]:51820  
PersistentKeepalive = 30
```

## Example Configs Explained
### Server Config
`Address = [Wireguard Server IP]` Sets the IP address for the server within the tunnel, e.g. `10.10.0.1/32`. The server config does not need its own public IP address.

This section configures network traffic forwarding from outside traffic into the tunnel and points it at the client specified in `[Wireguard Client IP]`

`Postup = iptables -t nat -A PREROUTING -i [external network interface] -p tcp -j DNAT --to-destination [Wireguard Client IP]`

`Postup` specifies when the commands are run, immediately after the Wireguard interface is started

`-t nat` "Select table Network Address Translation" Applies changes to the NAT table rather than the default table (filter).

`-A PREROUTING` "Append to the prerouting chain rule" The prerouting chain rule is for modifying packets before routing decisions are made

`-i [external network interface]` "For traffic from the external network interface" This specifies to only perform modifications for traffic from the specified interface. Replace the bracketed phrase with your server's network interface, e.g. `-i ens6` or `-i eth0`. 

`-p tcp` "Using the tcp protocol" Only applies transforms to traffic using the tcp network protocol.

`-j DNAT` "Set target to Destination Network Address Translation" This specifies what part of the packet is to be changed, the destination IP address.

`--to-destination [Wireguard Client IP]` "Change destination to Client IP" Specifies what the new destination IP for the packet should be.

`Postup = iptables -A FORWARD -i %i -j ACCEPT`

`-A FORWARD` "Append to the forwarding chain rule" The forwarding chain rule filters for traffic being sent through the server

`-i %i` "Send traffic to the Wireguard tunnel network interface" 

`-j ACCEPT` "Let the packet through to the network" 

`Postup = iptables -t nat -A POSTROUTING -j MASQUERADE`

`-A POSTROUTING` The postrouting chain rule is for modifying packets after routing.

`-j MASQUERADE` Masquerade modifies the packet's source to be from the server's IP address in the current context, in this instance it would set it to `[Wireguard Server IP]` as the traffic is routed through the Wireguard tunnel. 

```
PostDown = iptables -t nat -D PREROUTING -i [external network interface] -p tcp -j DNAT --to-destination [Wireguard Client IP]
PostDown = iptables -D FORWARD -i %i -j ACCEPT
PostDown = iptables -t nat -D POSTROUTING -j MASQUERADE
```
These three lines do the opposite of the above, using `PostDown` to specify that they should be run immediately after the Wireguard interface is closed. `-D` in this is used to specify deletion of the chain rules.

`ListenPort = 51820` 51820 is the default port for Wireguard connections but can be changed to any port that you would like. It's best practice to avoid using reserved ports however.

`AllowedIPs = [Wireguard Client IP]` this specifies the in tunnel IP address that the Wireguard interface will allow to connect using that public key.

### Client Config
`Address = [Wireguard Client IP]` Sets the IP address for the client within the tunnel, e.g. `10.10.0.2/32`. This is not the client's public IP address.

`Endpoint = [Wireguard Server Public IP]:51820` The public IP address or url for the server, e.g. `69.12.210.42:42069` or `google.com:51280` 

`PersistentKeepalive = 30` specifies the time in seconds between keepalive packets in order to maintain the connection.