[toc]

# push 失败

## 1. Connection reset by peer

```bash
PS E:\Projects\frontend\todolist-plus> git push origin master
Enumerating objects: 36, done.
Counting objects: 100% (36/36), done.
Delta compression using up to 12 threads
Writing objects: 100% (36/36), 596.62 KiB | 957.00 KiB/s, done.
Total 36 (delta 4), reused 0 (delta 0), pack-reused 0
client_loop: send disconnect: Connection reset by peer
send-pack: unexpected disconnect while reading sideband packet
```

```bash
PS E:\Projects\frontend\gsb_business_web> git pull
kex_exchange_identification: read: Connection reset by peer
Connection reset by 13.229.188.59 port 22
fatal: Could not read from remote repository.      

Please make sure you have the correct access rights
and the repository exists.
```

**解决方法：**再push一次，应该时网络原因

## 2. error: failed to push some refs to ’...‘

```bash
PS E:\Projects\frontend\gsb_business_web> git push
To github.com:n303b/gsb_business_web.git
 ! [rejected]        dev1 -> dev1 (non-fast-forward)
error: failed to push some refs to 'github.com:n303b/gsb_business_web.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

**原因：**本地commit上的版本落后于远程的，被拒了

**解决方法：**先pull下来merge，有冲突解决冲突，然后再push



# pull 失败

## 1. here is no tracking information for the current branch.

```bash
PS E:\Projects\frontend\gsb_data_web> git pull 
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> liuwenjun
```

**原因：**没有指定远程哪个分支跟本地哪个分支合并

**解决方法：**

1. 明确指定要合并的分支

   ```bash
   git pull 远程仓库名 远程分支名
   ```

2. 先设置本地分支upstream到远程要合并的分支，再pull

   ```bash
   git branch --set-upstream-to=远程仓库名/远程分支名 master
   git pull
   ```


# add 失败

## 1. warning: LF will be replaced by CRLF

```bash
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in src/components/content/patrolManagement/patrolTeam/PatrolTeamQuery.vue.
```

**原因：**windows中的换行符为 CRLF， 而在Linux下的换行符为LF，所以在执行add . 时出现提示

**解决方法：**配置git，false就是不转换符号，默认是true

```bash
git config --global core.autocrlf false
```

