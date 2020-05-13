---
layout: default
title:  "树莓派4B+ homeKit"
date:   2020-02-15 00:00:00
categories: main
---
<!-- https://shumeipai.nxez.com/2017/09/04/homekit-and-siri-voice-control-home-appliances.html -->
# 树莓派4B+使用homeKit
说明：

1.树莓派系统镜像：
2019-09-26-raspbian-buster-full.img
md5:941D122B724C8154FFF5785E4FF0F68A

2.nodejs 8.2.0 + npm5.2.0


安装指南：

1.卸载系统自带的nodejs和nvm，由于版本过高会带来一系列的问题（其实最主要的问题是npm的插件不兼容）。
sudo apt-get remove nodejs

确认是否卸载干净
```
node -v
npm -v 
sudo node -v
sudo npm -v
```

2.使用nvm安装指定版本的nodejs

```
pi@raspberrypi:~ $curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

pi@raspberrypi:~ $ source ~/.bashrc

//这里会自动安装配套的npm版本
pi@raspberrypi:~ $ nvm install 8.2.0
```
3.安装wiringpi树莓派GPIO的库(这个镜像里面默认是带的)
```
pi@raspberrypi:~ $sudo apt-get install wiringpi
```
4.安装homebridge
```
pi@raspberrypi:~ $npm install -g homebridge-gpio-wpi2
```

5.安装gpio的调用库
```
pi@raspberrypi:~ $sudo npm install -g homebridge-gpio-wpi2
```

6.硬件连接
在启动homebridge 之前，首先将LED 发光二极管正极，通过杜邦线连接在 树莓派 gpio 口的27 管脚，也就是 靠近芯片那一侧，从上往下数第7个管脚。二极管负极，通过另一根杜邦线连接在 树莓派gpio的ground 管脚，我这里选择是第39管脚。

注意，插杜邦线要注意正负极，配合管脚图谨慎操作，避免树莓派gpio口被烧坏。请根据自己二极管可承受电流，选择性使用电阻来限流。

软件配置
在普通用户pi家目录 新建/home/pi/.homebridge/config.json文件，复制以下内容到config.json ，保存。
```
{
        "bridge": {
                "name": "Homebridge",
                "username": "CC:22:3D:E3:CE:30",
                "port": 51826,
                "pin": "133-45-678"
        },
        "platforms": [{
                 "platform" : "WiringPiPlatform",
                "name" : "Pi GPIO (WiringPi)",
                "overrideCache" : "true",
                "autoExport" : "true",
                "gpiopins" : [{
                        "name" : "灯",
                        "pin"  : 27,
                        "enabled" : "true",
                        "mode" : "out",
                        "pull" : "down",
                        "inverted" : "false",
                        "duration" : 0,
                        "polling" : "true"
                        },{
                        "name" : "门",
                        "pin"  : 22,
                        "enabled" : "true",
                        "mode" : "in",
                        "pull" : "off",
                        "inverted" : "false",
                        "duration" : 0
                }]
        }]
}
```
在命令行输入homebridge启动,如下图说明启动成功

```
    1  sudo apt-get update
    2  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
    3  source ~/.bashrc
    4  nvm install 8.2.0
    5  sudo npm install -g --unsafe-perm homebridge
    6  sudo apt-get install wiringpi
    7  sudo npm install -g homebridge-gpio-wpi2
    8  node -v
    9  which node
   10  node -v
   11  npm -v
   12  ls /usr/bin/ | grep node
   13  sudo apt-get remove node
   14  sudo apt-get remove nodejs
   15  node
   16  node -v
   17  which node
   18  npm -v
   19  which npm
   20  history
   21  sudo npm install -g --unsafe-perm homebridge
   22  which node
   23  sudo ln -s .nvm/versions/node/v8.2.0/bin/node /usr/local/bin/node
   24  sudo ln -s .nvm/versions/node/v8.2.0/bin/npm /usr/local/bin/npm
   25  sudo npm install -g --unsafe-perm homebridge
   26  which node
   27  ls -l /usr/local/bin/node  
   28  sudo node -v
   29  npm install -g --unsafe-perm homebridge
   30  npm install -g homebridge-gpio-wpi2
   31  nano /home/pi/.homebridge/config.json
   32  /home/pi/.homebridge/config.json
   33  mkdir .homebridge
   34  nano .homebridge/config.json
   35  homebridge
   36  history -w
   37  history
```