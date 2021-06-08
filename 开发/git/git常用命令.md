# 一、分支

## 1. 查看分支

```javascript
# 查看本地分支
git branch
# 查看远程分支
git branch -r
# 查看所有分支
git branch -a
```

## 2. 操作分支

```javascript
# 创建分支
git branch 分支名
# 切换分支，本地没有远程有，则本地创建同名分支并切换过去
git checkout 分支名
# 创建并切换分支
git checkout -b 分支名
# 删除分支
git branch -d 分支名
# 合并分支
git merge 分支名
```

## 3. 分支改名

```javascript
# 本地分支改名
git branch -m 老名字 新名字
# 将重命名后的分支推送到远程
git push origin 新分支名
# 删除远程的旧分支
git push --delete origin 老分支名
```

显示如下说明成功

```javascript
To http://11.11.11.11/demo/demo.git
 - [deleted]           oleName
```

## 4. 删除远程分支

```javascript
# 删除远程分支，删除后还需推送到服务器
git branch -d -r 分支名
# 删除后推送至服务器
git push origin:分支名
#######或者
# 删除远程的旧分支
git push --delete 远程仓库名 老分支名
```



# 二、 远程相关

```javascript
# 查看远程仓库
git remote
# 查看远程仓库详细地址信息
git remote -v
# 添加远程仓库
git remote add 远程仓库命名 远程仓库地址
# 将远程仓库的更新，全部取回本地
git fetch 远程仓库名
# 将远程仓库的特定分支取回本地
git fetch 远程仓库名 分支名
# 将远程仓库的更新，全部取回本地，合并 pull = fecth + merge
git pull
```



# 三、操作流程

```javascript
# 添加
git add .
# 提交
git commit
# 拉取
git pull
# 解决冲突
# 推送
git push
```



# 四、config

```javascript
# 配置指令
git config
# 查看系统配置
git config --system --list
# 查看当前用户的 global 配置
git config --global --list
# 查看当前仓库的配置
git config --local --list
## 覆盖顺序 local > global > system

#设置
git config --global 配置项 配置值
```



# 五、版本退回

```git
# 查看 commit 日志
git log
# 查看所有操作对应的id
git reflog
# 退到/进到 指定commit的sha码 commit_id:239afed0857cc2e77c17c01014077808619af64d
git reset --hard commit_id
```



# ...、参数

```javascript
# 以下首字母不区分大小写
# 删除
-d
--delete
# 强制
-f
--force
# 移动或重命名
-m
--move
# 远程
-r
--remote
# 所有
-a
--all
```



