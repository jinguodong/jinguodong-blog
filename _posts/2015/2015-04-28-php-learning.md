---
layout: post
title: PHP基础
categories:
- PHP
tags:
- PHP
- 学习笔记
---

## 函数

    function traceHelloPHP(){
        echo 'Hello';
    }
    $func = 'traceHelloPHP'
    $func()
    function traceNum($a, $b){
        echo "a = $a, b = $b";
    }

## 循环

    for($i=0; $i<100; $i++){
        echo "Hello".$i.'<br>';
    }
    while($i<100){

    }

## 常用功能

### 字符串

    $str = 'Hello PHP';
    strpos($str, 'o'); // 查找o所在的位置
    $str1 = substr($str, 2, 3); // substr(string, begin[, length]);
    $result = str_split($str);  // str_split($str[, split_length:int=1]), 可以指定分割长度
    print_r($result) // 输出数组
    $result = explode(' ', $str); // 以空格来分割字符串
    $str = implode('-', $result); // 以 - 来连接数组
    $str2 = "$str<br>Object-C $num";

### 数组

    $arr = array(0=>'jike',1=>'iwen',h'=>'Hello', 'w'=>'World', 'name'=>'guodong');
    $arr[1] = 'Hello';
    print_r($arr);
    array_***函数
    array_push($arr, 'Item');
    $arr['h'] = 'Hello';

### include与require

    include：若文件不存在，则给出警告。提示程序 包含 该文件
    include_once();
    require：若文件不存在，则给出错误。提示程序 依赖于 该文件
        每一次使用时都会 执行 引入的文件。
    require_once();
        对于重复引入的文件，只会执行一次。


## 面向对象相关特性(from PHP5)

### 类

    // jkxy/hello.php
    namespace jkxy;
    class Hello{
        public function sayHello(){
            echo 'Hello jkxy';
        }
    }
    class Desktop{
        public fuction work(){
            echo 'Work';
        }
    }

    // jikexueyuan/hello.php
    namespace jikexueyuan;
    class Hello{
        public function sayHello(){
            echo 'Hello jikexueyuan'
        }
    }

    //index.php
    require_once('jkxy/hello.php');
    require_once('jikexueyuan/hello.php');
    $h = new \jikexueyuan\Hello();
    $h->sayHello();
    $h = new \jkxy\Hello();
    $h->sayHello();

### 构造方法

    // man.php
    class Man{
        /**
        *   @param int $age 这个人的年龄
        *   @param string $name 这个人的名字
        */
        public function __construct($age, $name){
            echo 'Construct a man';
            $this->_age = $age;
            $this->_name = $name;
        }
        public function getAge(){
            return $this->_age;
        }
        public function getName(){
            return $this->_name;
        }

        private $_age, $_name;
    }

    // index.php
    require_once('man.php');

### 成员方法和类方法

    // man.php
    class Man{
        /**
        *   @param int $age 这个人的年龄
        *   @param string $name 这个人的名字
        */
        public function __construct($age, $name){
            echo 'Construct a man';
            $this->_age = $age;
            $this->_name = $name;
            Man::$NUM++;
            if(Man::$NUM > Man::MAX_MAN_NUM){
                echo "Too much man";
            }
        }
        public function getAge(){
            return $this->_age;
        }
        public function getName(){
            return $this->_name;
        }
        private $_age, $_name;
        private static $NUM = 0; //类变量
        const MAX_MAN_NUM = 200;
        // 静态方法
        public static function sayHello(){
            echo 'Hello Man';
        }
    }

    // index.php
    require_once('man.php');


### 继承与重载

    // People.php
    class People{
        /**
        *   @param int $age 这个人的年龄
        *   @param string $name 这个人的名字
        *   @param string $sex
        */
        public function __construct($age, $name, &sex){
            echo 'Construct a man';
            $this->_age = $age;
            $this->_name = $name;
            $this->_sex = $sex;
            People::$NUM++;
            if(People::$NUM > People::MAX_MAN_NUM){
                echo "Too much man";
            }
        }
        public function getAge(){
            return $this->_age;
        }
        public function getName(){
            return $this->_name;
        }
        public function getSex(){
            return $this->_sex;
        }
        public function hi(){
            echo 'hi'.$this->_name;
        }
        private $_age, $_name, $_sex;
        private static $NUM = 0; //类变量
        const MAX_MAN_NUM = 200;
        // 静态方法
        public static function sayHello(){
            echo 'Hello Man';
        }
    }
    // Man.php
    require_once('People');
    class Man extends People{
        public function __construct(&age, &name){ // 继承
            parent::__construct($age, $name, '男')
        }
        public function hi(){
            // parent::hi(); // 保留父类的方法
            echo 'Man '.$this->getName().'Say hi'; // 方法重写
        }
    }

    // index.php
    require_once('Man.php');
    $m = new Man(10, 'jike');


## 常用库函数

### 时间与日期

    // 时间戳
    time(); // 1970年开始的毫秒数。
    data_default_timezone_get(); //获取时区
    date_default_timezone_set('Asia/Shanghai'); //设置时区
    echo data('Y-m-d H-i-s');
    echo data('Y-m-d H-i-s', time()); //呈现time()对应的时间戳的当地时间。
    echo data('Y-m-d H-i-s', '2000'); //将时间戳转换为日期。


### JSON格式数据的操作

    // JSON format
    $arr = Array(1,2,5,8,'Hello', Array('h'=>'hello', 'name'=>'jike'));
    echo json_encode($arr); // 将对象转为json类型的数据
    $jsonStr = '{"h":"hello", "w":"world"}';
    $obj = json_decode($jsonStr);
    echo $obj->h; // 可以通过 $obj->h 来访问 h 属性
    $arr=json_decode($json_string,true);//将第二个参数为true时将转化为数组

#### json跨域的数据调用:

    例如：主调文件index.html
    <script type="text/javascript">
        function getProfile(str) {
                var arr = str;
                document.getElementById('nick').innerHTML = arr.nick;
        }
    </script>
    <body>
          <div id="nick"></div>
    </body>
    <script type="text/javascript" src="http://www.openphp.cn/demo/profile.php"></script>

    例如：被调用文件profile.php
    <?php
    $arr = array(
        'name' => '魏艳辉',
            'nick' => '为梦翱翔',
             'contact' => array(
                     'email' => 'zhuoweida@163.com',
                     'website' => 'http://zhuoweida.blog.tianya.cn',
                )
           );
    $json_string = json_encode($arr);
    echo "getProfile($json_string)";
    ?>

#### 对象的json化

    <?php
    //1.对象
    class JsonTest{
        var $id = 1;
        var $name = 'heiyeluren';
        $gender = '男';
    }
    $obj = new JsonTest;
    echo json_encode($obj)."<br /> ";
    ?>
    浏览器输出结果：
    {
        "id":1,
        "name":"heiyeluren",
        "gender":"\u7537"
    }

#### js如何解析服务器端返回的json字符串？

我们在使用ajax做客户端和服务器端交互的时候，在不适用jquery等框架的前提下，一般的做法是让服务器端返回一段json字符串，然后在客户端将它解析成javascript对象。解析时用到的方法一般是eval或者是new function，而目前ie8和firefox3.1有内置了原生的json对象。

    [php] view plaincopy
    例1：
    var strTest='{"a":"b"}'; 转换成JS对象
    var obj=eval("("+strTest+")") ;
    例2:
    function strtojson(strTest){
        JSON.parse(str);
    }


### 文件操作

    // write file
    $f = fopen('data', 'w');
    if($f){
        fwrite($f, 'Hello');
        fclose($f);
    }else{
        echo 'Error';
    }
    
    // read file
    $f = fopen('data', 'r');
    if($f){
        while(!feof($f)){
            $content = fgets($f);
        }
    }
    fclose($f);

    // file_get_contents('data'); // 直接获取改定文件名称的内容


### 生成图片（GD）

    $img = imagecreate(400, 300);
    imagecolorallocate($img, 255,255,255);
    imageellipse($img, 200, 200, 50, 50, imagecolorallocate($img, 255, 0, 0));
    header('Content-type: image/png'); // 设置php头信息
    imagepng($img); // 生成img图片

### 图片打水印

    $img = imagecreatefromjpeg('img.jpg');
    imagestring($img, 2, 5, 5, 'jikexueyuan.com', imagecolorallocate($img, 0, 255, 0));
    header('Content-type: image/jpeg');
    imagejpeg($img);


## 会话控制

1. Cookie

> 数据存储在浏览器端
> 特点：
>   方便与JavaScript交换数据
>   方便获取用户信息
> 风险：浏览器可能会禁用Cookie
> 替代方案：URL参数

2. Session

> 数据存储在服务器
> 特点：
>   高效
>   安全
>   不依赖浏览器
>   服务器端会为每一个用户用一个 ID 来标识

### Cookie操作

    // 多个页面之间的数据交换
    // index.php
    setcookie('name', 'jikexueyuan'); // 创建Cookie
    header('Location:a.php');
    // a.php
    echo _COOKIE['name']; // 获得Cookie

    // 与JS程序做数据共享
    // index.php
    setcookie('name', 'jikexueyuan');
    setcookie('h', 'hello');
    <script>
        alert(document.cookie); // "name=jikexueyuan; h=hello" #每一个cookie之间使用 ; 标记
    </script>

    // Cookie被禁用,使用URL参数替代
    // b.php
    header("Location:c.php?name=jikexueyuan")
    // c.php
    echo $_GET['name']


### Session操作

    // index.php
    session_start(); // 启用session
    echo session_id();
    $_SESSION['name'] = 'jikexueyuan'; // 创建session
    unset($_SESSION['name']); // 立即清除session
    unset($_SESSION); // 立即清除全部session
    session_destroy(); // 清除Session
    header("Location:a.php");

    // a.php
    session_start();
    if(isset(_SESSION['name'])){
        echo _SESSION['name'];
    }else{
        echo 'No name found';
    }


## PDO对象的认识与应用

### PDO对象认识

    方便于系统移植，例如系统更换数据库。

2. PDO对象初始化

    try{
        $pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");
        $pdo = new PDO("uri:mysqlPdo.ini", "root", ""); // 通过配置文件
        $pdo = new PDO("mysqlpdo", "root", ""); // 通过php.ini配置
        $pdo->setAttribute(PDO::ATTR_AUTOCOMMIT, 0); // 设置属性为非自动提交。
    }catch(PDOException $e){
        echo "Error:".$e->getMessage();
        die();
    }
        echo $pdo->getAttribute(PDO::ATTR_AUTOCOMMIT);

3. PDO对象应用

成员方法：

    query($sql);
    exec($sql);
    setAttribute();
    fetchAll();

    try{
        $pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");
    }catch(PDOException $e){
        die("Error".$e->getMessage());
    }
    $sql = "select * from stu";
    $stmt = $pdo->query($sql);
    $list = $stmt->fetchAll();
    // $list = $stmt->fetchAll(PDO::FETCH_ASSOC); //设置返回格式为 关联数组
    // 解析数据
    foreach ($list as $val){
        echo $val['id']."----".$val['name']."<br>";
    }
    // 释放资源
    $stmt = null;
    $pdo = null;

    // 案例2---快捷操作方式
    foreach($pdo->query($sql) as $val){
        echo $val['id']."---".$val['name']."<br>";
    }

    // 插入语句
    $sql = "insert into stu values(null, 'oracle', 'w', 44)";
    $res = $pdo->exec($sql); // 返回影响行数
    if($res){
        echo "success!";
    }

    // 删除语句
    $sql = "delete from stu where id=6";
    $res = $pdo->exec($sql); // 返回影响行数
    if($res){
        echo "success!";
    }

    // 修改语句
    $sql = "update stu set name='js' where id=3";
    $res = $pdo->exec($sql); // 返回影响行数
    if($res){
        echo "success!";
    }
