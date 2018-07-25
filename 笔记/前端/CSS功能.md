### 下拉菜单

```
<!DOCTYPE html>
<html>
<head>
<title>下拉菜单实例|W3Cschool教程(w3cschool.cn)</title>
<meta charset="utf-8">
<style>
.dropbtn {
    background-color: #4CAF50;
    color: white;
    padding: 16px;
    font-size: 16px;
    border: none;
    cursor: pointer;
}

.dropdown {
    position: relative;
    display: inline-block;
}

.dropdown-content {
    display: none;
    position: absolute;
    right: 0;
    background-color: #f9f9f9;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
}

.dropdown-content a {
    color: black;
    padding: 12px 16px;
    text-decoration: none;
    display: block;
}

.dropdown-content a:hover {background-color: #f1f1f1}

.dropdown:hover .dropdown-content {
    display: block;
}

.dropdown:hover .dropbtn {
    background-color: #3e8e41;
}
</style>
</head>
<body>

<h2>下拉内容的对齐方式</h2>
<p>left 和 right 属性指定了下拉内容是从左到右或从右到左。</p>

<div class="dropdown" style="float:left;">
  <button class="dropbtn">左</button>
  <div class="dropdown-content" style="left:0;">
    <a href="#">W3Cschool教程 1</a>
    <a href="#">W3Cschool教程 2</a>
    <a href="#">W3Cschool教程 3</a>
  </div>
</div>

<div class="dropdown" style="float:right;">
  <button class="dropbtn">右</button>
  <div class="dropdown-content">
    <a href="#">W3Cschool教程 1</a>
    <a href="#">W3Cschool教程 2</a>
    <a href="#">W3Cschool教程 3</a>
  </div>
</div>

</body>
</html>
```

### 图片画廊

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>W3Cschool教程(w3cschool.cn)</title> 
<style>


div.img
{
  margin: 2px;
  border: 1px solid #000000;
  height: auto;
  width: auto;
  float: left;
  text-align: center;
}    
div.img img
{
  display: inline;
  margin: 3px;
  border: 1px solid #ffffff;
}
div.img a:hover img {border: 1px solid #0000ff;}
div.desc
{
  text-align: center;
  font-weight: normal;
  width: 120px;
  margin: 2px;
}
</style>
</head>


<body>
<div class="img">
 <a target="_blank" href="javascript;:"><img src="/statics/images/course/klematis_small.jpg" alt="Klematis" width="110" height="90"></a>
 <div class="desc">Add a description of the image here</div>
</div>
<div class="img">
 <a target="_blank" href="javascript;:"><img src="/statics/images/course/klematis2_small.jpg" alt="Klematis" width="110" height="90"></a>
 <div class="desc">Add a description of the image here</div>
</div>
<div class="img">
 <a target="_blank" href="javascript;:"><img src="/statics/images/course/klematis3_small.jpg" alt="Klematis" width="110" height="90"></a>
 <div class="desc">Add a description of the image here</div>
</div>
<div class="img">
 <a target="_blank" href="javascript;:"><img src="/statics/images/course/klematis4_small.jpg" alt="Klematis" width="110" height="90"></a>
 <div class="desc">Add a description of the image here</div>
</div>
</body>
```

### 透明图层

```
<html>
<head>
<style>
div.background  
{  
width:500px;  
height:250px;  
background:url(klematis.jpg) repeat; 
border:2px solid black;  
}
div.transbox  
{  
width:400px;  
height:180px;  
margin:30px 50px;  
background-color:#ffffff;  
border:1px solid black;  
opacity:0.6;  
filter:alpha(opacity=60); /* For IE8 and earlier */  
}
div.transbox p  
{  
margin:30px 40px;  
font-weight:bold;  
color:#000000;  
}
</style>
</head>

<body>

<div class="background">
<div class="transbox">
<p>This is some text that is placed in the transparent box.This is some text that is placed in the transparent box.This is some text that is placed in the transparent box.This is some text that is placed in the transparent box.This is some text that is placed in the transparent box.</p>
</div>
</div>

</body>
</html>
```

### 图片拼接

因为这是一个单一的图像，而不是6个单独的图像文件，当用户停留在图像上不会有延迟加载。

```
<!DOCTYPE html>
<html>
<head>
<style>
#navlist{position:relative;}
#navlist li{margin:0;padding:0;list-style:none;position:absolute;top:0;}
#navlist li, #navlist a{height:44px;display:block;}

#home{left:0px;width:46px;}
#home{background:url('/statics/images/course/img_navsprites_hover.gif') 0 0;}
#home a:hover{background: url('/statics/images/course/img_navsprites_hover.gif') 0 -45px;}

#prev{left:63px;width:43px;}
#prev{background:url('/statics/images/course/img_navsprites_hover.gif') -47px 0;}
#prev a:hover{background: url('/statics/images/course/img_navsprites_hover.gif') -47px -45px;}

#next{left:129px;width:43px;}
#next{background:url('/statics/images/course/img_navsprites_hover.gif') -91px 0;}
#next a:hover{background: url('/statics/images/course/img_navsprites_hover.gif') -91px -45px;}
</style>
</head>

<body>
<ul id="navlist">
  <li id="home"><a href="default.asp"></a></li>
  <li id="prev"><a href="css_intro.asp"></a></li>
  <li id="next"><a href="css_syntax.asp"></a></li>
</ul>
</body>
</html>
```

