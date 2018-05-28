---
title: 解决pip3的cannot import name main问题
date: 2018-05-28 11:56:59
tags: python
---

今天试用了win10的linux子系统，当配置python3环境时，升级pip3遇到了'cannot import name main'问题

```bash
Traceback (most recent call last):
  File "/usr/bin/pip3", line 9, in <module>
    from pip import main
ImportError: cannot import name main
```

解决方法是修改文件：
```bash
vim /usr/bin/pip3
```

原内容是：
```python
from pip import main
sys.exit(main())
```

修改为：
```python
from pip import __main__
if __name__ == '__main__':
    sys.exit(__main__._main())
```

保存后就可用了。

以上参考：[https://qiujunya.com/pip3mainproblem.html](https://qiujunya.com/pip3mainproblem.html)。