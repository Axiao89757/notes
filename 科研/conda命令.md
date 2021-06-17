# conda命令

## 1. 环境相关

1. 查看已安装的环境

   ```bash
   conda env list
   # 或者
   conda info -e
   ```

2. 创建虚拟环境

   ```bash
   conda create -n 虚拟环境名称 python=版本
   ```

3. 删除环境

   ```BA
   conda remove -n 虚拟环境名称 --all
   ```

4. 激活虚拟环境

   ```bash
   # window
   activate 虚拟环境名称
   # ubuntu
   source activate 虚拟环境名称
   ```
   
5. 退出当前环境

   ```bash
   # 返回上个环境
   conda deactivate
   ```

   

## 2. 包相关
 1. 安装、更新包

       ```bash
       # 在当前环境安装包
       conda install scrapy
       # 在指定环境中安装包
       conda install -n 环境名 scrapy
       # 在当前环境更新包
       conda update scrapy
       # 在指定环境更新包
       conda update -n 环境名 scrapy
       # 更新当前环境的所有包
       conda update -all
       # 更新指定环境的所有包
       conda update -n 环境名 --all
       # 在当前环境中删除包
       conda remove scrapy
       # 在指定环境中删除包
       conda remove -n 环境名 scrapy
       ```
       
 2. 查看包

       ```bash
       # 查看当前环境已安装包
       conda list
       # 查看指定环境已安装包
       conda list -n 环境名
       ```

       ## 3. 镜像

       参考 [Python- 解决PIP下载安装速度慢_wukai0909的博客-CSDN博客_pip 下载太慢](https://blog.csdn.net/wukai0909/article/details/62427437)

       1. 临时使用

          ```bash
          # 临时使用 https://pypi.tuna.tsinghua.edu.cn/simple 镜像安装 pyspider
          pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pyspider
          ```
       
       **一些镜像**

> 1. 清华：https://pypi.tuna.tsinghua.edu.cn/simple
> 2. 阿里云：http://mirrors.aliyun.com/pypi/simple/
> 3. 中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
> 4. 华中理工大学：http://pypi.hustunique.com/
> 5. 山东理工大学：http://pypi.sdutlinux.org/ 
> 6. 豆瓣：http://pypi.douban.com/simple/


# 其他

## 1. 把 jupyter notebook 放在后台运行

1. 启动
    ```bash
    # 运行 jupyter notebook 查看运行在哪个端口
    jupyter notebook
    # ctrl c 杀掉当前的 jupyter notebook
    # 1把 jupyter notebook 放到后台
    nohup jupyter notebook &
    # 2把 jupyter notebook 放到后台，有输出写入到文件
    nohup jupyter notebook --allow-root > jupyter.log 2>&1 &
    # 浏览器 vy-desktop:端口号 启动网页 jupyter
    ```
    
2. 关闭

    ```bash
    # 查看 PID
    ## 若 shell 还活着
    jobs -l
    ## 若 shell 死了，根据时间去筛选
    ps ux | grep jupyter notebook
    ```

