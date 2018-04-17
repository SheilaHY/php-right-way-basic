## 使用数组的键值对代替switch
```php
$value = 0;
switch ($sex) {
    case 'male':
        $value = 1;
        break;
    case 'female':
        $value = 2;
        break;
}
```

改为：
```php
$sexMapping = ['male' => 1, 'female' => 2]
```
## 没有意义的else
```php
function test($a)
{
    if ($a) {
        return true;
    } else {
        return false;
    }
}
```
改为：
```php
function test($a)
{
    if ($a) {
        return true;
    }
    return false; 
}
```

或
```php
function test($a)
{
    return (bool) $a;
}
```

## 字符串连接
```php
$a  = 'Multi-line example'; 
$a .= "\n";
$a .= 'of what not to do';
```

改为：
```php
$a = 'Multi-line example'     
    . "\n"                    
    . 'of what to do';
```

## 字符串包含多个待解析变量值
```php
echo 'phptherightway is ' . $adjective . '.' 
    . "\n"                                     
    . 'I love learning' . $code . '!';
```

改为：
```php
echo "phptherightway is $adjective.\n I love learning $code!" 
```

## 利用三元表达式代替简单的if/else赋值
```php
if (hour >= 12) {
   $timeStr += 'PM';
} else {
   $timeStr += 'AM';
}
```

改为：
```php
$timeStr +=  (hour >= 12) ? 'PM' : 'AM';
```



## 比较运算符本身得到bool类型，无需再次赋值bool
```php
$a = 3;
return ($a == 3) ? true : false;
```

改为：
```php
$a = 3;
return $a == 3;
```

## 无意义的变量预定义
```php
$a = 'Hello world';
echo $a; //$a仅在返回时被用到
```
改为：
```php
echo 'Hello world';
```

## 条件语句中参数的顺序
```php
if ($length >= 10)
```
vs
```php
if (10 <= $length)
```
和询问的语言表达保持一致，如通常会说“如果长度大于等于10”，而不是“如果10小于等于长度”

## 不要在循环中重复地执行和访问数据库
```php
for ($i = 0; $i <= 5; $i++) {
    $isSendCoupon = Setting::getValueByKey('is_send_coupon');
    ...
}
```

改为：
```php
$isSendCoupon = Setting::getValueByKey('is_send_coupon');
for ($i = 0; $i <= 5; $i++) {
    ...
}
```

## 在model类中定义与实体类无关的方法
