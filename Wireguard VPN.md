# Wireguard VPN Guide
Yes, you can run your own VPN Server ;)
Main reference: https://github.com/angristan/wireguard-install

## Why VPN?
~~Don't tell anyone but I really want to see cencored content~~ nah I'm kidding. That's probably what people thinks what using VPNs is for. But I use it to connect multiple offices, home computers, servers, and multiple sites together. Work remotely. I don't want to visit a factory that is on another city just to run a few lines of commands for debugging or configuring. 

## Why Wireguard?
You mean why not OpenVPN? I believe there at some cases OpenVPN does better. You'll want to see [this link](https://www.top10vpn.com/guides/wireguard-vs-openvpn/) if this is your question. Also, OpenVPN is harder to configure, and is relatively slower compared to Wireguard.

## Wireguard Server
See the main reference link. It really eases up installation and configuration, very handy to install Wireguard installations, adding and revoking users.

## Problems
### I cannot reach local networks when Wireguard is up!
Without extra configurations, if you just get the Wireguard configuration file, and activate the Wireguard interface, then yes, you likely won't be able to reach local networks. Why? Every traffic from on your client computer or device is routed through the Wireguard interface to the Wireguard server, and the server is not able to reach your local network.

To allow local networks, **edit your configuration file** (something that ends in .conf extension, usually named with the Wireguard interface name like wg0, on Linux is usually located in `/etc/wireguard/` folder , and check `AllowedIPs`in `[Peer]` section of the configuration.
```
AllowedIPs = 0.0.0.0/0,::/0
```

That is likely what your configuration looks like. That means, any traffic on any network interface on your computer is routed to Wireguard. Set only which IPs you want to be routed through Wireguard. By default, the private network of Wireguard is on `10.66.66.0`, and to reach `10.66.66.x`, I want it to be routed through the Wireguard tunnel.
```
AllowedIPs = 10.66.66.0/24,::/0
```

That will solve it.

### The Wireguard peer is unreachable after uncertain ammount of time!
Yes, and I was troubled with that too. Changing handshaking duration does not help, so I won't even try anymore. After some readings, I suspect it is because the Wireguard peer is behind a router. If the peer has no activity routed to reach Wireguard network, seems like the port on the router is then changed or disposed, makes the peer unreachable unless it reaches first. I came up with a workaround to make it keep reaching to the Wireguard server, with PING, in a systemd service. Check [this](https://github.com/seikosantana/wg-keepalive-ping) out.
