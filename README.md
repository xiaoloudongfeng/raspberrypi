基于树莓派的室内温湿度监控服务
==========================
## 1.简介
树莓派的功能很强大，我的初衷是想通过树莓派的GPIO，控制dht22获取室内温湿度，并显示在12864屏幕上<br>
为了充分利用12864的显示空间，又实现了实时监控树莓派的CPU、内存使用率的功能，并显示我所在地的天气<br>
12864屏幕的显示内容有：日期时间；CPU、内存利用率；室内温湿度；天气情况（爬自中国天气网）<br>
后来又实现了一个简单的异步http服务，将采集到的数据以json格式发送给Android的客户端，用曲线图的形式展示出来<br>
后面估计还会添加一些功能，比如将采集到的数据存入redis中统计分析之类的，这类功能简单易扩展<br>
gpio.jpg是手画的连线图，有点乱，将就看吧，整个服务使用C语言编写，个人比较熟悉，不用python是因为不会（惨。。）<br>

## 2.依赖
GPIO库用的是WiringPi，dht22对时序要求比较严格，linux不是实时操作系统，读dht22的总线信号时经常会出问题（漏信号）<br>
这个库性能比较好，能降低出问题的概率，但是需要root权限，要修改执行文件的特殊权限s，shell脚本chlcd.sh实现了该功能<br>
我的板子是RaspberryPi 2，RaspberryPi 3上肯定也可以运行，但是CPU性能好，dht22采集函数可能要修改一些东西<br>
系统是ArchlinuxArm，个人一直用的这个发行版本，官方系统没有试过，应该也能跑<br>

## 3.编译
确保WiringPi已经正确编译安装<br>
```
git clone https://github.com/xiaoloudongfeng/raspberrypi.git
cd raspberrypi
make
./chlcd.sh lcd
```
如果配合systemd目录中的lcd12864.service脚本，可以使用systemctl命令控制程序，当然脚本里的路径可能要修改一下<br>
启动：<br>
```
systemctl start lcd12864
```
停止：<br>
```
systemctl stop lcd12864
```
开机启动：<br>
```
systemctl enable lcd12864
```
