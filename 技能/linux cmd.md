# 1. 文件相关

## mv a b

重命名

```bash
mv /home/sur/pic/cat1.jpg /home/sur/pic/cat007.jpg
```

移动文件夹

```bash
mv /home/sur/pic /home/sur/pic2022
```

移动文件

```bash
# 单个文件。注：若目标目录有该文件，则覆盖
# 移动到cat目录下
mv /home/sur/pic/cat1.jpg /home/sur/pic/cat
# 移动的cat目录下并重命名
mv /home/sur/pic/cat1.jpg /home/sur/pic/cat/cat007.jpg

# 多个文件
mv /home/sur/pic/* /home/sur/pic/cat
```

总结

> a是源路径，b是目的路径，无论其是文件还是目录，mv操作的本质是将a路径的相关内容用b路径来引用。
> 
> a若是文件夹，则移动到该文件夹之下；b若是文件名，则移动到该位置并改名。
