# PHP  
# The Right Way  
原文：http://www.phptherightway.com/pages/The-Basics.html

## 基础知识

### 比较运算符
比较运算符是PHP经常被忽略的一个方面，它可能引发许多意想不到的结果。这样的问题来源于严格的比较（布尔类型作为整型的比较）
```php
<?php
$a = 5;   // 5 as an integer

var_dump($a == 5);       // compare value; return true
var_dump($a == '5');     // compare value (ignore type); return true
var_dump($a === 5);      // compare type/value (integer vs. integer); return true
var_dump($a === '5');    // compare type/value (integer vs. string); return false

//Equality comparisons
if (strpos('testing', 'test')) {    // 'test' is found at position 0, which is interpreted as the boolean 'false'
    // code...
}

// vs. strict comparisons
if (strpos('testing', 'test') !== false) {    // true, as strict comparison was made (0 !== false)
    // code...
}
```
[比较运算符](http://php.net/language.operators.comparison)  
[类型比较表](http://php.net/manual/zh/types.comparisons.php)  
[Comparison cheatsheet](http://phpcheatsheets.com/index.php?page=compare)

### 条件声明
#### If条件
当在一个函数或类方法中使用‘if/else’ 条件时，没有意义的'else'

```php
<?php
function test($a)
{
    if ($a) {
        return true;
    } else {
        return false;
    }
}

// vs.

function test($a)
{
    if ($a) {
        return true;
    }
    return false;    // else is not necessary
}

// or even shorter:

function test($a)
{
    return (bool) $a;
}
```

#### Switch声明
Switch声明只比较值，不比较类型(相当于‘==’)

```php
<?php
$answer = test(2);    // 'case 2' 和 'case 3'将被执行

function test($a)
{
    switch ($a) {
        case 1:
            // code...
            break;             // break终止switch
        case 2:
            // code...         // 没有break, 'case 3'将继续被比较
        case 3:
            // code...
            return $result;    // 'return'终止函数
        default:
            // code...
            return $error;
    }
}
```

### 全局命名空间
当使用命名空间时，内置的全局函数可能被自定义函数隐藏，要解决这个问题，在引用的全局函数前加上反斜线。
```php
<?php

function fopen()
{
    $file = \fopen();    // 自定义函名和内置函数相同.
                         // 执行时加上反斜线'\'.
}

function array()
{
    $iterator = new \ArrayIterator();    // ArrayIterator is an internal class. Using its name without a backslash
                                         // will attempt to resolve it within your namespace.
}
```
[全局空间](http://php.net/language.namespaces.global)  
[规则](http://php.net/userlandnaming.rules)

### 字符串
#### 连接
当一行的字符长度超过建议的120个字符长度时，为了便于阅读，最好使用连接符代替连接赋值运算符。
```php
<?php
$a  = 'Multi-line example';    // 连接赋值运算符 (.=)
$a .= "\n";
$a .= 'of what not to do';

// vs

$a = 'Multi-line example'      // 连接运算符 (.)
    . "\n"                     // 新行缩进
    . 'of what to do';
 ```
[字符串运算符](http://php.net/language.operators.string)

#### 字符串类型
字符串是一系列字符，听起来很简单。 也就是说，有几种不同类型的字符串，它们提供略有不同的语法，并具有略有不同的行为。

#### 单引号
Single quotes are used to denote a “literal string”. Literal strings do not attempt to parse special characters or variables.

If using single quotes, you could enter a variable name into a string like so: 'some $thing', and you would see the exact output of some $thing. If using double quotes, that would try to evaluate the $thing variable name and show errors if no variable was found.

```php
<?php
echo 'This is my string, look at how pretty it is.';    // no need to parse a simple string

/**
 * Output:
 *
 * This is my string, look at how pretty it is.
 */
``` 
[单引号](http://php.net/language.types.string#language.types.string.syntax.single)

#### 双引号
双引号可以解析变量，也可以解析特殊字符，如转义符"\n"

```php
<?php
echo 'phptherightway is ' . $adjective . '.'     // a single quotes example that uses multiple concatenating for
    . "\n"                                       // variables and escaped string
    . 'I love learning' . $code . '!';

// vs

echo "phptherightway is $adjective.\n I love learning $code!"  // Instead of multiple concatenating, double quotes
                                                               // enables us to use a parsable string
```
双引号包含变量
```php
<?php
$juice = 'plum';
echo "I like $juice juice";    // Output: I like plum juice
```
使用大括号包裹变量，避免混淆
```php
<?php
$juice = 'plum';
echo "I drank some juice made of $juices";    // $juice cannot be parsed

// vs

$juice = 'plum';
echo "I drank some juice made of {$juice}s";    // $juice will be parsed

/**
 * Complex variables will also be parsed within curly brackets
 */

$juice = array('apple', 'orange', 'plum');
echo "I drank some juice made of {$juice[1]}s";   // $juice[1] will be parsed
```
[双引号](http://php.net/language.types.string#language.types.string.syntax.double)

#### Nowdoc语法
Nowdoc 结构是类似于单引号字符串的,适合用于嵌入 PHP 代码或其它大段文本而无需对其中的特殊字符进行转义。
```php
<?php
$str = <<<'EOD'             // initialized by <<<
Example of string
spanning multiple lines
using nowdoc syntax.
$a does not parse.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using nowdoc syntax.
 * $a does not parse.
 */
 ```
[Nowdoc syntax](http://php.net/manual/zh/language.types.string.php#language.types.string.syntax.nowdoc)

#### Heredoc语法
Heredoc语法是类似于双引号字符串的， internally behaves the same way as double quotes except it is suited toward the use of multi-line strings without the need for concatenating.
```php
<?php
$a = 'Variables';

$str = <<<EOD               // initialized by <<<
Example of string
spanning multiple lines
using heredoc syntax.
$a are parsed.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using heredoc syntax.
 * Variables are parsed.
 */
 ```
[Heredoc语法](http://php.net/manual/zh/language.types.string.php#language.types.string.syntax.heredoc)  

哪一个更快？
有一个关于单引号字符串比双引号字符串快一点的神话。 这根本不是事实。
如果定义一个单一的字符串，并且不会进行连接或任何结构复杂的，那么单引号或双引号字符串将完全相同。 两者都不快。
如果连接任何类型的多个字符串，或者将值插入双引号字符串中，结果可能会有所不同。 如使用少量值，连接速度要快得多。 `有了很多值，内插速度要快得多`。
无论您使用字符串做什么，都不会对您的应用程序产生任何显着影响。 试图重写代码以使用其中一种方法总是徒劳无功，所以要避免这种微观优化，除非你真正理解差异的含义和影响。
[证明“单引号性能神话”是不对的](http://nikic.github.io/2012/01/09/Disproving-the-Single-Quotes-Performance-Myth.html)

### 三元表达式
三元表达式是压缩代码的一个好方法，但是常常被过度使用。虽然三元运算符可以堆叠/嵌套，但建议每行使用一个以提高可读性。
```php
<?php
$a = 5;
echo ($a == 5) ? 'yay' : 'nay';
```
相比之下，这是一个牺牲所有形式的可读性以减少行数的例子。
```php
<?php
echo ($a) ? ($a == 5) ? 'yay' : 'nay' : ($b == 10) ? 'excessive' : ':(';    // excess nesting, sacrificing readability
To ‘return’ a value with ternary operators use the correct syntax.
```

```php
<?php
$a = 5;
echo ($a == 5) ? return true : return false;    // this example will output an error

// vs

$a = 5;
return ($a == 5) ? 'yay' : 'nope';    // this example will return 'yay'
```
应该注意的是不需要使用三元表达式返回一个布尔值，这将是一个例子。
```php
<?php
$a = 3;
return ($a == 3) ? true : false; // Will return true or false if $a == 3

// vs

$a = 3;
return $a == 3; // Will return true or false if $a == 3
```
与三元运算符一起使用括号来表示形式和功能
当使用三元运算符时，括号可以发挥其作用以提高代码的可读性，并且还可以在语句块中包含联合。 当没有要求使用括号时的一个例子是：
```php
<?php
$a = 3;
return ($a == 3) ? "yay" : "nope"; // return yay or nope if $a == 3

// vs

$a = 3;
return $a == 3 ? "yay" : "nope"; // return yay or nope if $a == 3
```
括号还使我们能够在块将作为整体进行检查的语句块中创建联合。 例如下面的这个例子，如果（$ a == 3和$ b == 4）都为真并且$ c == 5也是true，则返回true。

```php
<?php
return ($a == 3 && $b == 4) && $c == 5;
```
Another example is the snippet below which will return true if ($a != 3 AND $b != 4) OR $c == 5.
```php
<?php
return ($a != 3 && $b != 4) || $c == 5;
```
从PHP5.3开始，三元运算符的中间部分可能拿掉。表达式“expr1 ?: expr3”返回"expr1"如果expri==TRUE，否则返回“expr3”

[三元表达式](http://php.net/language.operators.comparison)

### 变量定义
有时，编码人员试图通过用不同的名称声明预定义的变量来使他们的代码变得更“干净”。 这实际上是将所述脚本的内存消耗加倍。
对于下面的示例，让我们假设一个示例的文本字符串包含1MB的数据，方法是将您将脚本执行次数增加到2MB的变量。
```php
<?php
$about = 'A very long string of text';    // uses 2MB memory
echo $about;

// vs

echo 'A very long string of text';        // uses 1MB memory
```
[性能Tips](https://developers.google.com/speed/articles/optimizing-php)
