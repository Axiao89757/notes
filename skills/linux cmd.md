# 文件相关

## 1. mv a b

**总结**

a是源路径，b是目标路径，本质是将b路径指向a的位置。

b若是文件夹，则移动到该文件夹之下；b若是文件则移动并改名，重名则覆盖

**使用**

```bash
# 移动单个文件
mv /home/usr/dog/d1.jpg /home/usr/dog2022
# 移动单个文件并重命名
mv /home/usr/dog/d1.jpg /home/usr/dog2022/dog1.jpg
# 移动多个文件
mv /home/usr/dog/* /home/usr/dog2022
# 移动文件夹
mv /home/usr/dog /home/usr/dog2022
```

## 2. rm

```bash
# 删除文件夹
rm -rf /home/usr/dog/d1.jpg
# 删除文件
rm -f /home/usr/dog
```

## 3. 解压缩

```bash
# .zip文件
unzip filename.zip
# .tar.gz文件 z:gzip x:extract v:verbose f:file
tar -zxvf filename.tar.gz
# .tar.bz2文件 j:bzip2
tar -jxvf filename.tar.bz2
# .tar.xz文件
tar -Jxvf filename.tar.xz
```

## 4. 统计

### 4.1 文件个数

```bash
ls -l | grep ^- | wc -l
```

### 4.2 磁盘空间统计

df

```bash
# 查看磁盘剩余空间
df -hl
# 查看每个根路径的分区大小
df -h：
# 返回该目录的大小
du -sh [目录名]
# 返回该文件夹总M数
du -sm [文件夹]
# 查看指定文件夹下的所有文件大小（包含子文件夹）
du -h [目录名]
```

du：disk usage

```bash
du -sh
du log2012.log 
du -h test
```

> -s：对每个Names参数只给出占用的数据块总数。
> -a：递归地显示指定目录中各文件及子目录中各文件占用的数据块数。若既不指定-s，也不指定-a，则只显示Names中的每一个目录及其中的各子目录所占的磁盘块数。
> -b：以字节为单位列出磁盘空间使用情况（系统默认以k字节为单位）。
> -k：以1024字节为单位列出磁盘空间使用情况。
> -c：最后再加上一个总计（系统默认设置）。
> -l：计算所有的文件大小，对硬链接文件，则计算多次。
> -x：跳过在不同文件系统上的目录不予统计。
> -h：以K，M，G为单位，提高信息的可读性。

# 便捷操作

## 1. 路径栈

**总结**

解决cd命令的时候总是得写全部的路径名很繁琐的问题。

[Linux 目录栈及目录切换 - lbnnbs - 博客园 (cnblogs.com)](https://www.cnblogs.com/lbnnbs/p/5883788.html)

**使用**

```bash
# 列出全部
dirs -v
# 访问具体某一个
dirs -/+ n

# 入栈
pushd /home/usr/dog
# 切换到具体某个路径
pushd -/+ n

# 删除
popd -/+ n # 删除dirs中倒数第一个
```

**技巧**

使用`pushd 路径`将经常访问路径的推入栈，使用`pushd -/+ n`来切换当前目录。

## 2. 脚本文件

创建 .sh 文件，即可通过`sh filename.sh`命令来执行脚本。
