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


# 其他

## 1. 把 jupyter notebook 放在后台运行

1. 启动

    ```bash
# 运行 jupyter notebook 查看运行在哪个端口
    jupyter notebook
    # ctrl c 杀掉当前的 jupyter notebook
    # 把 jupyter notebook 放到后台
    nohup jupyter notebook &
    # 浏览器 vy-desktop:端口号 启动网页 jupyter
    ```

2. 关闭

    ```bash
    # 查看 PID
    ## 若 shell 害活着
    jobs -l
    ## 若 shell 死了，根据时间去筛选
    ps ux | grep jupyter notebook
    ```

    

