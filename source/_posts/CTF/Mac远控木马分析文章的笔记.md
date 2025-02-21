---
title: 暗蚊黑产
date: 2024-01-21 08:19:49
categories: CTF
tags: [MAC]
---

>https://mp.weixin.qq.com/s/EhRX26TP9rxEKys_MzDYvQ

  **暗蚊黑产团伙采用供应链投毒、伪造官方软件网站、以及利用破解软件等多种方式传播恶意程序，主要攻击目标是IT运维人员，攻击范围涵盖了Windows、Linux以及macOS等操作系统。** 攻击水平不高，但危害很大。

## 攻击流程 

1.注册域名，投放木马，测试免杀。

2.被下载嵌入后门的运维工具-->下载远控木马-->窃取文件和主机信息

 置于匿名文件共享服务托管平台oshi.at 

3.-->下载和使用fscan、nmap内网扫描-->弱口令+漏洞等横向渗透

4.--> 部署并运行hellobot后门 

## 特点

域名精心构造

| **文件名**                                           | **恶意域名**              |
| ---------------------------------------------------- | ------------------------- |
| SecureCRT.dmg                                        | download.securecrt.vip    |
| ultraedit.dmg                                        | download.ultraedit.info   |
| Microsoft-Remote-Desktop-Beta-10.8.0(2029)_MacYY.dmg | download. rdesktophub.com |
| FinalShell_MacYY.dmg                                 | download.finallshell.cc   |
| navicat161_premium_cs.dmg                            | download.Macnavicat.com   |

2.域名注册时间不超过1年。攻击者 在3月至7月期间攻击者陆续注册了此次攻击活动中所涉及的10个C2域名 

3.载荷有上传至VT测试免杀效果。可以作为一个怀疑指标。

4.解密载荷上传至VT时间距今不超过3个月。

5.载荷的自动下载分为2个阶段，对应的url不同，可以防止被分析到落地载荷。

被植入恶意文件为libpng.dylib，下载的恶意文件命名为fs.log，centos7,libdb.so.2,/tem/.test，/Users/Shared/.fseventsd。

6.  使用**crond服务持久化**和**后门动态链接库**手法有特点 。centos7的文件，该文件运行后会在当前路径下释放一个名为libdb.so.2的文件，利用libdb.so.2文件对crond服务动态库文件进行劫持，然后将libdb.so.2文件及crond的时间属性值修改为/bin/ls的时间，最后重新启动crond服务，加载执行恶意文件libdb.so.2。经分析发现libdb.so.2文件为hellobot后门 

7. 远控木马基于开源跨平台KhepriC2框架修改，内网横向移动阶段的木马 基于开源跨平台工具goncat进行修改；
8. 后门动态库落地是通过样本自己读取自身内容，并根据“ELFELF”找到标记的偏移位置。 

## **crond服务持久化**和**后门动态链接库**手法

```
#将crond复制到当前路径
cp /usr/sbin/crond . 
#查看crond的依赖项里是否有libdb.so.2
#使用fgrep而非grep,把所有字符解释为字面含义，以避免.被解释为正则。
ldd ./crond |fgrep libdb.so.2  
#获取软链接对应的绝对路径，再获取目录路径
$P=dirname `ldd /bin/ls|grep libd1|awk '{print $3}'`;
echo $P;#获取crondb依赖项原所在路径
mv libdb.so.2 $P;/bin/ls
#修改/lib/x86_64-linux-gnu/libdb.so.2的时间属性为/bin/ls的时间
  -r, --reference=FILE   
#use this file's times instead of current time
touch -r /bin/ls $P/libdb.so.2;
#将被劫持的cornd移动会/usr/sbin，修改crond的时间属性

mv crond /usr/sbin;
#增强隐蔽性，因为各种系统程序的修改时间应该一致
touch -r /bin/ls crond;
#重启crond服务
service crond restart
```



 Linux touch命令用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件 