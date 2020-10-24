
## 錯誤代碼
[Php 錯誤級別](https://www.php.net/manual/en/errorfunc.constants.php)
| 代碼 | Constant            | Description                                                   |
| ---- | ------------------- | ------------------------------------------------------------- |
| 1    | E_ERROR             | 致命的執行時錯誤。 錯誤無法恢復過來。指令碼的執行被暫停       |
| 2    | E_WARNING           | 非致命的執行時錯誤。 指令碼的執行不會停止                     |
| 4    | E_PARSE             | 編譯時解析錯誤。解析錯誤應該只由分析器生成                    |
| 8    | E_NOTICE            | 執行時間的通知。                                              |
| 16   | E_CORE_ERROR        | 在PHP啟動時的致命錯誤。這就好比一個在PHP核心的E_ERROR         |
| 32   | E_CORE_WARNING      | 在PHP啟動時的非致命的錯誤。這就好比一個在PHP核心E_WARNING警告 |
| 64   | E_COMPILE_ERROR     | 致命的編譯時錯誤。 這就像由Zend指令碼引擎生成了一個E_ERROR    |
| 128  | E_COMPILE_WARNING   | 非致命的編譯時錯誤，由Zend指令碼引擎生成了一個E_WARNING警告   |
| 256  | E_USER_ERROR        | 致命的使用者生成的錯誤。                                      |
| 512  | E_USER_WARNING      | 非致命的使用者生成的警告。                                    |
| 1024 | E_USER_NOTICE       | 使用者生成的通知。                                            |
| 2048 | E_STRICT            | 執行時間的通知。                                              |
| 4096 | E_RECOVERABLE_ERROR | 捕捉致命的錯誤。                                              |
| 8191 | E_ALL來             | 所有的錯誤和警告。                                            |

## 函數

內建函數

[funcref](https://www.php.net/manual/en/funcref.php)

[string](https://www.php.net/manual/en/book.strings.php)

[array](https://www.php.net/manual/en/book.array.php)

[classobj](https://www.php.net/manual/en/book.classobj.php)

[math](https://www.php.net/manual/en/book.math.php)

## Type declarations [¶](https://www.php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration)

| Type                 | Description                                                                                                                                  | Minimum PHP version |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| Class/interface name | The parameter must be an instanceof the given class or interface name.                                                                       | PHP 5.0.0           |
| self                 | The parameter must be an instanceof the same class as the one the method is defined on. This can only be used on class and instance methods. | PHP 5.0.0           |
| array                | The parameter must be an array.	PHP                                                                                                          | 5.1.0               |
| callable             | The parameter must be a valid callable.                                                                                                      | PHP 5.4.0           |
| bool                 | The parameter must be a boolean value.                                                                                                       | PHP 7.0.0           |
| float                | The parameter must be a floating point number.                                                                                               | PHP 7.0.0           |
| int                  | The parameter must be an integer.                                                                                                            | PHP 7.0.0           |
| string               | The parameter must be a string.                                                                                                              | PHP 7.0.0           |
| iterable             | The parameter must be either an array or an instanceof Traversable.                                                                          | PHP 7.1.0           |
| object               | The parameter must be an object.                                                                                                             | PHP 7.2.0           |

## 變量

* $GLOBALS — 引用全局作用域中可用的全部变量
* $_SERVER — 服务器和执行环境信息
* $_GET — HTTP GET 变量
* $_POST — HTTP POST 变量
* $_FILES — HTTP 文件上传变量
* $_REQUEST — HTTP Request 变量
* $_SESSION — Session 变量
* $_ENV — 环境变量
* $_COOKIE — HTTP Cookies
* $php_errormsg — 前一个错误信息
* $HTTP_RAW_POST_DATA — 原生 POST 数据
* $http_response_header — HTTP 响应头
* $argc — 传递给脚本的参数数目
* $argv — 传递给脚本的参数数组

### 外部變量

當有 HTML
```html
<input type="image" src="image.gif" name="sub" />
```
PHP 會自動在補兩個變量 sub_x and sub_y.
These contain the coordinates of the user click within the image.
表示使用者點在圖片的位置

### 命名變量

```php
$a= 'x';
$$a = 'y';

echo $$a ; // 'y'，因為 $($a) -> $x -> 'y';
```
| Expression            | PHP 5 interpretation   | PHP 7 interpretation    |
| --------------------- | ---------------------- | ----------------------- |
| $$foo['bar']['baz']   | \${$foo['bar']['baz']} | ($$foo)['bar']['baz']   |
| \$foo->$bar['baz']    | \$foo->{$bar['baz']}   | (\$foo->$bar)['baz']    |
| \$foo->$bar\['baz']() | \$foo->{$bar['baz']}() | (\$foo->$bar)\['baz']() |
| Foo::$bar\['baz']()   | Foo::{$bar['baz']}()   | (Foo::$bar)\['baz']()   |


## 表達式

1. 括號
```php
(
    $colors = array('red', 'blue', 'green', 'yellow')
);

// 括號加上分號 or 大括號 然後裡面加上分號

{
    $colors = array('red', 'blue', 'green', 'yellow');
}
print_r($colors);
```
2. 變數
```php
for($i = 0 ; $i < count($colors); $i++) {
    $abc='gaugadi';
    echo $colors[$i]."\n";
}
echo $abc.$i; // 竟然可以取到
```
3. 函數
```php
function printVA() {
  for($i = 0 ; $i < count($colors); $i++) { // ERROR 找不到 $colors，$colors 必須在 function 內宣告
      echo $colors[$i]."\n";
  }
}
```

## 常數
Function 不行用 const，但是用 define 很危險，會變成全域常數。

Class 的 property 不行用 define ，但是用 const 會變成靜態屬性，要用 self::NAME_OF_CONSTANT 取得該常數
```php
Const a = 'b';
define('a', 'b', true);
```

| Name          | Description                                                                                                                                                                                                                            |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| __LINE__      | The current line number of the file.                                                                                                                                                                                                   |
| __FILE__      | The full path and filename of the file with symlinks resolved. If used inside an include, the name of the included file is returned.                                                                                                   |
| __DIR__       | The directory of the file. If used inside an include, the directory of the included file is returned. This is equivalent to dirname(__FILE__). This directory name does not have a trailing slash unless it is the root directory.     |
| __FUNCTION__  | The function name, or {closure} for anonymous functions.                                                                                                                                                                               |
| __CLASS__     | The class name. The class name includes the namespace it was declared in (e.g. Foo\Bar). Note that as of PHP 5.4 __CLASS__ works also in traits. When used in a trait method, __CLASS__ is the name of the class the trait is used in. |
| __TRAIT__     | The trait name. The trait name includes the namespace it was declared in (e.g. Foo\Bar).                                                                                                                                               |
| __METHOD__    | The class method name.                                                                                                                                                                                                                 |
| __NAMESPACE__ | The name of the current namespace.                                                                                                                                                                                                     |

## 字串

1.
  ```php
  $arr['s'] = 'axx';
  // 以下皆為正確顯示 hello axx
  echo " hello "  . $arr['s'];
  echo "hello $arr[s]"; //  雙引號自動偵測變數，且陣列裡的中括號不用表示字串
  echo "hello {$arr['s']}"; // 用大括號表達 complex，裡面為表達式。
  ```
2.
左邊會空行
右邊則是字串
```php
"\n"  !==  '\n'
```

3.
The [string](https://www.php.net/manual/en/language.types.string.php) in PHP is implemented as an array of bytes and an integer indicating the length of the buffer.
PHP 裡的字串其實是由 byte 的陣列與顯示 buffer 長度的整數組成，這也是為何 php 沒有 []bytes。
string will be encoded in whatever fashion it is encoded in the script file.
字串會自動但照現在的趨勢自動編碼。有個  Zend Multibyte 的開關，控制是否自動判別字串編碼

## Class 類別

- $this
-
  如果 class A 有個靜態方法指向 \$this，
  當 class B 使用 class a 的靜態方法時，$this 在版本 7 跟版本 5 有不一樣的結果。
  7 的 會是 undefined; 5 則是正常指向 class B 的物件。
