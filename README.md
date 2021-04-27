#APICloud接入

#**一、概述**

Xinstall支持APICloud平台的模块接入，你可以在 [APICloud模块store](https://www.apicloud.com/mod_detail/123029) 中找到Xinstall模块，让你应用快速集成Xinstall 模块。

本模块封装了 Xinstall 官方SDK，是集智能传参、快速安装、一键拉起、客户来源统计等功能，帮您提高拉新转化率、安装率和多元化精确统计渠道效果的产品。为用户提供点击、安装、注册、留存、活跃等等精准统计报表，并可实时排重，杜绝渠道流量猫腻，大大降低运营成本。
具体详细介绍可前往 [Xinstall 官网](https://xinstall.com/) 进行查看。


<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step0.png" />

# **二、如何接入**

## 1、添加Xinstall模块

在 APICloud 控制台中，找到需要接入的 APICloud 应用，点击【模块】按钮，再点击【模块库】，搜索 “xinstall”，找到模块后，点击模块右上角添加，将模块集成进入该 App（点击后该按钮变为灰色的已添加字样）：
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step1.png" />

## 2、创建Xinstall应用

进入 [Xinstall 官网](https://xinstall.com/) 注册账号，并在控制台中创建一个对应的应用，应用名字可以任意填写：
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step2.png" />
注意记录 Xinstall 中新创建应用的 appkey（后续配置需要用到）：
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step3.png" />

接入过程中如有任何疑问或者困难，可以随时联系 [Xinstall 官方客服](https://xinstall.com/) 在线解决。



## **3、初始化配置**
** 进入 APICloud 应用的工程，配置工程目录下的 config.xml 文件，配置完毕，必须通过云编译生效，配置方法如下：**

参数：urlScheme、appKey

###**3.1、配置 config.xml 文件**
**配置示例：**
```xml
<permission name="internet" />
<preference name="urlScheme" value="将这里替换成 Xinstall 官方自动分配的 scheme" />
// Android
<meta-data name="com.xinstall.APP_KEY" value="将这里替换成 Xinstall 官方自动分配的 appKey" />
// iOS
<feature name="xinstall">
    <param name="com.xinstall.APP_KEY" value="将这里替换成 Xinstall 官方自动分配的 appKey" />
</feature>
```
**字段介绍：**

* internet：添加网络权限
* urlScheme：如需使用一键拉起功能则必须配置。urlScheme 的 value 值详细获取位置：Xinstall 应用控制台-> Android集成-> 功能集成 中获取
* com.xinstall.APP_KEY：（必须配置）模块接入前准备工作时，从 Xinstall 平台获取的 AppKey

**示例图片（示例图片中开发工具均为 APICloud Studio 3）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step4.png" />


###**3.2、Info.plist 相关配置（针对iOS）**
** 进入 APICloud 应用工程的 res 目录下（该目录与 config.xml 同级）新增 Info.plist 文件，配置完毕，必须通过云编译生效，配置方法如下： **

**配置示例：**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>com.xinstall.APP_KEY</key>
  <string>Xinstall 分配给应用的 appkey</string>
</dict>
</plist>
```

**示例图片：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step5.png" />

** 补充说明 **

如果你的工程中已经在 res 中添加了 Info.plist 文件，则直接在文件的<dist></dict>之间添加如下键值对即可：
```xml
  <key>com.xinstall.APP_KEY</key>
  <string>Xinstall 分配给应用的 appkey</string>
```

###**3.3、Universal links 相关配置（针对 iOS）**
开启 Associated Domains 服务

对于 iOS，为确保能正常使用一键拉起功能，AppID 必须开启 Associated Domains 功能，请到苹果开发者网站，选择 “Certificate, Identifiers & Profiles”，选择 iOS 对应的 AppID，开启 Associated Domains：
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step6.png" />


注意：当 AppID 重新编辑过之后，需要更新相应的 mobileprovision 证书。(图文配置步骤请看 iOS 集成指南)。更新 mobileprovision 证书步骤请查看云编译 mobileprovision 证书制作 中的 "云编译 mobileprovision 发布证书制作"或"云编译 mobileprovision 测试证书制作"。

**配置 Universal links 关联域名**

关联域名(Associated Domains)的值请在 Xinstall 控制台获取（Xinstall 应用控制台 -> iOS集成 -> 功能集成）

该文件是给 iOS 平台配置的文件，在网页工程的 res 目录下（该目录与 config.xml 同级）下创建文件名为 UZApp.entitlements 的文件，UZApp.entitlements 内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.associated-domains</key><!--固定key值-->
    <array>
     <!--这里换成你在 xinstall 后台的关联域名(Associated Domains)-->
        <string>applinks:xxxxxx.xinstall.top</string>
    </array>
</dict>
</plist>
```

**示例图片：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step7.png" />

##**4、导出包上传 Xinstall**

代码集成完毕后，需要通过云编译导出 iOS 和 Android 安装包，并上传 Xinstall 控制台里对应的 App：

**示例图片（iOS 端）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step8.png" />

**示例图片（Android 端）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step9.png" />

上传完包后，需要进入 「iOS应用适配」和「Android应用配置」中选择下载的包的版本

**示例图片（iOS 端）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step10.png" />
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step11.png" />

**示例图片（Android 端）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step12.png" />
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step13.png" />

> 注意：每次上传完新的 ipa 或者 apk 包后，均需要进入「iOS应用适配」和「Android应用配置」中重新选择下载的包的版本


# **三、如何使用**

### 1、快速下载和一键拉起

如果只需要快速下载功能和一键拉起，无需其它功能（携带参数安装、渠道统计），完成初始化配置即可。其他影响因素如下图
![](https://xinstall-static-pro.oss-cn-hangzhou.aliyuncs.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/xinstall_yjlqksaztj.png)

### 2、携带参数安装/唤起

> 注意：调用该功能对应接口时需要在 Xinstall 中为对应 App 开通专业版服务

在 APP 需要安装参数时（由 web 网页中传递过来的，如邀请码、游戏房间号等动态参数），调用此接口，在回调中获取web中传递过来的参数，参数在App被一键唤起（拉起），或在快速下载第一次打开应用时候，会传递过来，App端可以分别从`addWakeUpEventListener `和`addInstallEventListener `两个方法中进行获取

## **addWakeUpEventListener**

添加唤醒页面事件监听者。在 JS 中获取到 api 对象（由 APICloud 官方提供）的 appintent 事件回调后，可通过该方法添加监听者。监听回调函数里可保存唤醒参数供后续业务使用。

**示例代码**

addWakeUpEventListener({uri:ret}, callback(ret, err))

**入参说明**：{uri:ret} 为 appintent 事件回调数据

**回调说明**：传入监听回调 callback(ret, err)

ret:

  类型：JSON对象

内部字段：

```
{
    channelCode: '渠道编号',//渠道编号
    data: '唤醒携带的参数'  //有携带参数，则返回数据，没有则为空
}
```

**调用示例**

```javascript
// 请严格遵循如下调用顺序，否则可能导致获取不到唤醒参数
var xinstall = api.require('xinstall');
api.addEventListener({
  name:'appintent'
},function(ret,err){
  xinstall.addWakeUpEventListener({'uri': ret}, function(ret, err) { // 唤醒参数
  // 回调函数将在合适的时机被调用，这里编写拿到渠道编号以及携带参数后的业务逻辑代码
  var channelCode = ret.channelCode;
  var data = ret.data;
  // ....
  });
});
```

**补充说明**

此方法用于获取动态唤醒参数，通过动态参数，在拉起APP时，获取由web网页中传递过来的，如邀请码、游戏房间号等自定义参数，通过注册监听后，获取web端传过来的自定义参数。请严格遵循示例中的调用顺序，否则可能导致获取不到唤醒参数。

**可用性**

Android系统，iOS系统

可提供的 1.1.0 及更高版本

## **addInstallEventListener**

添加携带参数安装事件监听者。在 App 启动时（入口程序处，**一般为 apiready 中**）需要通过该方法添加监听者。监听回调函数里可保存安装参数供后续业务使用。

**示例代码**

addInstallEventListener({}, callback(ret, err))

**入参说明**：无需主动传入参数，直接传入 {} 即可

**回调说明**：传入监听回调 callback(ret, err)

ret:

  类型：JSON对象

内部字段：

```
{
    channelCode: '渠道编号',//渠道编号
    data:    '个性化安装携带的参数'
}
```

**调用示例**

```javascript
var xinstall = api.require('xinstall');
xinstall.addInstallEventListener({}, function(ret, err) {
  // 回调函数将在合适的时机被调用，这里编写拿到渠道编号以及携带参数后的业务逻辑代码
  var channelCode = ret.channelCode;
  var data = ret.data;
  // ....
});
```

**补充说明**

此接口用于获取动态安装参数，测试时候建议卸载再安装正确获取参数，在APP需要个性化安装参数时（由web网页中传递过来的，如邀请码、游戏房间号等自定义参数），在回调中获取参数，可实现跳转指定页面、统计渠道数据等。**调用该函数的时机建议越早越好，尽量在程序启动时的 apiready 内进行注册，以免错过回调时机。**

**可用性**

Android系统，iOS系统

可提供的 1.1.0 及更高版本


##**4、渠道统计 **

> 注意：调用该功能对应接口时需要在 Xinstall 中为对应 App 开通专业版服务

###**4.1、注册量统计 **

在业务中合适的时机（一般指用户注册）调用指定方法上报注册量

## **reportRegister**

**示例代码**

addWakeUpEventListener()

**入参说明**：无需主动传入参数

**回调说明**：无需传入回调函数

**调用示例**

```javascript
var xinstall = api.require('xinstall');
xinstall.reportRegister();
```

**补充说明**

Xinstall 会自动完成安装量、留存率、活跃量、在线时长等渠道统计数据的上报工作，如需统计每个渠道的注册量（对评估渠道质量很重要），可根据自身的业务规则，在确保用户完成 app 注册的情况下，调用 reportRegister()上报注册量。 在 Xinstall 平台即可看到注册量。

**可用性**

Android系统，iOS系统

可提供的 1.1.0 及更高版本


### **4.2、事件统计**

事件统计，主要用来统计终端用户对于某些特殊业务的使用效果，如充值金额，分享次数，广告浏览次数等等。

调用接口前，需要先进入 Xinstall 管理后台**事件统计**然后点击新增事件。

## **reportEventPoint**

**示例代码**

reportEventPoint({params})

**入参说明**：需要主动传入参数，JSON对象

入参内部字段：

eventId： 类型：字符串 描述：效果点ID

eventValue： 类型：数字类型 描述：效果点值，货币以分为单位

**回调说明**：无需传入回调函数

**调用示例**

```javascript
var xinstall = api.require('xinstall');
xinstall.reportEventPoint({
  eventId:'payMoney',
  eventValue:13
});
```

**补充说明**

只有 Xinstall 后台创建事件统计，并且代码中传递的事件ID与后台创建的ID一致时，上报数据才会被统计。

**可用性**

Android系统，iOS系统

可提供的 1.1.4 及更高版本

# **四、如何测试功能**

参考官方文档 [测试集成效果](https://doc.xinstall.com/integrationGuide/comfirm.html)

# **五、更多 Xinstall 进阶功能**
若您想要自定义下载页面，或者查看数据报表等进阶功能，请移步 [Xinstall 官网](https://xinstall.com/) 查看对应文档。

若您在集成过程中如有任何疑问或者困难，可以随时联系 [Xinstall 官方客服](https://url.cn/q0z85YeZ?_type=wpa&qidian=true) 在线解决。
