# 你 git clone 竟然只有 3 kib/s ?

你是否早就烦透g了GitHub？网站访问速度跟蜗牛在那爬一样

更烦的是，本地 git 仓库 clone pull push，无限 443 timeout 轰炸，各种 connect reset、connect reject....

**今天，你有幸看到这篇文章，你的烦恼将成为过去！**

## 1. 首先，你得翻墙

首先，我想说：

> “翻墙是一个求学者的专业素养”

然后就是方法：

> 最简单、最无脑、最稳定、最高效、也是最推荐的方式是：**买！**
>
> 别跟我说什么修改host、浏览器插件翻墙，那是吃抱着撑着爱折腾的人的饭后活动，不稳定、速度慢、还得是不是去配置，烦死了，每个月几块钱的事，整得那么复杂干嘛？我现在也无法理解以前折腾那些东西干嘛，一顿饭少点一个菜的事...

也得有方法的买：

> 受人以鱼，不如受人以渔。告诉你个关键字，会百度的人都能自己搞定了。
>
> 关键字：**v2ray ** **二爷** **翻墙**
>
> 没那么神奇，**无非就是下载 v2ray 软件，然后去购买订阅链接来配置**就好，订阅链接的购买仁者见仁，自己找去，最好就是三四个人合租一个账号，**买一年一百块来块，平均下来也就是几块钱一个月，**我用的平民价格，还挺稳定，需要的私信推荐

方法很多，最简单、最无脑、最稳定、也是最推荐的是：**买！**。没错，就是买。什么修改host、谷歌浏览器插件等，都是瞎胡闹，每个月花几块钱的事，为啥想不开去折腾，时断时续、速度慢就算了，还得shi'b

## 2. Github 网站访问畅通无阻

现在，默认你能顺利访问墙外的世界了，当然访问 Github 速度不成问题了。那为什么还有这一章呢？嗯，我有 tips 分享。

二爷 一直开着全局，流量哗哗哗就没了！开着 pac，Github 又跟没翻墙一样。“那可以先开着 pac，等需要访问 Github 再开全局嘛！”，是的你可以这样，但很傻，很麻烦，你可能还会追了几集电视剧，看了几集动漫后才发现忘了关掉刚刚要访问 Github 而打开的二爷全局，这时候你只能咬牙切齿接着骂一句：“fuck!”

我废话怎么那么多。。。

**怎么办？**

> 二爷软件 -> 参数设置 -> 用户PAC设置 -> 输入以下内容保存
>
> ```
> ||github.com^,
> ||git.hust.cc^
> ```
>
> 然后一直开着 PAC 就好了

## 3. 以为这就好了？

现在只是 Github 网站访问得比较快而已，本地 git 访问还得配置

[参考这个]: https://ericclose.github.io/git-proxy-config.html

### 3.1 首先得有个概念

1. 代理有两种：http 和 socks5
2. 协议有两类：ssh 和 https

### 3.2 具体步骤

1. 查看你的代理**端口**

   在二爷软件低端可以看到，我的是 10808

2. **https 协议下的代理配置**

   可以用 **git 命令**

   ```git
   # 使用 socks5 代理
   git config --global http.proxy 'socks5://127.0.0.1:10808'
   git config --global https.proxy 'socks5://127.0.0.1:10808'
   
   # 使用 http 代理
   git config --global http.proxy 'http://127.0.0.1:10809'
   git config --global https.proxy 'http://127.0.0.1:10809'
   
   # 查看 config
   git config --global -l
   ```

   

   也可以编辑用户目录下的 **config 文件**

   ```
   [http "https://github.com"]
   	proxy = socks5://127.0.0.1:10808
   [https "https://github.com"]
   	proxy = socks5://127.0.0.1:10808
   ```

   ```
   [http]
   	proxy = http://127.0.0.1:10809
   [https]
   	proxy = http://127.0.0.1:10809
   ```

   

3. **ssh 协议下的代理配置**

   我个人一直倾向使用用 ssh 协议，因为他**快！** 两者的区别可参考：

   [ssh 协议 vs https 协议]: https://blog.cuiyongjian.com/engineering/git-https-ssh/

   简单来说，https 可匿名，但是得登录麻烦，速度也相对较慢；而 ssh 快一些，但是不能匿名。

   废话少说，ssh 协议下配置代理，一样有 http 和 socks5。**在 ~/.ssh/ 下创建 config 文件**，输入下面配置就好。

   **http:**

   ```
   Host github.com
       User git
       ProxyCommand connect -H 127.0.0.1:10809 %h %p
   ```

   **socks5:**

   ```
   Host github.com
       User git
       ProxyCommand connect -S 127.0.0.1:10808 %h %p
   ```

   **解释：**

   >- `Host` 后面 接的 `github.com` 是指定要走代理的仓库域名。
   >- 在 ProxyCommand 中，Windows 用户用的是 `connect`。
   >- 单独的 `-S` 选项指的就是 socks5 代理，单独的 `-H` 选项指的就是 http 代理。
   >- 在调用 ProxyCommand 时，`％h` 和 `％p` 将会被自动替换为**目标主机名**和 **SSH 命令指定的端口**（ `%h` 和 `%p` 不要修改，保留原样即可）。