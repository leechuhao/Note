C#远程连接DB2数据库，需要在本机上安装DB2（据说需要和服务器版本一致），否则无法连接成功。

连接字符串如下：

```
DB2Connection cn = new DB2Connection(@"
DATABASE=hisuptes;
SERVER=10.11.10.221:50168;
User ID=neusoft;
Password=neusoft;");
```

从Access数据库中查询数据，在Access数据库中的 null值，读取到对象中是空字符串，而DB2的数字和日期等类型不接受空字符串，因此会报错。

解决方法：

```
String Long_Time = @"yyyy-MM-dd HH:mm:ss";
//拼接字符串要注意不要带单引号不然会变成 'null'字符串
if (downs.Birth == "")
{
	downs.Birth = "null";
}
else
{
	downs.Birth = "'" + Convert.ToDateTime(downs.Birth).ToString(Long_Time) + "'";
}
```

