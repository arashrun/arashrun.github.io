
# php

#### 四种php页面间传递数据方法
1. 使用浏览器的cookie
```php
<?php
       setcookie('mycookie','自灵');
?>
<?php
     $wuziling = $_COOKIE['mycookie'];
     echo $wuziling;
?>
```

2. 使用服务器session
```php
<?php
    session_start();
    $_SESSION["temp"]=array('123','456','789');
?>
<?php
     session_start();
     for($i=0;$i<3;$i++)
     {
             echo $_SESSION['temp'][$i].'<br />';
     }
?>
```

3. 使用表单来传递
```html
    <form action="page02.php" method="post">
         <input type="text" name="wuziling" />
         <input type="submit" name="submit" value="提交" />
    </form>
```
```php
    <?php
        //使用post变量来获取
        $wu = $_POST['wuziling'];
        echo $wu;
    ?>
```

4. 使用超链接
```php
    <?php
    $var = 'I love you !';
    ?>
    <a href="<?php echo "page02.php?new=".$var ?>">get</a>

    <?php
         echo   $_GET['new'];
    ?>
```

#### js使用php变量

```js
    var arr="<?php echo $column['column_type'];?>";
```

#### array常用操作

```php
$arr = array(1, 2, 3);
count($arr)//get arr size
var_dump($arr)//print arr context

```

# js

```html
<head>
//第一种引用方式
<script type="text/javascript">
	var word = "Hello, itbsl";
	alert(word);
</script>
//第二种方式
<script type="text/javascript" src="./js/example.js"></script>
</head>


<body>
//第三种方式
<script type="text/javascript">
	var word = "Hello, itbsl";
	alert(word);
</script>
</body>
```


** 常用cdn **
```html
<script type="text/javascript" src="http://ajax.microsoft.com/ajax/jquery/jquery-1.4.min.js"></script>
```



# css

```html
//方式一
<head><link rel="stylesheet" type="text/css" href="mystyle.css"></head>

//方式二
<head>
<style type="text/css">
body {background-color: red}
p {margin-left: 20px}
</style>
</head>

//方式三
<p style="color: red; margin-left: 20px">
This is a paragraph
</p>
```

# h5