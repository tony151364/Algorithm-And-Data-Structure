## 一、基本命令　

### 1.安装服务

```
mysqld --install
```	

### 2.初始化

```
mysqld --initialize --console	
```

### 3.开启服务与关闭服务

```
net start mysql
```

```
net stop mysql
```

### 5.登录与退出mysql：

```
 隐藏密码：mysql -u root -p
 显示密码：mysql -uroot -proot
```

```
exit;
```
	
### 6.修改密码
```
alter user 'root'@'localhost' identified by 'root';(by 接着的是密码)
```

### 7.标记删除mysql服务：
```
sc delete mysql;
```

### 8.查看、使用、创建、删除数据库：
```
show databases;
```
```
use student;
会显示：Database changed;
```
```
create database student;
```
```
drop database student;
```
### 9.选中后数据库后查看表
```
show tables;
```

### 10.导入表
```
source D:\ProgramData\SQLProject\bjpowernode.sql;
1.需要先选中数据库
2.路径不能有中文
```

### 11. 不看数据，只看表的结构
```
desc 表名; 等价于 descript student;
```

### 12.查看当前MySQL版本号
```
select version();
```
查看当前所使用的数据库
```
select database();
```

### 13.终止输入：  
```
mysql> show  
->  
->  
->  
->  
->  
-> \c  
mysql>  
```
