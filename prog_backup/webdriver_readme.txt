https://images.nvidia.com/mac/pkg/387/WebDriver-387.10.10.10.40.134.pkg

cat /System/Library/CoreServices/SystemVersion.plist
需要的工具文件：
WebDriver-387.10.10.10.40.113.pkg
Kext Utility
PlistEdit Pro
1、解压此驱动包WebDriver-387.10.10.10.40.113.pkg, 修改 Distribution 文件
pkgutil –expand ./WebDriver-387.10.10.10.40.113.pkg ./WebDriver-387.10.10.10.40.113
找到并删除验证代码 if (!validateSoftware()) return false;
function InstallationCheck() { if (!validateSoftware()) return false; #删除这一行 return true; }
找到如下代码, 大概在42行:
function validateSoftware() { var supportedOSVer = “10.13.6”; var supportedOSBuildVer = “17G10021”; var targetBuild = system.version.ProductBuildVersion; var result = compareBuildVersions(targetBuild, supportedOSBuildVer);
到这就明白了把版本号改成17G66, 保存文件, 重新打包。
pkgutil –flatten ./WebDriver-387.10.10.10.40.113 ./WebDriver-387.10.10.10.40.113-17G66.pkg
然后就可以顺利安装了, 记住安装完成后不要重启, 需要继续修改文件。
快捷键 Shift + G 进入目录 /Library/Extensions 将文件 NVDAStartupWeb.kext 拷贝到桌面然后右键显示包内容打开文件夹Contents, 使用PlistEdit Pro修改Info.plist文件。
将节点NVDARequiredOS的值修改成17G66
然后替换/Library/Extensions目录下的NVDAStartupWeb.kext, 最后使用Kext Utility修复权限, 只需要打开它等待它自动修复完成, 提示Enjoy….就行了。
重点是Clover中config.plist文件的配置:
引导参数增加nvda_drv=1，
显卡设置界面没有特别设置
系统参数打勾NvidiaWeb。
最后重启
