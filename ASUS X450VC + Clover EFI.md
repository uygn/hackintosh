亲测用clover安装mavericks10.9.0到华硕笔记本x450vc，硬盘为got分区，用clover efi模式引导，安装好后可以升级到10.9.5（安装Xcode需要至少10.9.4）。

首先确保硬盘是gpt分区模式并且有efi分区。
先用分区工具（如Paragon Parition Manager），分出至少30G的硬盘来安装mavericks。
如果想要从硬盘启动，需要保证硬盘efi分区（ESP）的大小至少为200M（可用分区工具调整）。

1. 制作clover+mavericks安装u盘

下载：
a. BootDiskUtility: http://cvad-mac.narod.ru/index/0-5 or http://www.hackintoshosx.com/files/file/3767-bootdiskutility-v202014/
b. TransMac: http://www.acutesystems.com/scrtm.htm
c. OS X 10.9.0: 下载下来的文件解开后，最好确保验证InstallESD.dmg的MD5为f222952400db8535c03697c3293e168e

接下来参考进行u盘制作：
https://insanelydeepak.wordpress.com/2014/10/15/guide-how-to-create-os-x-installer-on-windows-pcs-vanilla-installation-method/
http://www.dognmonkey.com/techs/create-mavericks-10-9-2-usb-boot-and-installer-with-windows-8-no-mac.html
注意，在安装clover的时候：
A. /EFI/Clover/config.plist，可以用https://raw.githubusercontent.com/RehabMan/OS-X-Clover-Laptop-Config/master/config_HD4000_1366x768.plist替换。
B. 进入/EFI/Clover/drivers64UEFI:
  a. 下载https://github.com/JrCs/CloverGrowerPro/raw/master/Files/HFSPlus/X64/HFSPlus.efi
  b. 删除VBox*.efi（efi驱动说明见http://clover-wiki.zetam.org/What-is-what#EFI-drivers）
C. 进入/EFI/Clover/kexts/10.9，下载FakeSMC/网卡/usb 3.0/触摸板驱动：
  a. FakeSMC.kext，NullCpuPowermanagement.kext（这个也可不要），从http://www.tonymacx86.com/downloads.php?do=cat&id=11下载。
  b. RealtekRTL8111.kext，从http://www.insanelymac.com/forum/files/category/5-lan-and-wireless/
下载RealtekRTL8111-Binary下载。
  c. GenericUSBXHCI.kext，从http://sourceforge.net/projects/genericusbxhci/下载。
  d. SmartTouchPad.kext，从http://forum.osxlatitude.com/index.php?/topic/1948-elan-and-focaltech-smart-touchpad-driver-mac-os-x/下载SmartTouchPad_v4.2_Final.zip。

2. 安装mavericks系统

重启系统，ESC进入bios设置，确保SATA设置为ACPI。

之后参考进行安装：
http://www.tonymacx86.com/yosemite-laptop-support/148093-guide-booting-os-x-installer-laptops-clover-uefi.html
http://www.tonymacx86.com/mavericks-desktop-guides/125632-how-install-os-x-mavericks-using-clover.html
http://skylineosx.com/installation/installing-osx/

安装好后重启时用usb引导进入clover界面，选择安装mavericks的分区，进入。经过一系列设置步骤，最后进入mavericks。如果你不想每次启动都选择usb进行引导，就需要把clover安装到硬盘的efi分区。

3. 安装clover到硬盘
从http://sourceforge.net/projects/cloverefiboot/下载得到Clover_v2.3k_r3193.zip，解压后打开Clover_v2.3k_r3193.pkg进行安装，选择安装mavericks的分区和customize定制：
a. 旋转"Install for UEFI booting only", "Install Clover in the ESP"会自动选上。
c.  theme选择BootCamp和Bluemac。（theme预览：http://clover-wiki.zetam.org/theme-database）
b. 选择Drivers64UEFI下的”OsxAptioFixDrv-64”。
（关于是否需要选择EmuVar*，请参考http://www.insanelymac.com/forum/topic/298027-guide-aio-guides-for-hackintosh/page-2#entry2029552）


安装完后，打开terminal，进入/Volumns/ESP/CLOVER，重做第一步clover安装的三个步骤。

之后重启，ESC进入bios，boot下选择新建boot项，名称clover，磁盘选硬盘，目录用/EFI/CLOVER/CLOVERX64.efi，然后create，保存设置。重启。

4. 无线网卡
自带无线网卡（Atheros AR9485）好像没有驱动可用（try http://rog.asus.com/forum/showthread.php?39406-ASUS-G750JX-OSX-Mavericks-INSTALL-GUIDE&s=5c56a9d59f087e1feef05cab81bbc7aa&p=332133&viewfull=1），可以买个三四十块的usb网卡，jd上可以找到这些：
RTL8192CU芯片：EDUP（EDUP） EP-MS1558（亲测可用），TOTOLINK N300UM，Netcore NW362，COMFAST CF-WU825N，迅捷（FAST）FW300U，磊科（netcore） NW367（？），TP-LINK TL-WN823N（？）
RTL8188SU芯片：磊科（netcore）NW336 150M
RTL8191SU芯片：水星（Mercury） MW300TV 300M，Netcore NW360

RTL8192CU的驱动可以从http://www.hackintoshosx.com/files/download/4331-tp-link-tl-wn723n-v3-tl-wn725n-v2-usb-wifi/下载4in1_Wlan_11n_USB_MacOS10.10_MacOS10.9_Driver_83.29.12.03_UI_2.3.1.zip和Patched_WNU_2.3.1_for_NonEnglishLocale.zip进行安装，前面那个提供的wifi配置程序据说（我用的英文环境）在中文环境下会崩溃，可以用后面这个进行替换。

Realtek官网提供更多芯片的驱动：
http://www.realtek.com.tw/downloads/downloadsView.aspx?Langid=1&PNid=21&PFid=48&Level=5&Conn=4&ProdID=277&DownTypeID=3&GetDown=false&Downloads=true


5. 声卡（没有亲测）
VoodooHDA据说可以支持。

AppleHDA据说音质更好，见http://www.tonymacx86.com/laptop-compatibility/99966-hd4000-hm76-core-i3-ivybridge.html：
Audio: Best result with patched AppleHDA. Google 'patch AppleHDA guide'. Search for suitable AppleHDA or patch it yourself. Apply appropriate DSDT patches. 

DSDT patches you need for AppleHDA: https://github.com/RehabMan/Laptop-DSDT-Patch
Apply: "Audio Layout 12" (change layout-id from 12 to the layout-id used by the patched AppleHDA)
Apply: "IRQ Fix"


AppleHDA难度较大，可以参考：
http://forum.osxlatitude.com/index.php?/topic/1946-complete-applehda-patching-guide/
http://www.insanelymac.com/forum/topic/295001-guide-to-patch-applehda-for-your-codec/

6. 参考：
http://www.rampagedev.com/?page_id=144
http://www.insanelymac.com/forum/topic/298027-guide-aio-guides-for-hackintosh/

谢谢pcbeta！
谢谢tonymacx86！
谢谢insanelymac！
谢谢本文所有引用链接的原作者！
