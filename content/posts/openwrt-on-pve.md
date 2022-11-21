---
title: "在 pve 上安装 openwrt"
date: 2022-11-20T22:10:52+08:00
draft: false
---
## 下载

[下载适用于您设备的 OpenWrt 固件](https://firmware-selector.openwrt.org/?version=22.03.2&target=x86%2F64&id=generic)

https://downloads.openwrt.org/releases/22.03.2/targets/x86/64/openwrt-22.03.2-x86-64-generic-ext4-combined-efi.img.gz


到 pve 主机执行，102 是 pve vm id
``` shell
wget https://downloads.openwrt.org/releases/22.03.2/targets/x86/64/openwrt-22.03.2-x86-64-generic-ext4-combined.img.gz
gunzip openwrt-22.03.2-x86-64-generic-ext4-combined.img.gz
qm importdisk 102 openwrt-22.03.2-x86-64-generic-ext4-combined.img local-lvm
```


## 配置

### 网络配置

vim /etc/config/network

```
config interface 'lan'
        option device 'br-lan'
        option proto 'static'
        option ipaddr '192.168.50.64'
        option netmask '255.255.255.0'
        option ip6assign '60'
        option gateway '192.168.50.1'
        list dns '192.168.50.1'
```
service network restart

### 扩容分区

```shell
opkg update
opkg install block-mount e2fsprogs
opkg update
opkg install fdisk blkid vim

# fdisk add partation sda3
mkfs.ext4 /dev/sda3

mkdir -p /tmp/introot
mkdir -p /tmp/extroot
mount --bind / /tmp/introot
mount /dev/sda3 /tmp/extroot
tar -C /tmp/introot -cvf - . | tar -C /tmp/extroot -xf -
umount /tmp/introot
umount /tmp/extroot

reboot
```
### 第三方仓库
src/gz openwrt_kiddin9 https://op.supes.top/packages/x86_64
删除 opkg 配置中的 option check_signature

### 安装软件

openclash adguardhome

配置接口 lan：关闭 DHCP

### 界面报错

luci-compat

### tailscale

https://github.com/adyanth/openwrt-tailscale-enabler

``` shell
wget https://github.com/adyanth/openwrt-tailscale-enabler/releases/download/v1.32.2-98e126e-autoupdate/openwrt-tailscale-enabler-v1.32.2-98e126e-autoupdate.tgz
tar x -zvC / -f openwrt-tailscale-enabler-<tag>.tgz
opkg update
opkg install libustream-openssl ca-bundle kmod-tun iptables-nft
/etc/init.d/tailscale start
tailscale up --accept-dns=false --advertise-routes=192.168.50.0/24
```

**subnet router 无效**
添加防火墙 NAT 规则，防火墙 开启转发 accept


