# jupyterlab 全副武装

## 0. 写在前面

jupyter 是什么？

> jupyter 只是一个适合**科研**的 IDE。jupyter 包含 jupyter notebook 和 jupyterlab，两者的区别在于 notebook 功能简单，推荐使用 jupyterlab，更强大，更像个 IDE。
>
> jupyterlab 本身其实很难用，是各种各样的**插件使其强大起来**。因此，本文的目的在于搭建一个能跟 PyCharm 之类的桌面 IDE 媲美的IDE。

这里触及到一个问题：**为什么不直接使用 IDE？**

> 我觉得是因为我们科研的代码往往**要在服务器上运行**，而桌面 IDE 不好用。
>
> 先在桌面 IDE 写好再**拷贝**到服务器运行很麻烦，若想在服务器 **debug** 某段代码这过程更加困难。相比之下，jupyter 的模式**更加适合科研的操作逻辑**，比如**笔记**的模式、**代码块**的形式和**内核**的概念。当然如果你觉得这些麻烦无关痛痒、这些好处不值一提，那你就直接使用桌面的就好。

搭建思路

> 1. 专门为 jupyterlab 创建一个 conda 虚拟环境。不放在 base 环境，因为可能污染 base 环境，另外，也不希望跟别人共用。
> 2. 安装 jupyterlab
> 3. 安装 debug 插件
> 4. 安装 lsp 插件。有代码补全、go to definition 等
> 5. 安装 valiableInspector 插件。可监测变量的值
> 6. 安装 sysytem monitor 插件。可监测服务器 memory 使用量和 cpu 使用量
> 7. 安装 主题。原来主题太丑

**注意：特别注意 jupyterlab 的版本，安装插件时根据版本来，很重要！**

## 1. 创建并激活 conda 虚拟环境

```bash
# 创建环境
conda create -n axiao python=3.7
# 激活环境
source activate axiao
```

## 2. 安装 jupyterlab

值得一提

> 无论时软件也好、框架也好，**版本日志必须看**，一般在官网能找到，要么就是在文档里，重要时挑大版本来看。即像这种 2.x -> 3.x

官网地址

> [Project Jupyter | Home](https://jupyter.org/)，安装步骤以官网的为准。

看版本日志

> [JupyterLab Changelog — JupyterLab 3.1.0a13 documentation](https://jupyterlab.readthedocs.io/en/latest/getting_started/changelog.html)。可以看到，**最新的 3.x，是不需要安装 nodejs 来支持的。**

### 2.1 安装

```bash
# pip 镜像下载安装
pip install jupyterlab -i https://pypi.tuna.tsinghua.edu.cn/simple
```

### 2.2 配置

### 2.2.1 设置密码

```bash
# 启动 ipython
ipython
# 编程
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter passwd:				# 这里输入你的密码
Verify passwd:				# 重复输入密码
Out[2]:'argon2:$argon2id$v=19$m=10240,t=10,p=8$G6F6NOqcKQ15Ij1rk0kv1A$ln0h9NpDi0s34rfDG8J5Ug' 	# 后面配置文件要用到
In [3]: exit()	# 退出
```

### 2.2.2 编辑配置文件

```bash
# 查看配置文件在哪
jupyter --paths
```

配置文件在 config 下，优先度由上往下

```
config:
    /home/szu/.jupyter
    /home/szu/anaconda3/envs/axiao/etc/jupyter
    /usr/local/etc/jupyter
    /etc/jupyter
data:
    /home/szu/.local/share/jupyter
    /home/szu/anaconda3/envs/axiao/share/jupyter
    /usr/local/share/jupyter
    /usr/share/jupyter
runtime:
    /home/szu/.local/share/jupyter/runtimeconfig:
    /home/szu/.jupyter
    /home/szu/anaconda3/envs/axiao/etc/jupyter
    /usr/local/etc/jupyter
    /etc/jupyter
```

去 /home/szu/.jupyter 创建 jupyter_server_config.json 文件。保存后 `jupyter lab` 启动，稍等即可自动弹出火狐浏览器打开了。

```json
{
  "ServerApp": {
    "jpserver_extensions": {
      "jupyterlab": true
    },
	"port": 8198,
	"password": "argon2:$argon2id$v=19$m=10240,t=10,p=8$G6F6NOqcKQ15Ij1rk0kv1A$ln0h9NpDi0s34rfDG8J5Ug",
  }
}
```

*tips: 想看还有哪些配置项？`jupyter lab --generate-config`生成配置文件，有注释说明。*

## 3. debugger

官网：[jupyterlab/debugger: A visual debugger for Jupyter notebooks, consoles, and source files (github.com)](https://github.com/jupyterlab/debugger)

```bash
# pip 镜像下载安装 xeus-python 
pip install xeus-python -i https://pypi.tuna.tsinghua.edu.cn/simple
# 安装完启动看看
```

## 4. 代码补全lsp

官网：[krassowski/jupyterlab-lsp: Coding assistance for JupyterLab (code navigation + hover suggestions + linters + autocompletion + rename) using Language Server Protocol (github.com)](https://github.com/krassowski/jupyterlab-lsp)

```bash
# pip 镜像下载安装 jupyterlab-lsp 
pip install jupyterlab-lsp -i https://pypi.tuna.tsinghua.edu.cn/simple
# install LSP servers
conda install -c conda-forge python-lsp-server
# 安装完启动看看
```

## 5. 变量监测

官网：[lckr/jupyterlab-variableInspector: Variable Inspector extension for Jupyterlab (github.com)](https://github.com/lckr/jupyterlab-variableInspector)

```bash
pip install lckr-jupyterlab-variableinspector -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## 6. 主题

这个是 idea 的！https://github.com/telamonian/theme-darcula

```bash
pip install theme-darcula -i https://pypi.tuna.tsinghua.edu.cn/simple
```

