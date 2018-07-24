# OpenWRT/LEDE Project Packages Repository (papal-repo)
This repo includes custom packages primarily related to SQM and bufferbloat mitigation.

## How to Use

#### Online Install
To add this repo to your OpenWrt/LEDE Project router run the following commands.  
(NOTE: Versions of LEDE/OpenWRT earlier that 17.01.5 include an out-of-date `CAKE` kernel module and `iproute2` `tc` utility, which **MUST** be manually upgraded to more recent versions.)

###### LEDE 17.01.5, OpenWrt 18.xx or later
```
opkg update; opkg install libustream-mbedtls
wget -O /tmp/papal-repo.pub https://raw.githubusercontent.com/guidosarducci/papal-repo/master/papal-repo.pub && opkg-key add /tmp/papal-repo.pub
sed -n -i '/papal_repo/!p;$a src/gz papal_repo https://raw.githubusercontent.com/guidosarducci/papal-repo/master' /etc/opkg/customfeeds.conf
opkg update
```

#### Offline Install
###### Image Builder
Add the following line
```
src/gz papal_repo https://raw.github.com/guidosarducci/papal-repo/master
```
to the ```repositories.conf``` file inside your Image Builder directory. You can use the following shell script code to achieve that:
```
sed -n -i '/papal_repo/!p;$a src/gz papal_repo https://raw.github.com/guidosarducci/papal-repo/master' repositories.conf
```

## Description of Packages

#### sqm-scripts & luci-app-sqm
These are enhanced versions of the standard OpenWRT SQM packages, developed and tested independently for more than a year, which provide major improvements in ease of use, configuration, functionality and robustness. Further details of the changes and usage guidelines are available in the [TODO](https://github.com/guidosarducci/papal-repo/wiki). However, much of the content in the [OpenWRT User Guide](https://openwrt.org/docs/guide-user/network/traffic-shaping/sqm) is still applicable, noting the following:
- Legacy `.qos` scripts (e.g. `layer_cake`, `simple`) have been removed; all aspects of their functionality are now directly configurable by users from the GUI or CLI. Any previously configured legacy scripts are emulated after package installation for a seamless upgrade.
- Some complicated but important qdisc-specific settings are now simply configurable within the GUI. This includes CAKE's per-host fairness and ACK filter settings.
- Link Layer adaptation can now explicitly set framing compensation (ATM/PTM), MPU and packet overhead, which are applied generally across all qdiscs. Users can also choose to import predefined adaptation settings (e.g. "DOCSIS Cable", "ADSL pppoe-vcmux").

**CAUTION:**
When installed, these packages (versioned 2.x.x) will upgrade the standard packages (versioned 1.x.x) and migrate an existing SQM config file to a newer schema. **Before installing these packages, make a backup of your existing SQM configuration:**
```
cp /etc/config/sqm /etc/config/sqm-v1-backup
```




