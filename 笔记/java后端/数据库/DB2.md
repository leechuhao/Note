C#远程连接DB2数据库，需要在本机上安装DB2（据说需要和服务器版本一致），否则无法连接成功。

连接字符串如下：

```
DB2Connection cn = new DB2Connection(@"
DATABASE=hisuptes;
SERVER=10.11.10.221:50168;
User ID=neusoft;
Password=neusoft;");
```

