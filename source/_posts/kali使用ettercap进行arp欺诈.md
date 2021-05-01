---
title: kali使用ettercap进行arp欺诈和dns欺诈
date: 2021-03-23 15:33:41
tags: kali 网络
categories: 网络安全
---
本文主要讲一下怎么使用Kali linux中的ettercap在内网中进行arp欺诈和dns欺诈
<br/>
<!--more-->
***
以下为步骤:
<br/><b>第一步</b><br/>首先准备一个kali linux虚拟机和另外一台虚拟机做靶机，进行设置，将两台虚拟机的网络适配器设置为NAT模式
![](example1.png)
***
<br/><b>第二步</b><br/>首先打开kali linux,通过Ctrl+alt+T打开终端，输入以下代码
``` linux
ifconfig
```
来查看kali linux的ip地址
比如我这里显示的为192.168.190.xxx
<br/>之后在终端输入如下命令
```
sudo mousepad /etc/ettercap/etter.dns
```
<br/>打开了etter.dns文件后,在这个文件的最底端增加两条
```
* A <Your Address>
* PTR <Your Address>
```
如图所示
![](example2.png)
<br/>之后新建一个html文件，随便写点什么，命名为index.html，把它放入
```
var/www/html
```
文件夹中，并替换掉原有的index.html,替换如果提示权限不够可以使用以下代码来进行替换操作
```
sudo cp <想要复制过去的文件名> /var/www/html
```
之后使用终端输入
```
/etc/init.d/apache2 start
```
来打开服务端
<br/>
<b>第三步</b><br/>启动靶机，靶机记得同样要使用<b>NAT模式</b>
***
<br/><b>第四步</b><br/>在kali linux中启动ettercap，该软件在kali linux的工具菜单中有一个GUI版本的快捷方式，所以直接在那里启用就好
![](example3.png)
设置考虑到实验性质，如上设置（通常也是默认设置）就好，设置完成后点击右上角的√按钮，进入下一个界面
![](example4.png)
设置完后，先点击![](icon1.png)暂停，之后点击![](icon2.png)来查找主机
<br/>查找完成后，点击![](icon3.png)切换到hosts list
***
<br/><b>第五步</b>
在kali linux终端输入
```
route
```
其中出现自己ip的一行中的gateway就是当前内网的网关。之后继续切回到ettercap中，在hosts list中,首先选中<b>内网网关</b>的地址，点击<b>add to target 1</b>
之后选中<b>靶机ip</b>，点击<b>add to target 2</b> 设置完后，点击![](icon4.png),选中第一个<b>ARP poisoning</b>
![](example5.png)<br/>
如此设置之后点击ok，此时arp欺诈的设置阶段已经完成,接下来进行dns欺诈设置
***
<b>第六步</b><br/>
点击![](icon5.png)
```
选择plugins->manage plugins
```
进入如下界面
![](example6.png)
```
选择dns_spoof 右键点击 activate 激活
```
激活dns_spoof后，点击左上角的![](icon6.png)开始攻击操作
***
<b>第七步</b><br/>
切换到<b>靶机</b>后使用浏览器进行上网操作，发现打开网页发现所有网址最后都会跳转访问到我们kali虚拟机的ip，显示我们刚才写的html网页的内容
![](example7.png)
<font color =red size =3>注意:如果没有跳转而是显示无法连接，可能是因为使用的是https协议，改为http即可</font>
***
本文只做实验和学习交流用,不要有任何非分之想，当然这点技术力也达不到非分之想的要求...