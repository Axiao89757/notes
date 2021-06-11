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

   