# Install shadowsocks on OpenWRT

## Prepare

+ dnsmamq
+ download [shadowsocks-libev](https://sourceforge.net/projects/openwrt-dist/files/shadowsocks-libev/)
+ download [ChinaDNS](https://github.com/shadowsocks/ChinaDNS/releases)

## Install

First, `scp` these files to your OpenWrt Router.

```
# NOTE: filename might be different based on arch.
$ opkg install ChinaDNS_1.3.2_ar71xx.ipk
$ opkg install shadowsocks-libev_2.4.8-3_ar71xx.ipk
```

If there is an error then try add these lines to `/etc/functions`:

```bash
default_postinst() {
        return 0
}
default_prerm() {
        return 0
}
```

## Configure

### dnsmasq
```
$ echo 'conf-dir=/etc/dnsmasq.d' >> /etc/dnsmasq.conf
$ mkdir /etc/dnsmasq.d
$ echo 'server=/#/127.0.0.1#5353' > /etc/dnsmasq.d/dns.conf
```

### shadowsocks

Put shadowsocks `init.d` configuration to the right place:
```
$ cp shadowsocks /etc/init.d/shadowsocks
```

Update `your server` and `your password` in `shadowsocks.json`, then:
```
$ cp shadowsocks.json /etc/shadowsocks.json
```

Update `1.1.1.1` as your VPS server IP and `1080` to your shadowsocks local port in `shadowsocks-firewall`, then:
```
$ cp shadowsocks-firewall /usr/bin/shadowsocks-firewall
```

## Run
```
$ /etc/init.d/dnsmasq restart
$ /etc/init.d/chinadns enable
$ /etc/init.d/chinadns start
$ /etc/init.d/shadowsocks enable
$ /etc/init.d/shadowsocks start
```
