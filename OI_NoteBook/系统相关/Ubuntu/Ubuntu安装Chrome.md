# Ubuntu安装Chrome

---

通过一下这条命令下载Chrome的Deb包

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

然后安装

```
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

如果出现依赖问题，输入一下命令后重装

```
sudo apt-get install -f
```