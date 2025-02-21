---
title: Python沙箱逃逸
date: 2024-09-27 14:19:49
categories: CTF
tags: [Pwn]
---

Python魔术方法和对象

```
__builtins__
__file__
get、set、del(attr,item)
__base__
__subclasses__()
_ Dos交互模式下重新输出先前输出
——import__('os')
impoet sys
sys.modules['os']='D:\softwares\Linuxsoft\Python-3.12.0\Lib\os.py'
```

```
>>> execfile('/home/paulc/miniconda3/envs/py27/lib/python2.7/os.py')

#python2
exec
```

