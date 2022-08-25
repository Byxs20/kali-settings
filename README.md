# 0.更改源更新apt

**更改源：**[戳我然后更改](https://developer.aliyun.com/mirror/kali)

**更新软件：**

```shell
sudo apt-get update && sudo apt-get upgrade
```

上面的命令需要一些时间的，这个根据个人网速。

<br>

# 1.安装编译依赖

```shell
sudo apt-get -y build-dep python3 && sudo apt-get install -y pkg-config
```

```shell
sudo apt-get install -y build-essential gdb lcov pkg-config \
      libbz2-dev libffi-dev libgdbm-dev libgdbm-compat-dev liblzma-dev \
      libncurses5-dev libreadline6-dev libsqlite3-dev libssl-dev \
      lzma lzma-dev tk-dev uuid-dev zlib1g-dev
```

参考链接：https://devguide.python.org/getting-started/setup-building/#linux

<br>

# 2.下载解压源码

```shell
cd /usr/local && mkdir Python3 && cd Python3 && wget https://www.python.org/ftp/python/3.8.8/Python-3.8.8.tgz &&  tar -xzf Python-3.8.8.tgz && cd Python-3.8.8/
```

<br>

# 3.编译及安装

```shell
./configure -with-ssl prefix=/usr/local/Python3/ && make && make install
```

<br>

# 4.软连接pip3 和python3.8

```shell
mv /usr/bin/python3 /usr/bin/python3.bak && ln -s /usr/local/Python3/bin/python3.8 /usr/bin/python3
```

```shell
mv /usr/bin/pip3 /usr/bin/pip3.bak && ln -s /usr/local/Python3/bin/pip3 /usr/bin/pip3
```

<br>

# 5.添加环境变量

```shell
vim ~/.zshrc
```

然后在文件末尾添加

`export PATH=/usr/local/Python3/bin:$PATH`

按ESC，输入:wq回车退出。

```
source ~/.zshrc
```

<br>

# 6.生成pip和库

```shell
pip3 install --upgrade pip && pip3 install --upgrade setuptools
```

<br>

# 报错解决：

**常见报错1：**

当使用pip的时候，就会出现：

`subprocess.CalledProcessError: Command '('lsb_release', '-a')' returned non-zero exit status 1.`

<br>

注意：是`/usr/local/Python3/lib/python3.8`路径下缺少lsb_release.py文件（具体报错路径查看自己电脑报错路径），解决方法

```shell
sudo find / -name 'lsb_release.py
```

```shell
# 这是我的路径，情况不同，自己注意，如果您一直按照我的步骤来做的话，那就可以复制下面的命令即可！
cp /usr/lib/python3/dist-packages/lsb_release.py /usr/local/Python3/lib/python3.8
```

<br>

**常见报错2：**

当使用终端时候：

`ModuleNotFoundError: No module named 'apt_pkg'`

<br>

```shell
sudo apt-get install --reinstall python3-apt
cd /usr/lib/python3/dist-packages/ && sudo cp apt_pkg.cpython-310-x86_64-linux-gnu.so apt_pkg.cpython-38-x86_64-linux-gnu.so
```

<br>

# 番外篇：

kali python2没有pip的问题

```shell
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py && sudo python2 get-pip.py && rm get-pip.py
```

升级pip和库：

```shell
pip2 install --upgrade pip && pip2 install --upgrade setuptools
```