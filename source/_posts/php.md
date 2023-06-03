---
title: php
date: 2023-05-11 21:15:23
categories:
- web
tags:
- php
---


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
