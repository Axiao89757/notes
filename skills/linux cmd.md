# 文件相关

## mv a b

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
