在写windows服务中遇到的几个问题

1. ### 权限不足，使用管理员权限

```
在“安装”阶段发生异常。
System.Security.SecurityException: 未找到源，但未能搜索某些或全部事件日志。
```

2. ### 路径问题

路径不能带空格

3. ### 安装脚本

```
%SystemRoot%\Microsoft.NET\Framework\v4.0.30319\installutil.exe %~dp0\WindowsService1.exe
net start WindowsService1
sc config WindowsService1 start=auto
echo ---------------------------------------------------
echo success to install service!
pause
```

4. ### 卸载脚本

```
%SystemRoot%\Microsoft.NET\Framework\v4.0.30319\installutil.exe %~dp0\WindowsService1.exe -u
sc delete WindowsService1
Net Stop WindowsService1
echo ---------------------------------------------------
echo 卸载服务成功！
pause
```

