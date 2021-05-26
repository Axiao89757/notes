# 1. 新建数据库

创建一个新的数据库，同时创建一个有全部权限的用户。**达到一个数据库一个管理员用户的目的。**

1. 先看看有哪些数据库

```mysql
show databases;
```

2. 再看看有哪些用户

```mysql
select User,Host from mysql.user;
```

## 1.1 创建一个用户

```mysql
create user 用户名 identified by '密码';
```

这个用户的host将是%，也就是所有的host都能使用这个用户来登录数据库server

## 1.2 创建一个数据库

```mysql
create database 数据库名;
```

## 1.3 授权

```mysql
grant all on 数据库名.* to '用户名'@'%';
```

## 1.4 ...

删除用户

```mysql
DROP USER '用户名'@'主机host';
```

删除数据库

```mysql
DROP DATABASE '数据库名'
```

