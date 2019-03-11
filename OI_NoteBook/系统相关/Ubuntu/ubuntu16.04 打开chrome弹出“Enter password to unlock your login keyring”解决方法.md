# ubuntu16.04 打开chrome弹出“Enter password to unlock your login keyring”解决方法

---

转载：[Blog](https://blog.csdn.net/u013485792/article/details/83140858)

首先输入命令

```
find ~/ -name login.keyring
```

然后把查找出来的东西都删掉

应该就会显示输入新的密码

输入新的密码后应该就不会再次弹出了