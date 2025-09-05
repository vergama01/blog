# ArchLinux 安装及配置

## 1. 安装

使用archinstall安装，没什么难度，使用archinstall脚本安装需用联网，使用`iwctl`链接网络

``` bash
iwctl
# 此时已经进入iwctl命令行，当前行会显示[iwd]#
# 列出所有的WiFi网卡
device list
# 此时会列出所有的WiFi网卡，一般使用第一个即可，也就是wlan0
# 下面命令假设使用wlan0
# 列出所有可以搜索到的网络
station wlan0 get-networks
# 连接网络，SSID换成网络名称
station wlan0 connect [SSID]
# 接下来可能会要你输入该WiFi的密码，正确输入后回车即可
# 连接完成后即可退出iwctl命令行
exit
# 此时已经退出iwctl命令行，尝试ping一下，看看能不能联网
ping baidu.com
```

## 2.配置选项

### 2.1 配置archlinuxcn源

使用方法：在 `/etc/pacman.conf` 文件末尾添加以下两行：

```bash
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

之后通过以下命令安装 `archlinuxcn-keyring` 包导入 GPG key。

```bash
pacman -Sy archlinuxcn-keyring
```

上面是清华的源，如需要其他的国内源，请自行更改。

[archlinuxcn源参考文章](https://mirrors.tuna.tsinghua.edu.cn/help/archlinuxcn/)

### 2.2

...

## 3. 桌面环境选择

### 3.1 选择桌面 

目前个人使用的是hyprland，由于配置选项过多，使用的是[HyDe](https://hydeproject.pages.dev/en/getting-started/introduction/)的脚本安装的环境，会安装的软件比较多，可以自行卸载。

关于aur库的工具选择，个人喜欢使用paru，会在HyDe的脚本中安装，自己根据需要选择yay和paru

目前HyDe安装后的sddm发生了一些错误，无法登录，目前是禁用了sddm登录，使用tty登录，手动运行hyprland启动桌面环境

### 3.2 输入法

fcitx5 + rime + 小鹤双拼

```bash
paru -S fcitx5 fcitx5-rime 
```

安装完双拼方案后会发现 rime 输入方案选单里是没有刚刚下载的双拼的，需要自己添加进去 在 rime 的配置文件目录下新建名为 default.custom.yaml 的文件，填入以下内容：

 ```bash
  patch:  
  	schema_list:   
  		- {schema: double_pinyin_flypy} 
 ```

这里使用的是小鹤双拼，自然码则是 double_pinyin，微软双拼是 double_pinyin_mspy，如果还需要其他输入方案，在 schema_list 下添加。 保存文件，重新部署 rime 生效，然后就可以打双拼了。

[双拼输入法参考文章](https://www.bilibili.com/opus/611505370455620358)

## 3.3 字体

使用微软字体

将 Windows 的字体复制到 `/usr/local/share/fonts/`：

```bash
# mkdir /usr/local/share/fonts
# mkdir /usr/local/share/fonts/WindowsFonts
# cp /windows/Windows/Fonts/* /usr/local/share/fonts/WindowsFonts/
# chmod 644 /usr/local/share/fonts/WindowsFonts/*
```

然后重新生成字体缓存：

```bash
# fc-cache --force
# fc-cache-32 --force
```

要使用微软字体，需要将上述通用名称映射到微软的字体：

```bash
 <?xml version="1.0"?>
 <!DOCTYPE fontconfig SYSTEM "fonts.dtd">
 <fontconfig>
 <!-- Map generics to MS specifics -->
        <!-- PostScript -->
        <alias binding="same">
          <family>Helvetica</family>
          <accept>
          <family>Arial</family>
          </accept>
        </alias>
        <alias binding="same">
          <family>Times</family>
          <accept>
          <family>Times New Roman</family>
          </accept>
        </alias>
        <alias binding="same">
          <family>Courier</family>
          <accept>
          <family>Courier New</family>
          </accept>
        </alias>
 </fontconfig>
```

一些微软的 TTF 字体，如 Calibri 和 Cambria，包含了特定大小的内嵌位图字体，这些字体不支持抗锯齿。如果启用了字体内嵌位图，在这些特定的尺寸下，字体不会被进行抗锯齿处理。通过[字体配置](https://wiki.archlinuxcn.org/wiki/字体配置)可以禁用内嵌位图字体：

```bash
 <?xml version="1.0"?>
 <!DOCTYPE fontconfig SYSTEM "fonts.dtd">
 <fontconfig>
   <match target="font">
     <edit name="embeddedbitmap" mode="assign">
       <bool>false</bool>
     </edit>
   </match>
 </fontconfig>
```

[微软字体设置参考文章](https://wiki.archlinuxcn.org/wiki/%E5%BE%AE%E8%BD%AF%E5%AD%97%E4%BD%93)

### 3.4 开发相关

java环境配置，[jdk17下载](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)



node管理，使用fnm管理node版本，安装前需要卸载自带的nodejs

```bash
# Download and install fnm:
curl -o- https://fnm.vercel.app/install | bash

# Download and install Node.js:
fnm install 22

# Verify the Node.js version:
node -v # Should print "v22.19.0".

# Download and install Yarn:
corepack enable yarn

# Verify Yarn version:
yarn -v
```

 ### 3.5 常用软件

浏览器： zen-browser，google-chrome

音乐：spotify

直播/录屏：obs-studio

ide：visual-studio-code-bin，idea，rustrover

游戏：steam，lutris，GE-proton

其他：localsend，yazi

## 4. 注意

### 4.1 timeshift

timeshift 在wofi中无法启动，个人解决方式

```bash
sudo -E timeshift-gtk
```





......