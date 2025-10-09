##修改网卡速率:ethtool -s enp3s0 speed 1000#######pct unlock <ID>解锁pve的id
################################################################
一      一键安装pve工具:
1,     wget -q -O /root/pve_source.tar.gz 'https://bbs.x86pi.cn/file/topic/2023-11-28/file/01ac88d7d2b840cb88c15cb5e19d4305b2.gz' && tar zxvf /root/pve_source.tar.gz && /root/./pve_source 

2,     export LC_ALL=en_US.UTF-8
        apt update && apt -y install git && git clone https://github.com/ivanhao/pvetools.git
        cd pvetools
       ./pvetools.sh
运行工具:      ./pve_source
################################################################
二      ####PVE一键安装ddns-go
wget https://ghfast.top/https://github.com/jeessy2/ddns-go/releases/download/v6.7.5/ddns-go_6.7.5_linux_x86_64.tar.gz;tar -xzvf ddns-go_6.7.5_linux_x86_64.tar.gz;./ddns-go -s install                                                                                     
################################################################
PVE硬盘直通
进入PVE的SSH，或者直接进入PVE管理网页Shell，查看你现在的存储设备的序列号:                                                  
ls /dev/disk/by-id
把硬盘序列号换成你硬盘的，100换成你的虚拟机ID）
qm set 100 -sata1 /dev/disk/by-id/ata-WDC_WDXXXX_XXXX_XXXX
如果返回以下信息，说明已成功挂载：
update VM 100: -sata1 /dev/disk/by-id/ata-WDC_WDXXXX_XXXX_XXXX
################################################################
设置pve虚拟机定时开关机命令:crontab -e
00 2 * * * pvesh create /nodes/pve/qemu/102/status/stop
00 6 * * * pvesh create /nodes/pve/qemu/102/status/start
################################################################
fake http伪装
wget https://ghfast.top/https://github.com/MikeWang000000/FakeHTTP/releases/download/0.9.18/fakehttp-linux-x86_64.tar.gz;tar -xzvf fakehttp-linux-x86_64.tar.gz;cd fakehttp-linux-x86_64;chmod +x ./fakehttp
chmod +x /fakehttp#####给权限
################################################################
wget https://ghfast.top/https://github.com/Mrlp988/ge/releases/download/a/fake86.tar;tar -xf fake86.tar;echo "/root/x86/fakehttp -b /root/x86/payload.bin -a -d -g" >> /etc/rc.local;chmod +x /etc/rc.local
先手动安装一下 luci-i18n-samba4-zh-cn 和script-utils 这两款插件
wget -qO imm.sh https://cafe.cpolar.top/wkdaily/zero3/raw/branch/main/zero3/imm.sh;chmod +x imm.sh;./imm.sh
################################################################
一键lxc:  pct create 100 /var/lib/vz/template/cache/openwrt.rootfs.tar.gz --arch amd64 --hostname openwrt --rootfs local:8 --memory 512 -swap 0 --cores 8 --ostype unmanaged --unprivileged 0 -net0 bridge=vmbr0,name=eth1
lxc配置:  nano /etc/pve/lxc/100.conf                                                                     
#lxc.include%3A /usr/share/lxc/config/openwrt.common.conf
arch: amd64
cores: 4
features: fuse=1,mount=nfs;cifs,nesting=1
hostname: OpenWrt
memory: 512
net0: name=eth1,bridge=vmbr0,hwaddr=BC:24:11:91:87:31,type=veth
onboot: 1
ostype: unmanaged
rootfs: local:100/vm-100-disk-0.raw,size=16G
swap: 0
lxc.net.1.type: phys
lxc.net.1.link: enp4s0
lxc.net.1.flags: up
lxc.net.1.name: eth0
lxc.apparmor.profile: unconfined
lxc.apparmor.allow_nesting: 1
lxc.cgroup.devices.allow: a
lxc.cgroup2.devices.allow: a
lxc.cap.drop:
lxc.mount.entry: /dev/ppp dev/ppp none bind,create=file
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
lxc.mount.entry: tmp tmp tmpfs rw,nodev,relatime,mode=1777 0 0
lxc.mount.auto: cgroup:rw
lxc.mount.auto: proc:rw
lxc.mount.auto: sys:rw
lxc.autodev: 1
