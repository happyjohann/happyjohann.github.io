---
layout: post
title: 微信用户网页授权页面PHP后台设计
category: Weixin
tags: PHP 中文
date: 2016-02-24
---

前前后后，从没有完整写过PHP程序开始，边写边学这个“微”项目，算是花了一个月的时间吧。
<!--more-->

因为写的是微信，老外对微信也并没有太大兴趣，所以这篇和其它所有主要微信文章的一样，还是用中文吧，虽然讲的是代码逻辑结构问题。

今天是开学第四天，没有课。所以从吃过午饭开始，除去写通过调用微信JS SDK获取GPS坐标、实现各种坐标转换、然后调用腾讯地图和百度地图API获取地址以及晚餐时间的一两个小时，算算到开始写这篇文章不觉又在这个项目上花了六七个小时。


## 关于代码逻辑图
整理这个项目，一是对过去一个月的代码更合理的重构一下，另一个方面也是为找工作做点准备。
折腾最久的算是，想通过PHP的代码直接生成函数与函数、函数与类、类与类等等之间的逻辑关系图吧。在Google上找到了一些工具，只是遗憾的事最终还是没能成功的实现。

其实，找也算是幸运的找到了需要的工具。

Code Graph: Generate call graphs of PHP code with GraphViz  
示例代码输出图  
![Code Graph Example](/assets/images/wx-authentication/codegraphexample.jpg)  

图例  
![Code Graph Key](/assets/images/wx-authentication/codegraphkey.jpg)  

然而不幸的是，示例程序运行起来不错，但当用到我的项目中去的时候却完全没了输出。或许对1300多行代码以及超越了Demo工具的能力范围吧。

Stackoverflow上有人推荐还PHP_UML-1.6.2，只是在Mac上折腾许久始终因为SQLite相关问题没能成功安装，然后还有BOUML只是付费的东西暂时还是消费不起而去并没能找到不付费的办法，最后算是唯一可以在Mac上用成功的也就是生成UML类的phUML了（我的“微”项目生产图如下），只不过并不是我需要的东西。

我的“微”项目UML类图  
![Code Graph Key](/assets/images/wx-authentication/umlclass.png)

虽然生成的UML类图明显有错误而却毫无相互关系，不过对于我来讲述这个“微”项目还是有挺大帮助，毕竟有胜于无。

给出了类图，也顺便给出精减的代码框架，毕竟相比于具体的代码实现，代码逻辑关系是更为核心的部分。虽然最终没能找到合适的工具生成代码逻辑关系图，所以也就只能给出并不太直观的代码框架了。

```php
<?php 
/* Path and URI Configuration Go First */

define('ROOT', '');
define('PATH_TO_ASSETS', '');

define('ROOT_URI', '');
define('URI_TO_ASSETS', '');

/* DB Configuration Comes the Second */
define('DB_DSN', '');
define('DB_USER', '');
define('DB_PASS', '');

try {$db = new  PDO(DB_DSN, DB_USER, DB_PASS);} 
catch (PDOException $e) {die("Error!:".$e->getMessage().'<br>');}

/* And the third Part: Weixin Identification */
$wxRawId = '';
$wxAppId = '';
$wxAppSecret = '';

/* 引用的Github的代码，实现IP地址到地理位置信息的映射. Author:@york */
class IpLocationSeekerBinary {
    public function seek($ip_address) {}
}

/* A class for abstract user information from HTTP request. Author: Johann Huang */
class UserRequest {
    public function getVisitTime() {}
    public function getIpList() {}
    public function getRealIp() {}
    public function getUserAgent() {}
    public function getAcceptLanguage() {}
    private function getXForwordIp() {}
    private function getClinetIp() {}
    private function getRemoteAddress() {}
}

/* A class for check web client. Author: Johann Huang */
class UserAgent {
    public function isWeixin() {}
    public function getWeixinVersion() {}
    public function getDevice() {}
    public function getMobileOS() {}
    public function getDesktopOS() {}
    public function getBrowser() {}
}

/* Generate Bicode. Author: Johann Huang */
class Bicode {
  /* Using Wwei Api */
  public function getWweiBicode($data='') {}
  /* In fact, this function can be assgined to a specialized Class*/
  private function httpGet($url) {}
}

/* A Class for maning data in MySQL database. Author: Johann Huang*/
class DbHelper {
    public function close() {}
    public function query($theSql) {}
    public function exec($theSql) {}
    public function createTableWithArray($theTable , $fieldArray) {}
    public function selectWithArray($theTable, $whereArray) {}
    public function selectWithArrays($theTable, $selectArray, $whereArray) {}
    public function insertWithArray($theTable, $insertArray) {}
    public function updateWithArrays($theTable, $fieldArray, $whereArray) {}
    public function updateOrInsertWithArrays($theTable, $fieldArray, $whereArray) {}
}

/* This is a class for clear output of PHP. */
class OutputUtility {
    // print_r() with htmlEncode() can do the same thing. Ref: http://php.net/manual/zh/function.print-r.php
    public function outputArray($theArray, $prefix = '') {}
    public function htmlEncode($theString) {}
    public function htmlDencode($theString) {}
}

/* A Class for maning data in MySQL database. Author: Johann Huang */
class PvRecord {
  public function close() {}
  public function query($theSql) {}
  public function recordPageView($pvFrom = '', $pvBy = '') {}
}

/* This is a class for getting Weixin user information. Author: Johann Huang */
class WxUser {
    public function setScope($theScope) {}
    /* App-Level Function. For getting app-level access_token.*/
    public function setDb(&$theDb) {}
    public function getUserInfo() {
        //...
        $url = "https://api.weixin.qq.com/sns/userinfo?access_token=$access_token&openid=$openid&lang=$lang";
        $res = json_decode($this->httpGet($url), true);
        //...
    }
    private function getTokenAndId($scope='snsapi_base') {
        //...
        $url = "https://api.weixin.qq.com/sns/oauth2/access_token?appid=$this->appId&secret=$this->appSecret&code=$code&grant_type=$grant_type";
        //...
    }
    private function getCode($scope='snsapi_base') {
        if(!empty($_GET["code"])) {} 
        else {
            header("Location: https://open.weixin.qq.com/connect/oauth2/authorize?appid=$this->appId&redirect_uri=$redirect_uri&response_type=$response_type&scope=$scope&state=$state#$wechat_redirect");
            exit(0);
        }
    }
    private function verifyAccessToken($theAccessToken, $theOpenId) {
        //...
        $url = "https://api.weixin.qq.com/sns/auth?access_token=$access_token&openid=$openid";
        $res = json_decode($this->httpGet($url));
        //...
    }
    private function httpGet($url) {}
    public function getUserInfoWithAT($openid = '') {
        //...
        $url = "https://api.weixin.qq.com/cgi-bin/user/info?access_token=$access_token&openid=$openid&lang=$lang";
        $res = json_decode($this->httpGet($url), true);
        //...
    }
    private function getAccessToken() {
        //...
        $url = "https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=$this->appId&secret=$this->appSecret";
        $res = json_decode($this->httpGet($url));
        //...
    }
}

/* The Web Api Part of Recording Visitor PageView. Author: Johann Huang */
/* Use Whether there is _POST[] parameter to tell whether it is a api visit */
if(!empty($_POST[''])) {exit(0);}

function renderBicodePage() {}
function renderContentPage() {}
if(strpos($_SERVER['HTTP_USER_AGENT'], 'MicroMessenger') === false) {
    renderBicodePage();
} else {
    $wxUser = new WxUser($wxAppId, $wxAppSecret);
    $userInfoPackage = $wxUser->getUserInfo();
    renderContentPage();
}

$pvRecord = new PvRecord($db, $pvTable);
$pvRecord->recordPageView($pvFrom, $pvBy);
?>
```

## 关于类
从UML类图和代码框架，可以看出抽象出来的类一共8个，除去在Github上借用的`IpLocationSeekerBinary`类，辅助调试用类`OutputUtility`，我一共写了6个类。然后，细看可以发现，`UserAgent`算是更加具体化的`UserRequest`类，所以实际上`UserAgent`可以背封装到`UserRequest`类中，虽然因为简捷化处理没有这样实现。所以实际上需要讨论的类一共5个。

个人感觉，除了`WxUser`类比较复杂以外，其它类抽象的还挺好的所以功能及实现也算是一目了然。

先从简单的一个一个说吧。

`DbHelper`类:  
和名字暗示的一样，是简化PHP数据库操作的类，采用的是PDO实现方式。简单来说，DbHelp类起到了PHP的数组和MySQL操作的中间层，不用再费心构建SQL语句。

`UserRequest`类和`UserAgent`类:
也算是两个工具类，使得从HTTP Request中取相关数据变得容易。

`BiCode`类:
用于生成调用页面的二维码。

`PvRecord`类:
PageView访问记录类，在其方法中实例化并调用了`UserRequest`类，`UserAgent`类，`IpLocationSeekerBinary`类以及`DbHelper`类。

`WxUser`类:
是一个很复杂的类，主要是涉及到了需要进行页面跳转到腾讯服务器进行用户授权，而且坑爹的事情是，一次向腾讯服务器的跳转请求将会迎来腾讯服务器两次回击请求（两次带code的回应请求）。鉴于复杂性，所以暂时不在这里讨论，后文详述。

## 关于微信用户授权两套方案
关于微信用户授权的实现方案，我之前考虑过两种方案。

一是类似于“代理服务器”的方案。所有的用户访问先通过用户授权脚本，然后用户完成授权后，由授权脚本讲强求转发给用户实际请求的页面。不过貌似这种方案需要进行服务器配置，而配置服务器这样的事情有时候会非常令人抓狂，比较我到现在还是用的免费服务器-_-（其实我也是想买的，然而并没有信用卡，同时并不想被备案审查）。

二是现在采用的方案，称作“组件式”方案或者“一页”式方案吧。  
在谈微信授权之前先看看非类定义代码部分的逻辑结构。

## 页面逻辑部分
从上面的代码框架可以看出除了类以外的，其实就是一个POST请求接收处理部分、分是微信浏览器，还是非微信浏览器去生成两种不同的页面部分、和记录PageView部分共三个部分。以下分别介绍。

### POST请求处理部分
```php
<?php
/* The Web Api Part of Recording Visitor PageView. Author: Johann Huang */
/* Use Whether there is _POST[] parameter to tell whether it is a api visit */
if(!empty($_POST[''])) {exit(0);}
?>
```

逻辑很简单，也就是如果请求中带有某写POST参数，那么当作POST数据请求而非页面内容请求，所以执行完数据处理，立刻结束脚本。

### 浏览器分伺部分
```php
<?php
function renderBicodePage() {}
function renderContentPage() {}
if(strpos($_SERVER['HTTP_USER_AGENT'], 'MicroMessenger') === false) {
    renderBicodePage();
} else {
    $wxUser = new WxUser($wxAppId, $wxAppSecret);
    $userInfoPackage = $wxUser->getUserInfo();
    renderContentPage();
}
?>
```

简单说来，也就是通过UserAgent中的信息判断浏览器类型，然后分别生成不同页面。对于非微信浏览器，通过调用BiCode类生成用户请求的当前页面URL的二维码，然后显示给用户并提示用微信扫码；对于微信浏览器，简单来看是先点用WxUser组件获取用户授权，然后生成相应的页面。

不过由于授权过程的复杂性，所以其实背后的逻辑不是那么直接。后文来详述。

### PageView纪录部分
```php
<?php
$pvRecord = new PvRecord($db, $pvTable);
$pvRecord->recordPageView($pvFrom, $pvBy);
?>
```

鉴于PageView服务器端能做的事情比较局限而去实现代码被封装在PvRecord类，所以页面逻辑部分只需要驱动实例化PvRecord类并驱动其方法。（当然，如果你仔细思考过页面第一部分POST请求的存在，你会发现其实前端还有些东西的，不过这篇文章不涉及前端的东西，所以与前端相关的后台部分也就不怎么说了。）

## 微信用户授权取信息部分
```php
<?php
/* This is a class for getting Weixin user information. Author: Johann Huang */
class WxUser {
    public function setScope($theScope) {}
    public function getUserInfo() {
        //...
        $url = "https://api.weixin.qq.com/sns/userinfo?access_token=$access_token&openid=$openid&lang=$lang";
        $res = json_decode($this->httpGet($url), true);
        //...
    }
    private function getTokenAndId($scope='snsapi_base') {
        //...
        $url = "https://api.weixin.qq.com/sns/oauth2/access_token?appid=$this->appId&secret=$this->appSecret&code=$code&grant_type=$grant_type";
        //...
    }
    private function getCode($scope='snsapi_base') {
        if(!empty($_GET["code"])) {} 
        else {
            header("Location: https://open.weixin.qq.com/connect/oauth2/authorize?appid=$this->appId&redirect_uri=$redirect_uri&response_type=$response_type&scope=$scope&state=$state#$wechat_redirect");
            exit(0);
        }
    }
    private function verifyAccessToken($theAccessToken, $theOpenId) {
        //...
        $url = "https://api.weixin.qq.com/sns/auth?access_token=$access_token&openid=$openid";
        $res = json_decode($this->httpGet($url));
        //...
    }
    private function httpGet($url) {}
}
?>
```

有心人会发现，这部分代码是我唯一留了相对较多细节的部分。（当然，其实也没怎么留细节）
如果页面不回发生跳转，仅仅是调用API，那么问题也是很简单的。（不过由于个人还有其它想法也在这里面实现，所以实际上这篇文章讲的东西，复杂度是下降了一个维度的。）

言归正传，因为从`getUserInfo`到`getTokenAndId`再到`getCode`之后，`getCode`倒数第二是跳转到授权页面并且授权结束后还要回到当前页面（此时又是新的Request）。所以必须有一个机制去证明授权后的Requst是不用再跳转的请求，否则久发生无休无止的循环了。当然，这里最好的方法就是看有URI中有没有code，有code就可以认为是授权后的访问。（坑爹的腾讯会两次回击，嗯，这里还是不讨论如何解决这个问题）

嗯，简化过后的WxUser类貌似又不怎么难了。好吧，那就简单点儿吧。

不简单的问题就在于想在服务器端定制URL，毕竟带着code的URL并不适合被用户去分享。前端定制URL，一是可以借助WX JS SDK；二就是前端需要刷新页面的，当然用POST请求是一种方式，但总感觉并不逻辑上干净，而且刷新页面是很不友好的。

## 扯下蛋
然后总感觉，这里需要讲讲变量的生命周期问题，然后还有`$_SESSION[]`这样的东西其实还有有些意思的。不过还是算了吧。其实比较不信任的是用前端的Javascript去做有关后台关键动作的，比如决定某`$_SESSION[]`是否释放，毕竟网络问题和浏览器对Javascript的支持一直都没有100%这种说法。谈这个是，因为确实纠结过关于SESSION中保持的值是否释放的问题。现在也没有弄明白SESSION的生命周期，反正和浏览器关不关闭没什么关系，而且同一个浏览器不同几次的访问session_id()居然是一样的。所以在关于session_id()如何生成，还有session_id()能不能唯一标识用户方面我还在确认中。不管算是一个临时的解决方案吧。  

之前在网上看貌似唯一标识用户可以用帆布指纹识别Fingerprintjs，但又看了几篇评测貌似并不可以。毕竟两台崭新的同型的iPhone 7让你选一个更好的，你作为一个高智商的灵长类动物也分辨不了吧。  

写罢，收手。 
本来准备用Coggle.io去画微信开发关系图，也暂且放放吧。  
主要是有个远方的小伙伴儿需要帮忙看C++语言代码了...（要不是C++=D，我才不看呢）  

