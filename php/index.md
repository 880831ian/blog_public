# PHP 介紹


介紹 PHP
<!--more-->

## 1. PHP 是什麼
PHP 全名是(PHP:Hypertext Preprocessor)，是後端的腳本語言

根據W3Techs的報告，截至2021年9月，有78.9%網站都是使用PHP，像是著名的 FaceBook、Tesla、Slack、WordPress
	
[stackshare](https://stackshare.io/php) 是分享開發工具的網站，可以看到不少公司也是使用PHP來進行開發的

其優點是

* 是開源、免費的
* 是跨平台的開發語言，Linux、Windows、macOS也可以使用
* 能開發：動態網站、爬蟲程式、WordPress 外掛佈景主題
* 也可以結合多種的資料庫，例如：Mysql、Mariadb、Oracle


過去使用 PHP 開發網頁程式，就是有什麼寫什麼，程式邏輯和網頁顯示混在一起，雖然開發方便但會造成高維護成本，和難以擴充功能

* 物件導向程式設計
* 函數式編程
* 事件驅動程式開發

PHP 有各式各樣的 Web Framework：

* Laravel - 最受歡迎的PHP Web Framewrok
* Codelgniter - 最新版的Codelgniter4 只有 1.4 MB，小而強

## 2. PHP 基本語法

### 標籤

PHP 程式可以放置在檔案中的任何位置，其檔案副檔名是"```.php```"，

* PHP的標籤是開頭 <?php 以及 ?> 結尾

```php
<?php 
[ 程式碼 ]
?>
```
* PHP 不分大小寫，例如 if、else、while、echo，函數等，但**變數有區分大小寫**

(圖1)

### 註解

PHP 程式中可以使用註解功能，註解後該段程式不會被執行

* PHP 單行註解

(圖2)

* PHP 多行註解

(圖3)

### 變數

PHP 程式中可以使用變數功能，變數是儲存訊息的容器，變數以" ```$``` "符號開頭，後面是變數名稱

PHP 變數規則

* 變數名必須以字母或是下底線開頭，不可以**用數字開頭**
* 變數名只包含字母數字或是下底線(A-z、0-9、_)
* 變數名區分大小寫 ($age 跟 $AGE 是兩個不同的變數)

```php
<?php 
$color = 'blue';
$items = 'Toy';
$num1 = '13';
$num2 = '18';
echo "I have " .$num1+$num2 ." ". $color ." ". $items;
?>
```
(圖4)

### 變數作用區9

PHP 變數有不同的作用區(local、global、static)

#### local

函式中的變數具有local scope，只能用於該函式中使用
	
```php
	<?php
	function myTest() {
	  $x = 5; // local scope
	  echo "<p>Variable x inside function is: $x</p>";
	}
	myTest();
	
	// using x outside the function will generate an error
	echo "<p>Variable x outside function is: $x</p>";
	?>
	```
	
(圖5)

#### global

函式外部的變數具有global scope，只能用於該函式外部使用
	
	```php
	<?php
	$x = 5; // global scope
	
	function myTest() {
	  // using x inside this function will generate an error
	  echo "<p>Variable x inside function is: $x</p>";
	}
	myTest();
	
	echo "<p>Variable x outside function is: $x</p>";
	?>
	```
	
	(圖6)
	
想要在函式中，用外部的變數，就在函式內用global 來定義變數
	
	```php
	<?php
	$x = 5;
	$y = 10;
	
	function myTest() {
	  global $x, $y;
	  $y = $x + $y;
	}
	
	myTest();
	echo $y; // outputs 15
	?>
	```

	(圖7)

#### static

正常函式執行完，變數值都會被刪除，想要保留值可以加 static
	
	```php
	<?php
	function myTest() {
	  static $x = 0;
	  echo $x;
	  $x++;
	}
	
	myTest();
	myTest();
	myTest();
	?>
	```
	
	(圖8)

### 顯示

PHP 程式中可以用 echo 以及 print 來顯示資訊 

* PHP echo / print 顯示

	```php
	<?php
	$txt1 = "學習PHP";
	$txt2 = "Ian_Zhuang";
	$x = 5;
	$y = 4;
	
	echo "<h2>" . $txt1 . "</h2>";
	echo $txt2 . " 在 " . $txt1 . "<br>";
	echo $x + $y;
	
	print "<h2>" . $txt1 . "</h2>";
	print $txt2 . " 在 " . $txt1 . "<br>";
	print $x + $y;
	?>
	```
	(圖9)

### 資料型態

變數可以儲存不同型態的資料，以下是 PHP 支持的資料型態 

! 可以用var_dump() 函數
來查看資料的x態以及值

* 字串 (String)
* 整數 (Integer)
* 浮點數 (Float)
* 布林 (Boolean)
* 陣列 (Array)
* 物件 (Object)
* 空值 (NULL)

#### 字串 (String)
字串可以是引號內的任何文本，可以使用單引號或雙引號來表示

```php
<?php
$x = "Hello world!";
$y = 'Hello world!';

echo $x;
echo "<br>";
echo $y;
?>
```
(圖10)

#### 整數 (Integer)
整數有幾點規則：

1. 必須至少有一個數字 
2. 不能有小數點 
3. 可以是正數也可以是負數 
4. 可以指定不同的進制表示法
5. 其範圍介於 -2,147,483,648 和 2,147,483,647 之間的非十進制數

``` php
<?php
$x = 5985;
var_dump($x);
?>
```
(圖11)

#### 浮點數 (Float)

浮點數是帶小數點的數字或指數形式的數字

```php
<?php
$x = 10.365;
var_dump($x);
?>
```
(圖12)

#### 布林 (Boolean)

布爾值表示兩種可能的狀態：TRUE 或 FALSE

```php
<?php
$x = true;
$y = false;
var_dump($x);
?>
```
(圖13)

#### 陣列 (Array)

陣列可以將多個值存在一個變數中

```php
<?php
$x = 10.365;
var_dump($x);
?>
```
(圖14)

#### 物件 (Object)

在討論物件前，要先知道類別(Class)與物件(Object)的關係

* 類別 (Class) ：可以比喻為房屋的設計藍圖，是為了讓大家了解房屋的結構與形狀。
* 物件 (Object) ：可以比喻為真的房屋，物件是類別的實體化

為了要蓋好一棟房子有房子的設計藍圖(類別)還不夠，還需要蓋房子的材料，而材料就是所謂的資料(data)。

類別定義結構和行為用來產生物件，當多個物件是由同一
個類別產生出來時，每個物件都是一個獨立個體。

1. 建立類別語法很簡單，只需要使用 Class 來定義一個類別

```php
<?php
class MyClass
{
    // 在大括號裡面宣告類別的屬性與方法
}

$obj = new MyClass; //使用 new 來實體化類別並將他存入變數中
var_dump($obj); //查看類別內容
?>

```

(圖15)

2. 我們在替 MyClass 加入屬性，用 public 決定屬性的可視性

```php
<?php
class MyClass
{
    public $prop = "I'm a class Property";
}

$obj = new MyClass;
echo $obj->prop;
?>
```

(圖16)

因為有很多物件實例化都來自同一個類別，如果沒有指定被實體化的物件，程式碼會無法判斷，所以要用 **->** 在 PHP 的物件中，來存取物件的屬性和方法。


3. 定義類別(Class)的方法(Methods)，方法(Methods)是類別裡面的函式(Functions)，物件可以藉由執行這些方法來更動每個物件的行為。


```php
<?php
class MyClass
{
    public $prop = "I'm a class Property!";

    public function set($newval)
    {
        $this->prop = $newval;
    }

    public function get()
    {
        return $this->prop . "<br />";
    }
}

$obj = new MyClass;
echo $obj->get();                             //得到屬性的值。

$obj->set("I'm a new Property value!");       //設定新的屬性的值。
echo $obj->get();
?>
```

(圖17)

物件導向允許物件透過$this來參考自己。物件使用$this就如同直接使用物件名稱來指定物件，同等於MyClass->prop1。

#### 空值 (NULL)

Null 是一種特殊的數據類型，它只能有一個值：NULL

如果創建的變量沒有值，則會自動為其分配 NULL 

```php
<?php
$x = "Hello world!";
$x = null;
var_dump($x);
?>
```
(圖18)

### 運算符號

PHP 將運算符號分為以下幾組

* 算術運算符
* 賦值運算符
* 比較運算符
* 邏輯運算符
* 字符串運算符

####算術運算符

```php
<?php 
$x = 3; $y = 6;
echo $x + $y;  // 輸出 x + y ，3 + 6 ，顯示 9
echo $x - $y;  // 輸出 x - y ，3 - 6 ，顯示 -3
echo $x * $y;  // 輸出 x * y ，3 * 6 ，顯示 18
echo $x / $y;  // 輸出 x / y ，3 / 6 ，顯示 0.5
echo $x % $y;  // 輸出 x % y ，3 % 6 ，顯示 3
echo $x ** $y; // 輸出 x ^ y ，3 ^ 6 ，顯示 729
?>
```


####賦值運算符

```php
<?php 
$x = 3; $y = 6; echo $x += $y;  //輸出 x = x + y ， 9 = 3 + 6 ，輸出 9
$x = 3; $y = 6; echo $x -= $y;  //輸出 x = x - y ， -3 = 3 - 6 ，輸出 -3
$x = 3; $y = 6; echo $x *= $y;  //輸出 x = x * y ， 18 = 3 * 6 ，輸出 18
$x = 3; $y = 6; echo $x /= $y;  //輸出 x = x / y ， 0.5 = 3 / 6 ，輸出 0.5
$x = 3; $y = 6; echo $x %= $y;  //輸出 x = x % y ， 3 = 3 % 6 ，輸出 3
?>
```


####比較運算符

```php
<?php 
$x = 3; $y = 6; $z = "6";
var_dump($x == $y);  // 判斷 x 等於 y (值)，3 不等於6 ，回傳 false
var_dump($x === $z); // 判斷 x 等於 z (型態)，數字不等於字串 ，回傳 false
var_dump($x != $y);  // 判斷 x 不等於 y (值)，3不等於6 ，回傳 true
var_dump($x <> $y);  // 判斷 x 不等於 y (型態)，數字不等於字串 ，回傳 true
var_dump($x > $y);   // 判斷 x 大於 y ，3沒有大於6 ，回傳 false
var_dump($x < $y);   // 判斷 x 小於 y ，3小於6 ，回傳 true
var_dump($x >= $y);  // 判斷 x 大於等於 y ，3沒有大於等於6 ，回傳 false
var_dump($x <= $y);  // 判斷 x 小於等於 y ，3小於等於6 ，回傳 true
var_dump($x <=> $y); // 判斷 x y 的關係 ，如果 x 大於 y ，回傳 1 ; 如果 x 小於 y ，就回傳 0 ; 如果 x 等於 y ，就回傳+1
?>
```

####邏輯運算符

```php
<?php 
$x = 3; $y = 6;
if ($x == 3 and $y == 6){  echo "And"; }  // 判斷 x 等於 3 且 y 等於 6 (兩者都要true)
if ($x == 3 or $y or 9){  echo "Or"; }    // 判斷 x 等於 3 或 y 等於 9 (擇一true)
if ($x == 3 && $y == 6){  echo "And"; }   // 判斷 x 等於 3 且 y 等於 6 (兩者都要true)
if ($x == 3 || $y == 9){  echo "Or"; }    // 判斷 x 等於 3 或 y 等於 9 (擇一true)
if ($x == 3 xor $y == 9){  echo "Xor"; }  // 判斷 x 等於 3 或 y 等於 9 (任一者為true，但只能一者為true)
if ($x != 4){  echo "Not"; }              // 判斷 x 不等於 4
?>
```

### 條件判斷

條件語句用於根據不同的條件下的操作

* if 
* if...else
* if...elseif...else
* switch

#### if 

如果條件為真，就執行代碼

* 案例：如果現在時間(hour)小於20，就輸出 Have a good day ! 

```php
<?php
$t = date("H");

if ($t < "20") {
  echo "Have a good day!";
}s
?>
```

* 輸出

```html
Have a good day!
```

#### if-else

如果條件為真，就執行該程式碼，如果條件為假，就執行另一個程式碼

* 案例：如果現在時間(hour)小於20，就輸出 Have a good day! ，超過時間，就輸出 Have a good night! 

```php
<?php
$t = date("H");

if ($t < "20") {
  echo "Have a good day!";
}
else {
  echo "Have a good night!";
}
?>
```

#### if-elseif-else

針對兩個以上的條件判斷，如果條件為真，就執行該程式碼，如果條件為假，就執行另一個程式碼

* 案例：如果現在時間(hour)小於10，就輸出 Have a good morning! ，如果時間小於20，輸出Have a good day!，否則，就輸出 Have a good night! 

```php
<?php
$t = date("H");

if ($t < "10") {
  echo "Have a good morning!";
}
elseif ($t < 20)  {
  echo "Have a good day!";
}
else {
  echo "Have a good night!";
}
?>
```

#### switch
根據不同的條件來執行不同的操作

```php
<?php
$weather = '下雨天';

switch($weather)
{
    case '晴天':
        echo "今天天氣是『 $weather 』";
        break;
    case '陰天':
        echo "今天天氣是『 $weather 』";
        break;
    case '下雨天':
        echo "今天天氣是『 $weather 』";
        break;
    default:
        echo "世界末日";
        break;
}
?>
```

### 迴圈

想要相同程式碼反覆執行一定的次數

* while 
* do...while
* for
* foreach

#### while 

如果條件為真，就執行代碼

```php
<?php
$x = 1;

while($x <= 3) {
  echo "The number is: $x <br>";
  $x++;
}
?>

```

* 輸出

```html
The number is: 1
The number is: 2
The number is: 3
```
#### do...while 

先執行一次do，再檢查指定條件是否為真，是的話就會循環通過該程式碼

```php
<?php
$x = 1;

do {
  echo "The number is: $x <br>";
  $x++;
} while ($x <= 5);
?>
```

* 輸出

```html
The number is: 6
```
因為do…while的條件會在執行循環程式碼後才檢查，所以do…while **至少會循環一次**該程式碼。

#### for

假如預先知道需要循環幾次，就可以使用for，可以設定程式碼在循環中的次數。

```php
<?php
for ($x = 0; $x <= 10; $x++) {
  echo "The number is: $x <br>";
}
?>
```

* 輸出

```html
The number is: 0
The number is: 1
The number is: 2
The number is: 3
The number is: 4
The number is: 5
The number is: 6
The number is: 7
The number is: 8
The number is: 9
The number is: 10
```

#### foreach

此循環適用於陣列，可以用來顯示陣列的key跟value

```php
<?php
$age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");

foreach($age as $x => $val) {
  echo "$x = $val<br>";
}
?>
```

* 輸出

```html
Peter = 35
Ben = 37
Joe = 43
```

#### break

想要在迴圈執行中跳出，可以使用break 指令，來跳出循環。

```php
<?php  
for ($x = 0; $x < 10; $x++) {
  if ($x == 4) {
    break;
  }
  echo "The number is: $x <br>";
}
?>
```

* 輸出

```html
The number is: 0
The number is: 1
The number is: 2
The number is: 3
```

#### continue

continue 與 break 是相對的指令。break 中斷目前執行的迴圈，continue 則是回到迴圈的開頭，執行「下一次」迴圈。

```php
<?php
for ($x = 0; $x < 5; $x++) {
  if ($x == 2) {
    continue;
  }
  echo "The number is: $x <br>";
}
?>
```

* 輸出

```html
The number is: 0
The number is: 1
The number is: 3
The number is: 4
```

### 函式

PHP除了內建的函式外，還可以創建自己的函式
注意：函式名稱必須以字母或下底線開頭。不區分大小寫。

```php
<?php
function writeMsg() {
  echo "Hello world!";
}

writeMsg(); // call the function
?>
```

* 輸出

```html
Hello world!
```

也可以在函式裡面放入任意數量的參數，只需要用逗號分開


```php
<?php
function familyName($fname, $year) {
  echo "$fname Born in $year <br>";
}

familyName("Hege", "1975");
familyName("Stale", "1978");
familyName("Kai Jim", "1983");
?>
```

* 輸出

```html
Hege Born in 1975
Stale Born in 1978
Kai Jim Born in 1983
```

## 3. PHP 進階語法

### 日期和時間
可以透過輸入參數，顯示想要的日期

* d - 代表月份中的某天（01 到 31）
* m - 代表一個月（01 到 12）
* Y - 代表年份（四位數）
* l（小寫“L”）— 代表星期幾


```php
<?php
echo "Today is " . date("Y/m/d") . "<br>";
echo "Today is " . date("Y.m.d") . "<br>";
echo "Today is " . date("Y-m-d") . "<br>";
echo "Today is " . date("l");
?>
```

* 輸出

```html
Today is 2022/02/22
Today is 2022.02.22
Today is 2022-02-22
Today is Tuesday
```
可以使用 strtotime()，顯示想要的日期

```php
<?php
$d=strtotime("tomorrow");
echo date("Y-m-d h:i:sa", $d) . "<br>";

$d=strtotime("next Saturday");
echo date("Y-m-d h:i:sa", $d) . "<br>";

$d=strtotime("+3 Months");
echo date("Y-m-d h:i:sa", $d) . "<br>";
?>
```

* 輸出

```html
2022-02-23 12:00:00am
2022-02-26 12:00:00am
2022-05-22 04:21:41am
```

可以透過輸入參數，顯示想要的時間

* H - 一個小時的 24 小時格式（00 到 23）
* h - 小時的 12 小時格式，前面補零（01 到 12）
* i - 前面補零的分鐘（00 到 59）
* s - 前面補零的秒數（00 到 59）
* a - 小寫的 Ante meridiem 和 Post meridiem（am 或 pm）


```php
<?php
date_default_timezone_set("Asia/Taipei"); 
echo "The time is " . date("h:i:sa");
?>
```

* 輸出

```html
The time is 12:12:10am
```
###json

可以使用json_encode()，將陣列存為JSON格式，也可以用json_decode()，將JSON格式變成陣列。

```php
<?php
$age = array("Peter"=>'35', "Ben"=>'45', "Ian"=>'22');
echo $enjson = json_encode($age);
var_dump(json_decode($enjson,true));
?>
```

* 輸出

```html
{"Peter":"35","Ben":"45","Ian":"22"}
array (size=3)
  'Peter' => string '35' (length=2)
  'Ben' => string '45' (length=2)
  'Ian' => string '22' (length=2)
```


## 4. PHP 表單

PHP 可以用 $_GET 和 $_POST 於收集表單數據

### GET

我們先模擬 $_GET ，先建立一個 index.html 的檔案程式碼如下
* HTML 表單 (GET)

```html
<html>
<body>

<form action="welcome_get.php" method="get">
Name: <input type="text" name="name"><br>
E-mail: <input type="text" name="email"><br>
<input type="submit">
</form>

</body>
</html>
```
再建立一個 welcome.php 的檔案程式碼如下

* PHP ($_GET)

```php
<html>
<body>

Welcome <?php echo $_GET["name"]; ?><br>
Your email address is: <?php echo $_GET["email"]; ?>

</body>
</html>

```
我們先模擬 $_POST ，先建立一個 index.html 的檔案程式碼如下
* HTML 表單 (POST)

```html
<html>
<body>

<form action="welcome_get.php" method="post">
Name: <input type="text" name="name"><br>
E-mail: <input type="text" name="email"><br>
<input type="submit">
</form>

</body>
</html>
```

### POST

再建立一個 welcome.php 的檔案程式碼如下

* PHP ($_POST)

```php
<html>
<body>

Welcome <?php echo $_POST["name"]; ?><br>
Your email address is: <?php echo $_POST["email"]; ?>

</body>
</html>

```

## 5. PHP 常用函式

## 6. PHP RESTful

## 7. PHP 進階語法

## 8. 資料來源



Middleware 元件設定)

