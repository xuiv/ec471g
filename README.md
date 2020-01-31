引导到Recovery模式，打开命令终端运行: ```csrutil disable```
然后重启进入黑苹果

AR5B125无线网卡安装: 
```
sudo -s
mkdir /Volumes/ESP && mount_msdos /dev/disk0s1 /Volumes/ESP
cd /Volumes/ESP/EFI/CLOVER/prog_backup
cp -R -H AirPortAtheros40.kext /System/Library/Extensions/IO80211Family.kext/Contents/PlugIns/
cp -R -H ATH9KInjector.kext /System/Library/Extensions/
touch /System/Library/Extensions && kextcache -u /
```

因为不能正确的加载kernel cache，启动时总是随机禁行，我在这里使用EmuVariableUefi-64.efi虚拟NVRAM，配合OsxAptioFixDrv-64.efi解决这个问题，NVRAM需要Clover中的RC Script的支持保存内容到EFI分区根目录的NVRAM.plist中，你需要用Clover_v2.5k_r5103.pkg重新安装Clover到macos分区，一定要选中RC Script，另两个RC script是不需要的，不需要安装到任何apple分区，之后可以删掉macos分区中的EFI和EFI_backup目录。重启之后可以在EFI分区根目录生成NVRAM.plist，主要用来保存屏幕亮度。

没有测试休眠是否工作，如果一睡不醒，可以关掉休眠：
```
sudo -s
pmset -a hibernatemode 0
pmset -a standby 0
pmset -a autopoweroff 0
rm /var/vm/sleepimage
mkdir /var/vm/sleepimage
```
这几条指令还未测试过，从网上抄来的。

config.plist来自于https://github.com/holoto/ec-471g

测试webdriver无法驱动gt630m独显，macosx不支持nvidia的软件切换独显，如果是bios里能禁独显，或者独显不通过集显可以独立输出信号，这两种情况都是可驱动的。

我的机器上集显，声卡，有线网卡，无线网卡，亮度调节都是正常的，只是低分屏字体太糟糕，还不如linux，只是因为要使用checkra1n还装的，个人感觉apple的软件是最差的，远不如微软和开源社区，功能有限，处处加密，其实漏洞最多。
