# Ubuntu安装Monaco字体
先从github上下载:

	git clone https://github.com/cstrap/monaco-font.git

然后解压,跳到解压的目录,发现有一个Install_ubuntu.sh什么的,直接运行,但是因为默认的下载主题路径是googlecode,已经被封了,所以要在运行后面加上参数,具体命令为:

	sudo ./install-font-ubuntu.sh https://github.com/todylu/monaco.ttf/blob/master/monaco.ttf\\?raw\\=true