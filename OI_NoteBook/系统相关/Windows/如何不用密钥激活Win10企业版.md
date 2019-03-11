# 如何不用密钥激活Win10企业版

**仅供学习！！！**

其实win10企业版内的iso文件自带了一个激活密钥，所以其实用这个密钥就可以了，不需要输入别的密钥

首先打开CMD，Windows+X 然后进入CMD（管理员），或者PowerShell（管理员）

输入以下命令
```
slmgr /skms kms.digiboy.ir
slmgr /ato
```

---

[原文](https://blog.csdn.net/chaoyu168/article/details/79241506)

>win10 2016 长期服务版的ISO文件中本身就带有KMS激活KEY，不用输入任何KEY，连接网络进入CMD，只要输入：
slmgr /skms kms.digiboy.ir
slmgr /ato
这两条命令，就可以KMS激活。