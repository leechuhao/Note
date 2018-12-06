### XMLHttpRequest 是AJAX的基础，用于在后台与服务器交换数据。

```
var xmlhttp;
if(window.XMLHttpRequest){
    //如果支持XMLHttpRequest对象
    xmlhttp = new XMLHttpRequest();
}else{
    // 创建ActiveX对象
    xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
```

### 向服务器发送请求：

- open(method, url, async)
  - method: GET 、POST
  - url
  - async:（异步）ture/false
- send(string )
  - String : 仅用POST

### 服务器响应：

- responseText：返回字符串形式的响应

```
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```

- responseXML：如果响应是XML。而且需要作为XML对象解析


```
xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i=0;i<x.length;i++){
	txt=txt + x[i].childNodes[0].nodeValue + "<br />";
}
document.getElementById("myDiv").innerHTML=txt;
```

### onreadystatechange 事件

当请求被发送到服务器时，我们需要执行一些基于响应的任务。

每当 readyState 改变时，就会触发 onreadystatechange 事件。

readyState 属性存有 XMLHttpRequest 的状态信息。

XMLHttpRequest属性：

- ontreadystatechange:函数名，每当readyState属性改变时，会调用这个函数
- readState：
  - 0：请求未初始化
  - 1：服务器连接已建立
  - 2：请求已接受
  - 3：请求处理中
  - 4：请求已完成，且响应已就绪
- status
  - 200：“OK”
  - 404：未找到页面

### AJAX与脚本

实例：

```
<html>
<head>
<script type="text/javascript">
function showHint(str)
{
var xmlhttp;
if (str.length==0)
  { 
  document.getElementById("txtHint").innerHTML="";
  return;
  }
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("txtHint").innerHTML=xmlhttp.responseText;
    }
  }
xmlhttp.open("GET","/ajax/gethint.asp?q="+str,true);
xmlhttp.send();
}
</script>
</head>
<body>

<h3>请在下面的输入框中键入字母（A - Z）：</h3>
<form action=""> 
姓氏：<input type="text" id="txt1" onkeyup="showHint(this.value)" />
</form>
<p>建议：<span id="txtHint"></span></p> 

</body>
</html>
```

### AJAX 与数据库

asp文件

```
<%
response.expires=-1
sql="SELECT * FROM CUSTOMERS WHERE CUSTOMERID="
sql=sql & "'" & request.querystring("q") & "'"

set conn=Server.CreateObject("ADODB.Connection")
conn.Provider="Microsoft.Jet.OLEDB.4.0"
conn.Open(Server.Mappath("/db/northwind.mdb"))
set rs=Server.CreateObject("ADODB.recordset")
rs.Open sql,conn

response.write("<table>")
do until rs.EOF
  for each x in rs.Fields
    response.write("<tr><td><b>" & x.name & "</b></td>")
    response.write("<td>" & x.value & "</td></tr>")
  next
  rs.MoveNext
loop
response.write("</table>")
%>
```

### AJAX 与XML

```
function loadXMLDoc(url)
{
var xmlhttp;
var txt,xx,x,i;
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    txt="<table border='1'><tr><th>Title</th><th>Artist</th></tr>";
    x=xmlhttp.responseXML.documentElement.getElementsByTagName("CD");
    for (i=0;i<x.length;i++)
      {
      txt=txt + "<tr>";
      xx=x[i].getElementsByTagName("TITLE");
        {
        try
          {
          txt=txt + "<td>" + xx[0].firstChild.nodeValue + "</td>";
          }
        catch (er)
          {
          txt=txt + "<td> </td>";
          }
        }
    xx=x[i].getElementsByTagName("ARTIST");
      {
        try
          {
          txt=txt + "<td>" + xx[0].firstChild.nodeValue + "</td>";
          }
        catch (er)
          {
          txt=txt + "<td> </td>";
          }
        }
      txt=txt + "</tr>";
      }
    txt=txt + "</table>";
    document.getElementById('txtCDInfo').innerHTML=txt;
    }
  }
xmlhttp.open("GET",url,true);
xmlhttp.send();
}
```

