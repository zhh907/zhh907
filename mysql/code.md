# 用户登录
```
mysql -u root -p
```

# 用户赋权
```
grant all privileges on *.* to 'test'@'%' identified by '123456' with grant option;
# all privileges：表示将所有权限授予给用户。也可指定具体的权限，如：SELECT、CREATE、DROP等。
# on：表示这些权限对哪些数据库和表生效，格式：数据库名.表名，这里写“*”表示所有数据库，所有表。如果我要指定将权限应用到test库的user表中，可以这么写：test.user
# to：将权限授予哪个用户。格式：”用户名”@”登录IP或域名”。%表示没有限制，在任何主机都可以登录。比如：”yangxin”@”192.168.0.%”，表示yangxin这个用户只能在192.168.0IP段登录
# identified by：指定用户的登录密码


# 刷新权限
flush privileges;

# 查看用户权限
grant select,create,drop,update,alter on *.* to 'test'@'localhost' identified by '123456' with grant option;

# 回收权限
revoke create on *.* from 'test@localhost';
flush privileges;

# 删除用户
select host,user from user;
drop user 'yangxin'@'localhost';

# 用户重命名
rename user 'test'@'%' to 'test1'@'%';

# 修改密码
use mysql;
# mysql5.7之前
update user set password=password('123456') where user='root';
# mysql5.7之后
update user set authentication_string=password('123456') where user='root';

```


