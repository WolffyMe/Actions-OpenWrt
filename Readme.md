# ImmortalWrt MT798X 路由器固件在线自动编译

### MTK万能串口救砖：[MediaTek Filogic 系列路由器串口救砖教程](https://www.cnblogs.com/p123/p/18046679)
### 我的刷机教程：[Tutorial](https://github.com/lgs2007m/Actions-OpenWrt/tree/main/Tutorial) 可直接下载包含教程的刷机文件：[Router-Flashing-Files](https://github.com/lgs2007m/Actions-OpenWrt/releases/tag/Router-Flashing-Files)

---
<details>
<summary><h2>24.10固件编译说明</h2></summary>
  
#### 237大佬24.10 mt798x闭源仓库- [padavanonly/immortalwrt-mt798x-6.6](https://github.com/padavanonly/immortalwrt-mt798x-6.6).

#### 237大佬24.10 mt7988分支闭源仓库- [padavanonly/immortalwrt-mt798x-6.6;mt798x-mt799x-6.6-mtwifi](https://github.com/padavanonly/immortalwrt-mt798x-6.6/tree/mt798x-mt799x-6.6-mtwifi).

- Set LAN IP Address
- Manually Set Default Theme
- Use xdr6078 eeprom(only for CLX-S20P)

- #### 说明
openwrt-24.10-6.6分支中5G Wi-Fi的MAC地址无法设置，会自动使用2.4G的MAC进行置位得到5G MAC，2.4G的MAC可以使用向b0dat配置文件添加MacAddress的方法设置，未找到5G MAC被修改的地方，暂时无法修改。  
mt798x-mt799x-6.6-mtwifi分支编译出来的7dr7299固件，使用发现两个问题：1.无线桥接无法使用；2.设置无线参数保存并应用，有线网口会重启。  

- #### 1. Set LAN IP Address
设置LAN IP地址（路由器登录地址），默认192.168.6.1。  

- #### 2. Manually Set Default Theme
设置[Fluent主题](https://github.com/LazuliKao/luci-theme-fluent)为默认主题，当编译配置config有多个主题也可以设置其它主题为默认主题，比如Argon、Design。  

- #### 3. Use xdr6078 eeprom(only for CLX-S20P)
默认使用TP的xdr6078的高功率eeprom，2.4G可提升至25dBm、5G提升至24dBm。  
对于使用mt7975无线芯片内置功放的AX6000机型，可以替换MT7986_iPAiLNA_EEPROM_AX6000.bin来取代机子原来的eeprom，进而提高无线功率，外置功放不能使用该方法。  
开启该选项会将名为MT7986_iPAiLNA_EEPROM_AX6000.bin的TP xdr6078的eeprom拷贝固件的firmware文件夹中，驱动检测到该文件并查询文件中的MAC是有效的，就不会从factory读取原机的eeprom了。  
如果是SPI NAND机型，还需要删除对应驱动下的这个补丁文件package/mtk/drivers/mt_wifi/patches-76xx/001-eeprom-flash-api.patch，237大佬的仓库已做好这些步骤，内置功放想要用高功率eeprom，只需要将编译配置文件config中的CONFIG_MTK_MT7986_NEW_FW=y注释掉即可。  

</details>

---
<details>
<summary><h2>21.02固件编译说明</h2></summary>

#### hanwckf大佬21.02 mt798x闭源仓库- [hanwckf/immortalwrt-mt798x](https://github.com/hanwckf/immortalwrt-mt798x).

#### 237大佬21.02 mt798x闭源仓库- [padavanonly/immortalwrt-mt798x](https://github.com/padavanonly/immortalwrt-mt798x).

#### hanwckf大佬mt798x uboot仓库- [hanwckf/bl-mt798x](https://github.com/hanwckf/bl-mt798x).

## MT7986 hanwckf workflow 手动运行可选项：
<details>
<summary>展开详细</summary>

- Select Source Branch
- Set LAN IP Address
- Choose WiFi Driver
- Choose Switch Driver
- [ ] Use luci-app-mtk wifi config
- [ ] Not build luci-app-dockerman

- #### 说明
百里源码中的WAN、LAN地址顺序已修复并固定了WiFi MAC地址，交换机驱动已改为使用GSW。  
储留箱S20P可同时编译DSA和GSW两种交换机驱动的固件，不用选择。  
YVR X6使用GSW，不可选择。  

- #### 1. Select Source Branch
默认使用 [lgs2007m/immortalwrt-mt798x](https://github.com/lgs2007m/immortalwrt-mt798x) 的 [v7672](https://github.com/lgs2007m/immortalwrt-mt798x/tree/v7672) 分支，该分支支持mt_wifi v7.6.7.2和v7.6.6.1，conninfra datconf warp使用v7.6.7.2版本配套的； [v7661](https://github.com/lgs2007m/immortalwrt-mt798x/tree/v7661) 分支也支持mt_wifi v7.6.7.2和v7.6.6.1，conninfra datconf warp改为v7.6.6.1版本配套的旧版。v7.6.7.3版本驱动听说有问题，未测试，观望中。  

- #### 2. Set LAN IP Address
设置LAN IP地址（路由器登录地址），默认192.168.1.1。  

- #### 3. Choose WiFi Driver
默认使用WiFi驱动版本v7.6.7.2-fw-20240823(recommend)。  
mt_wifi的firmware可选，warp默认使用v7.6.7.2配套的warp_20231229-5f71ec，firmware用驱动自带的，不可选。  
驱动版本v7.6.7.2-fw-default不建议使用，我使用5G无线，电脑打CS2同时手机刷视频，CS2会延迟增高卡顿。  
根据网络和ChatGPT查询，我理解：  
mt_wifi是MediaTek的WiFi驱动程序，主要用于控制无线网络功能，提供WiFi协议栈支持、无线电控制、连接管理等。  
warp是MediaTek的WiFi Warp Accelerator加速框架，通常用于WiFi网络数据处理的加速。  
mt_wifi和warp中的固件（firmware）是驱动程序与无线芯片之间的中间层。它通常被加载到无线芯片中，以控制其硬件功能，管理无线协议并处理数据传输，会影响无线性能和稳定性。  
v7.6.7.2-fw-20240823(recommend) 推荐，使用[mtk-openwrt-feeds(20240823)](https://git01.mediatek.com/plugins/gitiles/openwrt/feeds/mtk-openwrt-feeds/+/9de357f7abdbc03fd2daf9d9a9aa5ccce6f4fc30)  
v7.6.6.1-fw-20230808(recommend) 推荐，使用提取自TP-XDR6088固件的fw-20230808  
v7.6.7.2-fw-default 使用驱动包自带firmware fw-20231229  
v7.6.6.1-fw-default 使用驱动包自带firmware fw-20220906  
其他firmware可自行组合尝试：  
fw-20221208 使用mt7986-7.6.7.0-20221209-b9c02f-obj驱动包的fw-20221208  
fw-20230306 使用YVR X6的fw-20230306  
fw-20230421 使用mtk-openwrt-feeds(20230421)的fw-20230421  
fw-20231024 使用mtk-openwrt-feeds(20231024)的fw-20231024  
```
# SSH查看内核版本
uname -a
# 查看WiFi驱动版本
iwpriv ra0 get_driverinfo
# 查看WiFi驱动mt_wifi mt7986 firmware版本
strings /lib/firmware/7986_WACPU_RAM_CODE_release.bin | grep -E '202[0-9]{6}'
strings /lib/firmware/mt7986_patch_e1_hdr.bin | grep -E '202[0-9]{6}'
strings /lib/firmware/mt7986_patch_e1_hdr_mt7975.bin | grep -E '202[0-9]{6}'
strings /lib/firmware/WIFI_RAM_CODE_MT7986.bin | grep -E '202[0-9]{6}'
strings /lib/firmware/WIFI_RAM_CODE_MT7986_MT7975.bin | grep -E '202[0-9]{6}'
# 查看WiFi驱动warp mt7986 firmware版本
strings /lib/firmware/7986_WOCPU0_RAM_CODE_release.bin | grep -E '202[0-9]{6}'
strings /lib/firmware/7986_WOCPU1_RAM_CODE_release.bin | grep -E '202[0-9]{6}'
# 或者
cat /proc/warp_ctrl/warp0/wo
```

- #### 4. Choose Switch Driver
该选项为百里专用，默认使用GSW交换机驱动，可选DSA交换机驱动。  
GSW：Gigabit Switch swconfig 模式，有交换机配置插件，不过京东云百里AX6000的WAN是单独接CPU的2.5G PHY RTL8221B，不接在MT7531交换机上，所以WAN不支持在交换机配置插件中设置VLAN。  
DSA：Distributed Switch Architecture 分布式交换架构模式，DSA没有单独的交换机配置插件，但在“网口”-“接口”-“设备”选项卡中的br-lan设备中的网桥VLAN过滤中可以查看网口状态设置VLAN。  
百里原厂固件使用的是DSA，hanwckf大佬源码中百里的交换机驱动先前是DSA，听说在WAN、LAN互换时硬件加速可能失效，但是我测试了是正常的。  
目前hanwckf大佬源码中百里已改为使用GSW，使用GSW在WAN、LAN互换时硬件加速正常，所以DSA、GSW随便用吧。  
两者具体区别可以参考OpenWrt社区资料：[converting-to-dsa](https://openwrt.org/docs/guide-user/network/dsa/converting-to-dsa) [dsa-mini-tutorial](https://openwrt.org/docs/guide-user/network/dsa/dsa-mini-tutorial)  

- #### 5. Use luci-app-mtk wifi config
该选项默认关闭，即按.mtwifi-cfg.config配置文件，使用mtwifi-cfg配置工具，需要使用旧的luci-app-mtk无线配置工具请打钩。  
mtwifi-cfg：为mtwifi设计的无线配置工具，兼容openwrt原生luci和netifd，可调整无线驱动的参数较少，配置界面美观友好。  
luci-app-mtk：源自mtk-sdk提供的配置工具，需要配合wifi-profile脚本使用，可调整无线驱动的几乎所有参数，配置界面较为简陋。  
区别详见大佬的博客[mtwifi无线配置工具说明](https://cmi.hanwckf.top/p/immortalwrt-mt798x/#mtwifi%E6%97%A0%E7%BA%BF%E9%85%8D%E7%BD%AE%E5%B7%A5%E5%85%B7%E8%AF%B4%E6%98%8E)  
.mtwifi-cfg.config配置文件中已设置使用mtwifi-cfg配置工具：  
CONFIG_PACKAGE_luci-app-mtwifi-cfg=y  
CONFIG_PACKAGE_luci-i18n-mtwifi-cfg-zh-cn=y  
CONFIG_PACKAGE_mtwifi-cfg=y  
CONFIG_PACKAGE_lua-cjson=y  

- #### 6. Not build luci-app-dockerman
该选项默认关闭，即按.mtwifi-cfg.config配置文件编译dockerman，不需要编译dockerman请打钩。  
.mtwifi-cfg.config配置文件中已设置编译dockerman：  
CONFIG_PACKAGE_luci-app-dockerman=y  

- #### 百里改无线发射功率
MT7976/MT7915射频前端芯片修改eeprom v2参考[mt7981_factory_txpwr_patch](https://github.com/4n0n4/mt7981_factory_txpwr_patch/blob/main/README_EN.md)。  
2.4 GHz: MT_EE_TX0_POWER_2G_V2 @ 0x441 (4 bytes)  
5 GHz: MT_EE_TX0_POWER_5G_V2 @ 0x445 (20 bytes)   
百里5G无线发射功率23dBm，2.4G发送功率25dBm。  
其中各个功率十六进制数据代表如下：  
23dBm x2A  
24dBm x2B  
25dBm x2C 或 x2D  
百里直接SSH使用下面命令，软修改（即不修改factory分区）5G为24dBm，修改好之后reboot重启即可。  
MT7986_ePAeLNA_EEPROM_AX6000.bin文件只在固件第一次启动时从factory复制出来，所以修改一次即可。  
```
hex_value='\x2B'
printf "$hex_value%.0s" $(seq 1 20) > /tmp/tmp.bin
dd if=/tmp/tmp.bin of=/lib/firmware/MT7986_ePAeLNA_EEPROM_AX6000.bin bs=1 seek=$((0x445)) conv=notrunc
```
当然也可以直接硬修改factory分区，使得以后每次刷新固件都不用再修改了。  
首先备份好原厂factory分区，然后修改MT7986_ePAeLNA_EEPROM_AX6000.bin并写入factory分区，再备份一次factory分区。  
自行到tmp下载保存好备份，然后reboot重启即可。  
```
hex_value='\x2B'
printf "$hex_value%.0s" $(seq 1 20) > /tmp/tmp.bin
dd if=$(blkid -t PARTLABEL=factory -o device) of=/tmp/mmcblk0px_factory_backup.bin conv=fsync
dd if=/tmp/tmp.bin of=/lib/firmware/MT7986_ePAeLNA_EEPROM_AX6000.bin bs=1 seek=$((0x445)) conv=notrunc
dd if=/tmp/tmp.bin of=$(blkid -t PARTLABEL=factory -o device) bs=1 seek=$((0x445)) conv=notrunc
dd if=$(blkid -t PARTLABEL=factory -o device) of=/tmp/mmcblk0px_factory.bin conv=fsync
```
- #### YVR X6改无线发射功率
YVR X6的2.4G 5G天线是通过双工器进行合路输出，出厂是2.4G 25dBm 5G 25dBm。  
25dBm x2E  
26dBm x30  
下面直接硬修改factory分区，把2.4G 5G同时拉到26dBm。  
自行到tmp下载保存好备份，然后reboot重启即可。  
```
hex_value='\x30'
printf "$hex_value%.0s" $(seq 1 24) > /tmp/tmp.bin
mtd_dev=$(grep -w "Factory" /proc/mtd | cut -d: -f1)
dd if="/dev/${mtd_dev}" of="/tmp/${mtd_dev}_Factory_backup.bin"
dd if="/dev/${mtd_dev}" of=/tmp/eeprom.bin
dd if=/tmp/tmp.bin of=/tmp/eeprom.bin bs=1 seek=$((0x441)) conv=notrunc
mtd write /tmp/eeprom.bin Factory
```
- #### 储留箱S20P改无线发射功率
详见刷机文件中的图片。  
</details>

---
## MT7981-eMMC-USB hanwckf workflow 手动运行可选项：
<details>
<summary>展开详细</summary>

- Select Source Branch
- Set LAN IP Address
- Choose WiFi Driver
- [x] Use nx30pro eeprom
- [ ] Use luci-app-mtk wifi config
- [ ] Not build luci-app-dockerman

- #### 说明
源码中的WAN、LAN地址顺序已修复  
RAX3000M算力版（RAX3000M-eMMC）的eMMC默认使用26MHz频率  
RAX3000Z增强版（XR30-eMMC）的eMMC默认使用52MHz频率  
BT-R320的eMMC默认使用52MHz频率  

- #### 1. Select Source Branch
默认使用 [lgs2007m/immortalwrt-mt798x](https://github.com/lgs2007m/immortalwrt-mt798x) 的 [v7672](https://github.com/lgs2007m/immortalwrt-mt798x/tree/v7672) 分支，该分支支持mt_wifi v7.6.7.2和v7.6.6.1，conninfra datconf warp使用v7.6.7.2版本配套的； [v7661](https://github.com/lgs2007m/immortalwrt-mt798x/tree/v7661) 分支也支持mt_wifi v7.6.7.2和v7.6.6.1，conninfra datconf warp改为v7.6.6.1版本配套的旧版。v7.6.7.3版本驱动听说有问题，未测试，观望中。  

- #### 2. Set LAN IP Address
设置LAN IP地址（路由器登录地址），默认192.168.1.1。  

- #### 3. Choose WiFi Driver
默认使用WiFi驱动版本v7.6.7.2-fw-20240823(recommend)。  
mt_wifi的firmware可选，warp默认使用v7.6.7.2配套的warp_20231229-5f71ec，firmware用驱动自带的，不可选。  
【mt7981的机子上未测试，建议直接使用推荐的选项。】  
根据网络和ChatGPT查询，我理解：  
mt_wifi是MediaTek的WiFi驱动程序，主要用于控制无线网络功能，提供WiFi协议栈支持、无线电控制、连接管理等。  
warp是MediaTek的WiFi Warp Accelerator加速框架，通常用于WiFi网络数据处理的加速。  
mt_wifi和warp中的固件（firmware）是驱动程序与无线芯片之间的中间层。它通常被加载到无线芯片中，以控制其硬件功能，管理无线协议并处理数据传输，会影响无线性能和稳定性。  
v7.6.7.2-fw-20240823(recommend) 推荐，使用[mtk-openwrt-feeds(20240823)](https://git01.mediatek.com/plugins/gitiles/openwrt/feeds/mtk-openwrt-feeds/+/9de357f7abdbc03fd2daf9d9a9aa5ccce6f4fc30)  
v7.6.6.1-fw-20230306(recommend) 推荐，使用提取自H3C-NX30Pro固件的fw-20230306  
v7.6.7.2-fw-default 使用驱动包自带firmware fw-20231229  
v7.6.6.1-fw-default 使用驱动包自带firmware fw-20220906  
其他firmware可自行组合尝试：  
fw-20230330 使用提取自TP-XDR3030固件的fw-20230330  
fw-20230411 使用提取自H3C-NX30Pro固件的fw-20230411  
fw-20230717 使用提取自Xiaomi-AX3000T固件的fw-20230717  
fw-20231024 使用mtk-openwrt-feeds(20231024)的fw-20231024  
```
# SSH查看内核版本
uname -a
# 查看WiFi驱动版本
iwpriv ra0 get_driverinfo
# 查看WiFi驱动mt_wifi mt7981 firmware版本
strings /lib/firmware/7981_WACPU_RAM_CODE_release.bin | grep -E '202[0-9]{6}'
strings /lib/firmware/mt7981_patch_e1_hdr.bin | grep -E '202[0-9]{6}'
strings /lib/firmware/WIFI_RAM_CODE_MT7981.bin | grep -E '202[0-9]{6}'
# 查看WiFi驱动warp mt7981 firmware版本
strings /lib/firmware/7981_WOCPU0_RAM_CODE_release.bin | grep -E '202[0-9]{6}'
# 或者
cat /proc/warp_ctrl/warp0/wo
```

- #### 4. Use nx30pro eeprom
该选项默认开启，即使用nx30pro的高功率eeprom，不需要请取消打钩。  
不使用独立fem无线功放的MT7981B路由器可以通过替换高功率的eeprom提高信号强度。  
RAX3000M/XR30的factory eeprom设置功率不高，2.4G是23dBm、5G是22dBm，使用NX30 PRO的高功率eeprom，2.4G可提升至25dBm、5G提升至24dBm。  
开启该选项会使用NX30 PRO的eeprom替换掉固件中的MT7981_iPAiLNA_EEPROM.bin文件。  
如果是SPI NAND机型，还需要删除对应驱动下的这个补丁文件package/mtk/drivers/mt_wifi/patches-76xx/001-eeprom-flash-api.patch。  

- #### 5. Use luci-app-mtk wifi config
该选项默认关闭，即按.mtwifi-cfg.config配置文件，使用mtwifi-cfg配置工具，需要使用旧的luci-app-mtk无线配置工具请打钩。  
mtwifi-cfg：为mtwifi设计的无线配置工具，兼容openwrt原生luci和netifd，可调整无线驱动的参数较少，配置界面美观友好。  
luci-app-mtk：源自mtk-sdk提供的配置工具，需要配合wifi-profile脚本使用，可调整无线驱动的几乎所有参数，配置界面较为简陋。  
区别详见大佬的博客[mtwifi无线配置工具说明](https://cmi.hanwckf.top/p/immortalwrt-mt798x/#mtwifi%E6%97%A0%E7%BA%BF%E9%85%8D%E7%BD%AE%E5%B7%A5%E5%85%B7%E8%AF%B4%E6%98%8E)  
.mtwifi-cfg.config配置文件中已设置使用mtwifi-cfg配置工具：  
CONFIG_PACKAGE_luci-app-mtwifi-cfg=y  
CONFIG_PACKAGE_luci-i18n-mtwifi-cfg-zh-cn=y  
CONFIG_PACKAGE_mtwifi-cfg=y  
CONFIG_PACKAGE_lua-cjson=y  

- #### 6. Not build luci-app-dockerman
该选项默认关闭，即按.mtwifi-cfg.config配置文件编译dockerman，不需要编译dockerman请打钩。  
.mtwifi-cfg.config配置文件中已设置编译dockerman：  
CONFIG_PACKAGE_luci-app-dockerman=y  
</details>
</details>

---
### 感谢P3TERX的Actions-OpenWrt
- [P3TERX](https://github.com/P3TERX/Actions-OpenWrt)
[Read the details in my blog (in Chinese) | 中文教程](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

### 固件-docker
```
CONFIG_TARGET_mediatek=y
CONFIG_TARGET_mediatek_mt7981=y
CONFIG_TARGET_MULTI_PROFILE=y
CONFIG_TARGET_DEVICE_mediatek_mt7981_DEVICE_bt_r320=y
CONFIG_TARGET_DEVICE_PACKAGES_mediatek_mt7981_DEVICE_bt_r320=""
CONFIG_TARGET_DEVICE_mediatek_mt7981_DEVICE_cmcc_rax3000m-emmc=y
CONFIG_TARGET_DEVICE_PACKAGES_mediatek_mt7981_DEVICE_cmcc_rax3000m-emmc=""
CONFIG_TARGET_DEVICE_mediatek_mt7981_DEVICE_cmcc_xr30-emmc=y
CONFIG_TARGET_DEVICE_PACKAGES_mediatek_mt7981_DEVICE_cmcc_xr30-emmc=""
CONFIG_TARGET_PER_DEVICE_ROOTFS=y
##---------下面配置可自行修改-------------
## 主题皮肤
CONFIG_PACKAGE_luci-theme-argon=y
CONFIG_PACKAGE_luci-theme-design=y
CONFIG_PACKAGE_luci-app-dockerman=y
CONFIG_PACKAGE_luci-app-ttyd=y
CONFIG_PACKAGE_luci-app-ddns=y
CONFIG_PACKAGE_ddns-scripts=y
CONFIG_PACKAGE_ddns-scripts_aliyun=y
CONFIG_PACKAGE_ddns-scripts-cloudflare=y
CONFIG_PACKAGE_ddns-scripts-cnkuai=y
CONFIG_PACKAGE_ddns-scripts-digitalocean=y
CONFIG_PACKAGE_ddns-scripts-dnspod=y
CONFIG_PACKAGE_ddns-scripts-dnspod-com=y
CONFIG_PACKAGE_ddns-scripts-freedns=y
CONFIG_PACKAGE_ddns-scripts-gandi=y
CONFIG_PACKAGE_ddns-scripts-godaddy=y
CONFIG_PACKAGE_ddns-scripts-noip=y
CONFIG_PACKAGE_ddns-scripts-nsupdate=y
CONFIG_PACKAGE_ddns-scripts-route53=y
CONFIG_PACKAGE_luci-app-arpbind=y
CONFIG_PACKAGE_luci-app-eqos-mtk=y
CONFIG_PACKAGE_luci-app-upnp=y
CONFIG_PACKAGE_coremark=y
CONFIG_PACKAGE_bash=y
CONFIG_PACKAGE_gdisk=y
CONFIG_PACKAGE_sgdisk=y
CONFIG_PACKAGE_luci-app-samba4=n
CONFIG_PACKAGE_luci-app-usb-printer=n
## 在线安装openlist所需依赖
CONFIG_PACKAGE_kmod-fuse=y
## 在线安装docker所需依赖
CONFIG_PACKAGE_kmod-dm=y
CONFIG_PACKAGE_kmod-br-netfilter=y
CONFIG_PACKAGE_kmod-ikconfig=y
CONFIG_PACKAGE_kmod-nf-ipvs=y
CONFIG_PACKAGE_kmod-veth=y
## 针对eMMC加的一些文件系统和工具
CONFIG_PACKAGE_kmod-fs-ext4=y
CONFIG_PACKAGE_kmod-fs-f2fs=y
CONFIG_PACKAGE_e2fsprogs=y
CONFIG_PACKAGE_f2fs-tools=y
CONFIG_PACKAGE_lsblk=y
CONFIG_PACKAGE_mmc-utils=y
##---------下面默认配置不建议修改-------------
## 使用mtwifi-cfg无线配置
CONFIG_PACKAGE_luci-app-mtwifi-cfg=y
CONFIG_PACKAGE_luci-i18n-mtwifi-cfg-zh-cn=y
CONFIG_PACKAGE_mtwifi-cfg=y
CONFIG_PACKAGE_lua-cjson=y
## 使用新的无线驱动
CONFIG_MTK_MT_WIFI_DRIVER_VERSION_7672=y
CONFIG_MTK_MT_WIFI_MT7981_20240823=y
## 其他默认配置
CONFIG_DEVEL=y
CONFIG_TOOLCHAINOPTS=y
CONFIG_BUSYBOX_CUSTOM=y
CONFIG_AFALG_UPDATE_CTR_IV=y
CONFIG_BUSYBOX_CONFIG_BLKID=y
CONFIG_BUSYBOX_CONFIG_FEATURE_BLKID_TYPE=y
CONFIG_BUSYBOX_CONFIG_VOLUMEID=y
CONFIG_CONNINFRA_AUTO_UP=y
CONFIG_CONNINFRA_EMI_SUPPORT=y
# CONFIG_GDB is not set
CONFIG_INCLUDE_CONFIG=y
CONFIG_JSON_OVERVIEW_IMAGE_INFO=y
# CONFIG_KERNEL_BLK_DEV_THROTTLING is not set
# CONFIG_KERNEL_CFS_BANDWIDTH is not set
CONFIG_KERNEL_CGROUP_DEVICE=y
CONFIG_KERNEL_CGROUP_FREEZER=y
CONFIG_KERNEL_DEVMEM=y
# CONFIG_KERNEL_KEYS is not set
# CONFIG_KERNEL_MEMCG_SWAP is not set
CONFIG_KERNEL_NET_CLS_CGROUP=y
CONFIG_MTK_ACK_CTS_TIMEOUT_SUPPORT=y
CONFIG_MTK_AIR_MONITOR=y
CONFIG_MTK_AMPDU_CONF_SUPPORT=y
CONFIG_MTK_ANTENNA_CONTROL_SUPPORT=y
CONFIG_MTK_APCLI_SUPPORT=y
CONFIG_MTK_ATE_SUPPORT=y
CONFIG_MTK_BACKGROUND_SCAN_SUPPORT=y
CONFIG_MTK_CAL_BIN_FILE_SUPPORT=y
CONFIG_MTK_CFG_SUPPORT_FALCON_MURU=y
CONFIG_MTK_CFG_SUPPORT_FALCON_PP=y
CONFIG_MTK_CFG_SUPPORT_FALCON_SR=y
CONFIG_MTK_CFG_SUPPORT_FALCON_TXCMD_DBG=y
CONFIG_MTK_CHIP_MT7981=y
CONFIG_MTK_CONNINFRA_APSOC=y
CONFIG_MTK_CONNINFRA_APSOC_MT7981=y
CONFIG_MTK_CON_WPS_SUPPORT=y
CONFIG_MTK_DBDC_MODE=y
CONFIG_MTK_DOT11K_RRM_SUPPORT=y
CONFIG_MTK_DOT11R_FT_SUPPORT=y
CONFIG_MTK_DOT11W_PMF_SUPPORT=y
CONFIG_MTK_DOT11_HE_AX=y
CONFIG_MTK_DOT11_N_SUPPORT=y
CONFIG_MTK_DOT11_VHT_AC=y
CONFIG_MTK_FAST_NAT_SUPPORT=y
CONFIG_MTK_FIRST_IF_EEPROM_FLASH=y
CONFIG_MTK_FIRST_IF_IPAILNA=y
CONFIG_MTK_FIRST_IF_MT7981=y
CONFIG_MTK_GREENAP_SUPPORT=y
CONFIG_MTK_G_BAND_256QAM_SUPPORT=y
CONFIG_MTK_HDR_TRANS_RX_SUPPORT=y
CONFIG_MTK_HDR_TRANS_TX_SUPPORT=y
CONFIG_MTK_ICAP_SUPPORT=y
CONFIG_MTK_IGMP_SNOOP_SUPPORT=y
CONFIG_MTK_INTERWORKING=y
CONFIG_MTK_MAP_R2_VER_SUPPORT=y
CONFIG_MTK_MAP_R3_VER_SUPPORT=y
CONFIG_MTK_MAP_SUPPORT=y
CONFIG_MTK_MBSS_DTIM_SUPPORT=y
CONFIG_MTK_MBSS_SUPPORT=y
CONFIG_MTK_MCAST_RATE_SPECIFIC=y
CONFIG_MTK_MEMORY_SHRINK=y
CONFIG_MTK_MEMORY_SHRINK_AGGRESS=y
CONFIG_MTK_MGMT_TXPWR_CTRL=y
CONFIG_MTK_MLME_MULTI_QUEUE_SUPPORT=y
CONFIG_MTK_MT_AP_SUPPORT=m
CONFIG_MTK_MT_DFS_SUPPORT=y
CONFIG_MTK_MT_MAC=y
CONFIG_MTK_MT_WIFI=m
CONFIG_MTK_MT_WIFI_PATH="mt_wifi"
CONFIG_MTK_MUMIMO_SUPPORT=y
CONFIG_MTK_MU_RA_SUPPORT=y
CONFIG_MTK_OFFCHANNEL_SCAN_FEATURE=y
CONFIG_MTK_OWE_SUPPORT=y
CONFIG_MTK_QOS_R1_SUPPORT=y
CONFIG_MTK_RA_PHY_RATE_SUPPORT=y
CONFIG_MTK_RED_SUPPORT=y
CONFIG_MTK_RTMP_FLASH_SUPPORT=y
CONFIG_MTK_RT_FIRST_CARD_EEPROM="flash"
CONFIG_MTK_RT_FIRST_IF_RF_OFFSET=0xc0000
CONFIG_MTK_SCS_FW_OFFLOAD=y
CONFIG_MTK_SECOND_IF_NONE=y
CONFIG_MTK_SMART_CARRIER_SENSE_SUPPORT=y
CONFIG_MTK_SPECTRUM_SUPPORT=y
CONFIG_MTK_SUPPORT_OPENWRT=y
CONFIG_MTK_THIRD_IF_NONE=y
CONFIG_MTK_TPC_SUPPORT=y
CONFIG_MTK_TXBF_SUPPORT=y
CONFIG_MTK_UAPSD=y
CONFIG_MTK_VLAN_SUPPORT=y
CONFIG_MTK_VOW_SUPPORT=y
CONFIG_MTK_WARP_V2=y
CONFIG_MTK_WDS_SUPPORT=y
CONFIG_MTK_WHNAT_SUPPORT=m
CONFIG_MTK_WIFI_BASIC_FUNC=y
CONFIG_MTK_WIFI_DRIVER=y
CONFIG_MTK_WIFI_EAP_FEATURE=y
CONFIG_MTK_WIFI_FW_BIN_LOAD=y
CONFIG_MTK_WIFI_MODE_AP=m
CONFIG_MTK_WIFI_MT_MAC=y
CONFIG_MTK_WIFI_TWT_SUPPORT=y
CONFIG_MTK_WLAN_HOOK=y
CONFIG_MTK_WLAN_SERVICE=y
CONFIG_MTK_WNM_SUPPORT=y
CONFIG_MTK_WPA3_SUPPORT=y
CONFIG_MTK_WSC_INCLUDED=y
CONFIG_MTK_WSC_V2_SUPPORT=y
# CONFIG_OPENSSL_ENGINE_BUILTIN is not set
# CONFIG_OPENSSL_PREFER_CHACHA_OVER_GCM is not set
CONFIG_OPENSSL_WITH_NPN=y
# CONFIG_PACKAGE_TURBOACC_INCLUDE_FLOW_OFFLOADING is not set
CONFIG_PACKAGE_TURBOACC_INCLUDE_NO_FASTPATH=y
CONFIG_PACKAGE_blockd=y
CONFIG_PACKAGE_ca-certificates=y
CONFIG_PACKAGE_datconf=y
CONFIG_PACKAGE_datconf-lua=y
CONFIG_PACKAGE_ebtables=y
CONFIG_PACKAGE_ethtool=y
CONFIG_PACKAGE_htop=y
CONFIG_PACKAGE_ip-bridge=y
CONFIG_PACKAGE_ip6tables-extra=y
CONFIG_PACKAGE_ipset=y
CONFIG_PACKAGE_iptables-mod-conntrack-extra=y
CONFIG_PACKAGE_iptables-mod-extra=y
CONFIG_PACKAGE_iptables-mod-filter=y
CONFIG_PACKAGE_iptables-mod-hashlimit=y
CONFIG_PACKAGE_iptables-mod-iface=y
CONFIG_PACKAGE_iptables-mod-ipmark=y
CONFIG_PACKAGE_iptables-mod-ipopt=y
CONFIG_PACKAGE_iptables-mod-iprange=y
CONFIG_PACKAGE_iptables-mod-ipv4options=y
CONFIG_PACKAGE_iptables-mod-nat-extra=y
CONFIG_PACKAGE_iptables-mod-proto=y
CONFIG_PACKAGE_iptables-mod-tee=y
CONFIG_PACKAGE_iptables-mod-tproxy=y
CONFIG_PACKAGE_iptables-mod-u32=y
CONFIG_PACKAGE_iw=y
CONFIG_PACKAGE_iwinfo=y
CONFIG_PACKAGE_kmod-ata-core=y
CONFIG_PACKAGE_kmod-conninfra=y
CONFIG_PACKAGE_kmod-crypto-acompress=y
CONFIG_PACKAGE_kmod-crypto-ccm=y
CONFIG_PACKAGE_kmod-crypto-cmac=y
CONFIG_PACKAGE_kmod-crypto-crc32c=y
CONFIG_PACKAGE_kmod-crypto-ctr=y
CONFIG_PACKAGE_kmod-crypto-des=y
CONFIG_PACKAGE_kmod-crypto-gcm=y
CONFIG_PACKAGE_kmod-crypto-gf128=y
CONFIG_PACKAGE_kmod-crypto-ghash=y
CONFIG_PACKAGE_kmod-crypto-hmac=y
CONFIG_PACKAGE_kmod-crypto-md4=y
CONFIG_PACKAGE_kmod-crypto-md5=y
CONFIG_PACKAGE_kmod-crypto-rng=y
CONFIG_PACKAGE_kmod-crypto-seqiv=y
CONFIG_PACKAGE_kmod-crypto-sha256=y
CONFIG_PACKAGE_kmod-crypto-sha512=y
CONFIG_PACKAGE_kmod-ebtables=y
CONFIG_PACKAGE_kmod-ebtables-ipv4=y
CONFIG_PACKAGE_kmod-ebtables-ipv6=y
CONFIG_PACKAGE_kmod-fs-autofs4=y
CONFIG_PACKAGE_kmod-fs-vfat=y
CONFIG_PACKAGE_kmod-ifb=y
CONFIG_PACKAGE_kmod-inet-diag=y
CONFIG_PACKAGE_kmod-ip6tables-extra=y
CONFIG_PACKAGE_kmod-ipt-compat-xtables=y
CONFIG_PACKAGE_kmod-ipt-conntrack-extra=y
CONFIG_PACKAGE_kmod-ipt-extra=y
CONFIG_PACKAGE_kmod-ipt-filter=y
CONFIG_PACKAGE_kmod-ipt-hashlimit=y
CONFIG_PACKAGE_kmod-ipt-iface=y
CONFIG_PACKAGE_kmod-ipt-ipmark=y
CONFIG_PACKAGE_kmod-ipt-ipopt=y
CONFIG_PACKAGE_kmod-ipt-iprange=y
CONFIG_PACKAGE_kmod-ipt-ipv4options=y
CONFIG_PACKAGE_kmod-ipt-nat-extra=y
CONFIG_PACKAGE_kmod-ipt-offload=y
CONFIG_PACKAGE_kmod-ipt-proto=y
CONFIG_PACKAGE_kmod-ipt-raw6=y
CONFIG_PACKAGE_kmod-ipt-tee=y
CONFIG_PACKAGE_kmod-ipt-tproxy=y
CONFIG_PACKAGE_kmod-ipt-u32=y
CONFIG_PACKAGE_kmod-lib-crc32c=y
CONFIG_PACKAGE_kmod-lib-lzo=y
CONFIG_PACKAGE_kmod-mediatek_hnat=y
CONFIG_PACKAGE_kmod-mt_wifi=y
CONFIG_PACKAGE_kmod-nf-flow=y
CONFIG_PACKAGE_kmod-nls-base=y
CONFIG_PACKAGE_kmod-nls-cp437=y
CONFIG_PACKAGE_kmod-nls-iso8859-1=y
CONFIG_PACKAGE_kmod-nls-utf8=y
CONFIG_PACKAGE_kmod-sched-core=y
CONFIG_PACKAGE_kmod-scsi-core=y
CONFIG_PACKAGE_kmod-tcp-bbr=y
CONFIG_PACKAGE_kmod-tun=y
CONFIG_PACKAGE_kmod-warp=y
CONFIG_PACKAGE_kmod-zram=y
CONFIG_PACKAGE_kvcedit=y
CONFIG_PACKAGE_libatomic=y
CONFIG_PACKAGE_libblkid=y
CONFIG_PACKAGE_libcap-ng=y
CONFIG_PACKAGE_libcbor=y
CONFIG_PACKAGE_libevdev=y
CONFIG_PACKAGE_libfido2=y
CONFIG_PACKAGE_libipset=y
CONFIG_PACKAGE_libkvcutil=y
CONFIG_PACKAGE_libncurses=y
CONFIG_PACKAGE_libnl=y
CONFIG_PACKAGE_libnl-core=y
CONFIG_PACKAGE_libnl-genl=y
CONFIG_PACKAGE_libnl-nf=y
CONFIG_PACKAGE_libnl-route=y
CONFIG_PACKAGE_libopenssl-afalg_sync=y
CONFIG_PACKAGE_libopenssl-devcrypto=y
CONFIG_PACKAGE_libpcap=y
CONFIG_PACKAGE_libstdcpp=y
CONFIG_PACKAGE_libudev-zero=y
CONFIG_PACKAGE_libuuid=y
# CONFIG_PACKAGE_luci-app-rclone_INCLUDE_rclone-ng is not set
# CONFIG_PACKAGE_luci-app-rclone_INCLUDE_rclone-webui is not set
# CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_ChinaDNS_NG is not set
CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_NONE_V2RAY=y
# CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_ShadowsocksR_Libev_Client is not set
CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_Shadowsocks_NONE_Client=y
CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_Shadowsocks_NONE_Server=y
# CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_Shadowsocks_Rust_Client is not set
# CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_Shadowsocks_Rust_Server is not set
# CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_Shadowsocks_Simple_Obfs is not set
# CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_Xray is not set
CONFIG_PACKAGE_luci-app-turboacc-mtk=y
# CONFIG_PACKAGE_luci-app-vssr_INCLUDE_ShadowsocksR_Libev_Server is not set
# CONFIG_PACKAGE_luci-app-vssr_INCLUDE_Trojan is not set
# CONFIG_PACKAGE_luci-app-vssr_INCLUDE_Xray is not set
# CONFIG_PACKAGE_luci-app-vssr_INCLUDE_Xray_plugin is not set
CONFIG_PACKAGE_luci-i18n-turboacc-mtk-zh-cn=y
CONFIG_PACKAGE_mii_mgr=y
CONFIG_PACKAGE_mtkhqos_util=y
CONFIG_PACKAGE_nano=y
CONFIG_PACKAGE_openssh-keygen=y
CONFIG_PACKAGE_openssh-sftp-server=y
CONFIG_PACKAGE_openssl-util=y
CONFIG_PACKAGE_regs=y
CONFIG_PACKAGE_resolveip=y
CONFIG_PACKAGE_swconfig=y
CONFIG_PACKAGE_switch=y
CONFIG_PACKAGE_tc-mod-iptables=y
CONFIG_PACKAGE_tc-tiny=y
CONFIG_PACKAGE_tcpdump=y
CONFIG_PACKAGE_terminfo=y
CONFIG_PACKAGE_wifi-dats=y
CONFIG_PACKAGE_wireless-regdb=y
CONFIG_PACKAGE_wireless-tools=y
CONFIG_PACKAGE_zram-swap=y
# CONFIG_PKG_CHECK_FORMAT_SECURITY is not set
# CONFIG_PKG_FORTIFY_SOURCE_1 is not set
CONFIG_PKG_FORTIFY_SOURCE_2=y
CONFIG_WARP_CHIPSET="mt7981"
CONFIG_WARP_DBG_SUPPORT=y
CONFIG_WARP_VERSION=2
CONFIG_WED_HW_RRO_SUPPORT=y
# CONFIG_WOLFSSL_HAS_ECC25519 is not set
CONFIG_first_card=y
CONFIG_first_card_name="MT7981"
# CONFIG_AFALG_FALLBACK is not set
# CONFIG_MTK_BAND_STEERING is not set
# CONFIG_MTK_DEFAULT_5G_PROFILE is not set
# CONFIG_MTK_MAC_REPEATER_SUPPORT is not set
# CONFIG_MTK_MULTI_PROFILE_SUPPORT is not set
# CONFIG_MTK_PCIE_ASPM_DYM_CTRL_SUPPORT is not set
# CONFIG_MTK_PRE_CAL_TRX_SET1_SUPPORT is not set
# CONFIG_MTK_PRE_CAL_TRX_SET2_SUPPORT is not set
# CONFIG_MTK_RLM_CAL_CACHE_SUPPORT is not set
# CONFIG_MTK_SNIFFER_RADIOTAP_SUPPORT is not set
```
