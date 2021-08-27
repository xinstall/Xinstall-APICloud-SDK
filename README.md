
#APICloud接入

> 【重要说明】：从 v1.5.0 版本（含）开始，调用  Xinstall 模块的任意方法前，必须先调用一次初始化方法（init 或者 initWithAd），否则将导致其他方法无法正常调用。
>
> 从 v1.5.0 以下升级到 v1.5.0 以上版本后，需要自行修改代码调用初始化方法，Xinstall 模块无法在升级后自动兼容。

#**一、概述**

Xinstall 支持 APICloud 平台的模块接入，你可以在 [APICloud 模块Store](https://www.apicloud.com/modulestore) 中找到 Xinstall 模块，让你应用快速集成Xinstall 模块。

本模块封装了 Xinstall 官方 SDK，是集智能传参、快速安装、一键拉起、客户来源统计等功能，帮您提高拉新转化率、安装率和多元化精确统计渠道效果的产品。为用户提供点击、安装、注册、留存、活跃等等精准统计报表，并可实时排重，杜绝渠道流量猫腻，大大降低运营成本。
具体详细介绍可前往 [Xinstall 官网](https://xinstall.com/) 进行查看。

<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.5.0/introduce.png" />



# **二、如何接入**

## 1、添加 Xinstall 模块

在 APICloud 控制台中，找到需要接入的 APICloud 应用，点击【模块】按钮，再点击【模块库】，搜索 “xinstall”，找到模块后，点击模块右上角【+】按钮，将模块集成进入该 App（点击后该按钮变为灰色的已添加字样）：
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step1.png" />



## 2、创建 Xinstall 应用

进入 [Xinstall 官网](https://xinstall.com/) 注册账号，并在控制台中创建一个对应的应用，应用名字可以任意填写：
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step2.png" />
注意记录 Xinstall 中新创建应用的 appkey（后续配置需要用到）：
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step3.png" />

接入过程中如有任何疑问或者困难，可以随时联系 [Xinstall 官方客服](https://xinstall.com/) 在线解决。



## 3、初始化配置
**进入 APICloud 应用的工程，配置工程目录下的 config.xml 文件，配置完毕，必须通过云编译生效，配置方法如下：**

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
* urlScheme：如需使用一键拉起功能则必须配置。urlScheme 的 value 值详细获取位置：Xinstall 应用控制台 -> Android下载配置 中获取
* com.xinstall.APP_KEY：必须配置。模块接入前准备工作时，从 Xinstall 平台获取的 AppKey

**示例图片（示例图片中开发工具均为 APICloud Studio 3）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step4.png" />


###3.2、Info.plist 相关配置（针对iOS）
**进入 APICloud 应用工程的 res 目录下（该目录与 config.xml 同级）新增 Info.plist 文件，配置完毕，必须通过云编译生效，配置方法如下： **

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

**补充说明**

如果你的工程中已经在 res 中添加了 Info.plist 文件，则直接在文件的`<dict></dict>`之间添加如下键值对即可：
```xml
  <key>com.xinstall.APP_KEY</key>
  <string>Xinstall 分配给应用的 appkey</string>
```

###3.3、Universal links 相关配置（针对 iOS）
**开启 Associated Domains 服务**

对于 iOS，为确保能正常使用一键拉起功能，AppID 必须开启 Associated Domains 功能，请到苹果开发者网站，选择 “Certificate, Identifiers & Profiles”，再选择 iOS 对应的 AppID，开启 Associated Domains：
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step6.png" />

> 注意：当 AppID 重新编辑过之后，需要更新相应的 mobileprovision 描述文件并下载。下载后，需要在 APICloud 管理后台对应的 App 中，进入左侧菜单栏中的「证书」页面，更新「iOS 证书」中对应环境的描述文件。更新完成后，云编译时才能使用最新的这份描述文件。
>
> 若对制作证书和描述文件的过程有疑问，可以参考 APICloud 的官方文章：[iOS证书及描述文件制作流程](https://docs.apicloud.com/Dev-Guide/iOS-License-Application-Guidance)

**配置 Universal links 关联域名**

关联域名 (Associated Domains) 的值请在 Xinstall 控制台获取（Xinstall 应用控制台 -> iOS下载配置）

该文件是给 iOS 平台配置的文件，在网页工程的 res 目录下（该目录与 config.xml 同级）下创建文件名为 UZApp.entitlements 的文件，UZApp.entitlements 内容如下：

> 注意：一共需要配置2个关联域名

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.associated-domains</key><!--固定key值-->
    <array>
     <!--以下两个关联域名均换成你在 xinstall 后台的关联域名(Associated Domains)-->
        <string>applinks:xxxxxx.xinstall.top</string>
        <string>applinks:xxxxxx.xinstall.net</string>
    </array>
</dict>
</plist>
```

**示例图片：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.2.4/step7.png" />

##**4、导出包上传 Xinstall**

代码集成完毕后，需要通过云编译导出 iOS 和 Android 安装包，并上传 Xinstall 控制台里对应的 App：

**示例图片（iOS 端）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step8.png" />

**示例图片（Android 端）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step9.png" />

上传完包后，需要进入「iOS下载配置」和「Android下载配置」中选择下载的包的版本

**示例图片（iOS 端）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step10.png" />

**示例图片（Android 端）：**
<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/step12.png" />

> 注意：每次上传完新的 ipa 或者 apk 包后，均需要进入「iOS下载配置」和「Android下载配置」中重新选择下载的包的版本




# **三、如何使用**

### 1、快速下载和一键拉起

如果只需要快速下载功能和一键拉起，无需其它功能（携带参数安装、渠道统计），完成初始化配置即可。其他影响因素如下图
![](https://xinstall-static-pro.oss-cn-hangzhou.aliyuncs.com/APICloud%E7%B4%A0%E6%9D%90/v1.1.0/xinstall_yjlqksaztj.png)



### 2、初始化 Xinstall 模块

> 注意：从 v1.5.0 版本开始，在调用 Xinstall 模块的任意方法之前，必须调用一次初始化方法，只需要调用一次即可，不需要反复调用。
>
> v1.5.0 之前的版本会在启动时自动初始化，无需调用，也无法调用。


## **init**

初始化方法。在调用 Xinstall 模块其他方法之前必须调用一次该方法，否则其他方法均无法正常执行。

**示例代码**

init()

**入参说明**：无需主动传入参数

**回调说明**：无需传入回调函数

**调用示例**

```javascript
var xinstall = api.require('xinstall');
xinstall.init();
```

**可用性**

Android系统，iOS系统

可提供的 1.5.0 及更高版本




### 3、携带参数安装/唤起

> 注意：调用该功能对应接口时需要在 Xinstall 中为对应 App 开通专业版服务

在 APP 需要安装参数时（在 web 中下载并安装 App 完成后，由 web 网页中传递过来的，如邀请码、游戏房间号等动态参数），调用 `addInstallEventListener` 接口添加监听，在回调中获取 web 中传递过来的参数。

在 App 需要唤醒参数时（手机已经安装 App 时，在 web 中直接通过 Universal Links / scheme 一键拉起 App），首先在 App 启动时预先调用 `addWakeUpEventListener` 接口添加监听，在回调中获取 web 中传递过来的参数。


## **addInstallEventListener**

添加携带参数安装事件监听者。在 App 启动时（入口程序处，**一般为 apiready 中**）需要通过该方法添加监听者。监听回调函数里可保存安装参数供后续业务使用。

**示例代码**

addInstallEventListener({}, callback(ret, err))

**入参说明**：无需主动传入参数，直接传入 {} 即可

**回调说明**：传入监听回调 callback(ret, err)

ret:

  类型：JSON对象

内部字段：

```json
// 如果没有获取到安装时携带的参数，result 为一个空 json 对象：
{}

// 获取到了安装时携带的参数，result 为 json 对象，内部字段为：
{
    "channelCode":"渠道编号",  // 字符串类型。渠道编号，没有渠道编号时为 ""
    "data":{                                    // 对象类型。安装时携带的参数。
        "co":{                              // co 为唤醒页面中通过 Xinstall Web SDK 中的点击按钮传递的数据，key & value 均可自定义，key & value 数量不限制
            "自定义key1":"自定义value1", 
            "自定义key2":"自定义value2"
        },
        "uo":{                              // uo 为唤醒页面 URL 中 ? 后面携带的标准 GET 参数，key & value 均可自定义，key & value 数量不限制
            "自定义key1":"自定义value1",
            "自定义key2":"自定义value2"
        }
    },
    "timeSpan": 12, // 数字类型。代表下载页面上点击开始下载按钮与第一次打开App时的时间间隔，单位为秒
    "isFirstFetch": true // boolean类型。代表是否为第一次获取到安装参数，只有第一次获取到时为 true
}
```

**调用示例**

```javascript
var xinstall = api.require('xinstall');
xinstall.addInstallEventListener({}, function(ret, err) {
  // 回调函数将在合适的时机被调用，这里编写拿到渠道编号以及携带参数后的业务逻辑代码

  // 空对象时代表没有获取到安装参数
  if (JSON.stringify(ret) == '{}') {
    // 业务逻辑
  } else {
    var channelCode = ret.channelCode;
    var data = ret.data;
    var co = data.co;
    var uo = data.uo;
    var timeSpan = ret.timeSpan;
    var isFirstFetch = ret.isFirstFetch;
    // 根据获取到的数据做对应业务逻辑
  }
});
```

**补充说明**

此接口用于获取动态安装参数（由 web 网页中传递过来的，如邀请码、游戏房间号等自定义参数），测试时必须先卸载 App，再安装，才能正确获取参数。在回调中获取到参数后，可实现跳转指定页面、统计渠道数据等场景。**调用该函数的时机建议越早越好，尽量在程序启动时的 apiready 内进行注册，以免错过回调时机。**

> 您可以在 Xinstall 管理后台对应的 App 内，看到所有的传递参数以及参数出现的次数，方便你们做运营统计分析，如通过该报表知道哪些页面或代理带来了最多客户，客户最感兴趣的 App 页面是什么等。具体参数名和值，运营人员可以和技术协商定义，或联系 Xinstall 客服咨询。具体效果如下图：
>
> ![传参报表](https://cdn.xinstall.com/iOS_SDK%E7%B4%A0%E6%9D%90/paramsTable.png)

**可用性**

Android系统，iOS系统

可提供的 1.1.0 及更高版本


## **addWakeUpEventListener**

添加唤醒页面事件监听者。在 JS 中获取到 api 对象（由 APICloud 官方提供）的 appintent 事件回调后，可通过该方法添加监听者。监听回调函数里可保存唤醒参数供后续业务使用。

**示例代码**

addWakeUpEventListener({uri:ret}, callback(ret, err))

**入参说明**：{uri:ret} 为 appintent 事件回调数据

**回调说明**：传入监听回调 callback(ret, err)

ret:

  类型：JSON对象

内部字段：

```json
// 如果唤醒时没有携带任何参数，result 为一个空 json 对象：
{}

// 如果唤醒时有任意参数，result 为 json 对象，内部字段为：
{
    "channelCode":"渠道编号",  // 字符串类型。渠道编号，没有渠道编号时为 ""
    "data":{                                    // 对象类型。唤起时携带的参数。
        "co":{                              // co 为唤醒页面中通过 Xinstall Web SDK 中的点击按钮传递的数据，key & value 均可自定义，key & value 数量不限制
            "自定义key1":"自定义value1", 
            "自定义key2":"自定义value2"
        },
        "uo":{                              // uo 为唤醒页面 URL 中 ? 后面携带的标准 GET 参数，key & value 均可自定义，key & value 数量不限制
            "自定义key1":"自定义value1",
            "自定义key2":"自定义value2"
        }
    }
}
```

**调用示例**

```javascript
// 如果你的应用（安卓端）需要支持在 微信/QQ/QQ浏览器 内通过应用宝拉起，在调用 addWakeUpEventListener 方法前，请先调用一次 openYYBFunction 方法；若不需要支持，则无需调用 openYYBFunction 方法
var xinstall = api.require('xinstall');
xinstall.openYYBFunction({});


// 请严格遵循如下调用顺序，否则可能导致获取不到唤醒参数
var xinstall = api.require('xinstall');
api.addEventListener({
  name:'appintent'
},function(ret,err){
  xinstall.addWakeUpEventListener({'uri': ret}, function(ret, err) { // 唤醒参数
    // 回调函数将在合适的时机被调用，这里编写拿到渠道编号以及携带参数后的业务逻辑代码
    
    // 空对象时代表唤醒了，但是没有任何参数传递进来
    if (JSON.stringify(ret) == '{}') {
      // 业务逻辑
    } else {
      var channelCode = ret.channelCode;
      var data = ret.data;
      var co = data.co;
      var uo = data.uo;
      // 根据获取到的数据做对应业务逻辑
    }
    
  });
});
```

**补充说明**

此方法用于获取动态唤醒参数，在拉起 APP 时，获取由 web 网页中传递过来的，如邀请码、游戏房间号等自定义参数，通过预先注册监听后，在回调中获取 web 端传过来的自定义参数。**请严格遵循示例中的调用顺序，否则可能导致获取不到唤醒参数。**

**可用性**

Android系统，iOS系统

可提供的 1.1.0 及更高版本



### 4、渠道统计


> 注意：调用该功能对应接口时需要在 Xinstall 中为对应 App 开通专业版服务

#### 4.1、注册量统计

在业务中合适的时机（一般指用户注册）调用指定方法上报注册量


## **reportRegister**

**示例代码**

reportRegister()

**入参说明**：无需主动传入参数

**回调说明**：无需传入回调函数

**调用示例**

```javascript
var xinstall = api.require('xinstall');
xinstall.reportRegister();
```

**补充说明**

Xinstall 会自动完成安装量、留存率、活跃量、在线时长等渠道统计数据的上报工作，如需统计每个渠道的注册量（对评估渠道质量很重要），可根据自身的业务规则，在确保用户完成 App 注册的情况下，调用该方法上报后，即可在 Xinstall 平台即可看到注册量。

**可用性**

Android系统，iOS系统

可提供的 1.1.0 及更高版本


#### 4.2、事件统计

事件统计，主要用来统计终端用户对于某些特殊业务的使用效果，如充值金额，分享次数，广告浏览次数等等。

调用接口前，需要先进入 Xinstall 管理后台**事件统计**然后点击新增事件。


## **reportEventPoint**

**示例代码**

reportEventPoint({params})

**入参说明**：需要主动传入参数，JSON对象

入参内部字段：

eventId： 类型：字符串 描述：事件ID

eventValue： 类型：数字类型 描述：事件值，货币以分为单位

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

只有 Xinstall 后台创建事件统计，并且代码中 **传递的事件ID** 与 **后台创建的ID** 一致时，上报数据才会被统计。

**可用性**

Android系统，iOS系统

可提供的 1.1.4 及更高版本



### 5、广告平台渠道功能

>  如果您在 Xinstall 管理后台对应 App 中，**只使用「自建渠道」，而不使用「广告平台渠道」，则无需进行本小节中额外的集成工作**，也能正常使用 Xinstall 提供的其他功能。
>
> 注意：根据目前已有的各大主流广告平台的统计方式，目前 iOS 端和 Android 端均需要用户授权并获取一些设备关键值后才能正常进行 [ 广告平台渠道 ] 的统计，如 IDFA / OAID / GAID 等，对该行为敏感的 App 请慎重使用该功能。

#### 5.1、配置工作

**iOS 端：**

由于 iOS 端目前是通过采集 IDFA 进行广告平台渠道统计，而 IDFA 必须使用 APICloud 官方提供的 「iAd」模块进行获取，故您**必须在 [APICloud 管理后台] - [模块] - [模块库] 中，添加 「iAd」模块**（可以搜索 idfa）：

<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.5.0/step_idfa_1.png" />

添加完成后，在云编译页面内，**填写获取 IDFA 权限的说明文字**：

> 注意：该项若不配置，则会导致 App 启动后闪退！！！

<img src="https://cdn.xinstall.com/APICloud%E7%B4%A0%E6%9D%90/v1.5.0/step_idfa_2.png" />

**Android 端：**

**注：** 相关接入可以参考广告平台联调指南中的[《Android集成指南》](../../AD/AndroidGuide.md)

1. 在APICloud 工程里的的`config.xml`文件中添加（获取IMEI，需要用到该权限）

```xml
<permission name="readPhoneState" />
```

2. 如果使用OAID，因为内部使用反射获取oaid 参数，所以需要外部用户都接入OAID SDK 。具体接入参考[《Android集成指南》](../../AD/AndroidGuide.md)

#### 5.2、更换初始化方法

**使用新的 initWithAd 方法，替代原先的 init 方法来进行模块的初始化**


## **initWithAd**

**入参说明**：需要主动传入参数，JSON对象

入参内部字段：

* iOS 端：
  <table>
         <tr>
             <th>参数名</th>
             <th>参数类型</th>
             <th>描述 </th>
         </tr>
         <tr>
             <th>idfa</th>
             <th>字符串</th>
             <th>iOS 系统中的广告标识符</th>
         </tr>
     </table>


* Android 端：

  <table>
            <tr>
                <th>参数名</th>
                <th>参数类型</th>
                <th>描述 </th>
            </tr>
            <tr>
                <th>adEnabled</th>
                <th>boolean</th>
                <th>是否使用广告功能</th>
            </tr>
    				<tr>
                <th>oaid （可选）</th>
                <th>string</th>
                <th>OAID</th>
            </tr>
    				<tr>
                <th>gaid（可选）</th>
                <th>string</th>
                <th>GaID(google Ad ID)</th>
            </tr>
        </table>

  

**回调说明**：无需传入回调函数

**调用示例**

```javascript
var xinstall = api.require('xinstall');
// 由于 iOS 和 Android 两端需要传入的参数不同，故需要根据平台进行判断，传入不同的参数

if (api.systemType == "ios") {
  // 导入 iAd 模块
  var iAd = api.require('iAd');
  // 先请求 IDFA 权限
  iAd.adRequest(function(ret){
    // 权限请求完成后，开始获取 IDFA
    iAd.getIDFA({}, function(ret) {
      // 传入 IDFA 初始化 Xinstall 模块
      Xinstall.initWithAd({idfa:ret.IDFA})
    });
  });
} else if (api.systemType == "android") {
	requestPremission ( function() {
    requestPremission ( function() {
      // oaid和gaid 为选传，不传则代表使用SDK自动去获取（SDK内不包括OAID SDK，需要自己接入）
        Xinstall.initWithAd({adEnable:true,gaid:"gaid测试",oaid:"oaid测试"});
    });
    // 如果希望在完成初始化，立即执行之后的步骤可以通过 下列代码实现-------------------------
    // Xinstall.initWithAd({adEnable:true,gaid:"gaid测试",oaid:"oaid测试"},function() {
    //  xinstall.addWakeUpEventListener 或者xinstall.addInstallEventListener 等方法                
    //});
    //-----------------------------------------------------------------------------
  });
  
}


// Android IMEI获取权限的判断和申请方法
function requestPremission (callback) {
  var rets = api.hasPermission({
    list: ['phone-r']
  });

  var granted = false;
  if (rets.count > 0) {
    granted = rets[0].granted;
  }
  if (granted) {
    // 有权限
    if (callback) {
      callback();
    }
  } else {
    //  无权限
    api.requestPermission({
      list: ['phone-r'],
      code: 100001
    }, function(ret, err) {
      if (callback) {
        callback();
      }
    });
  }
}
```



**可用性**

Android系统，iOS系统

可提供的 1.5.0 及更高版本



#### 5.3、上架须知

**在使用了广告平台渠道后，若您的 App 需要上架，请认真阅读本段内容。**

##### 5.3.1 iOS 端：上架 App Store

1. 如果您的 App 没有接入苹果广告（即在 App 中显示苹果投放的广告），那么在提交审核时，在广告标识符中，请按照下图勾选：

![IDFA](https://cdn.xinstall.com/iOS_SDK%E7%B4%A0%E6%9D%90/IDFA_7.png)



1. 在 App Store Connect 对应 App —「App隐私」—「数据类型」选项中，需要选择：**“是，我们会从此 App 中收集数据”**：

![AppStore_IDFA_1](https://cdn.xinstall.com/iOS_SDK%E7%B4%A0%E6%9D%90/IDFA_1.png)

在下一步中，勾选「设备 ID」并点击【发布】按钮：

![AppStore_IDFA_2](https://cdn.xinstall.com/iOS_SDK%E7%B4%A0%E6%9D%90/IDFA_2.png)

点击【设置设备 ID】按钮后，在弹出的弹框中，根据实际情况进行勾选：

- 如果您仅仅是接入了 Xinstall 广告平台而使用了 IDFA，那么只需要勾选：**第三方广告**
- 如果您在接入 Xinstall 广告平台之外，还自行使用 IDFA 进行别的用途，那么在勾选 **第三方广告** 后，还需要您根据您的实际使用情况，进行其他选项的勾选

![AppStore_IDFA_3](https://cdn.xinstall.com/iOS_SDK%E7%B4%A0%E6%9D%90/IDFA_3.png)

![AppStore_IDFA_4](https://cdn.xinstall.com/iOS_SDK%E7%B4%A0%E6%9D%90/IDFA_4.png)

勾选完成后点击【下一步】按钮，在 **“从此 App 中收集的设备 ID 是否与用户身份关联？”** 选项中，请根据如下情况进行选择：

- 如果您仅仅是接入了 Xinstall 广告平台而使用了 IDFA，那么选择 **“否，从此 App 中收集的设备 ID 未与用户身份关联”**
- 如果您在接入 Xinstall 广告平台之外，还自行使用 IDFA 进行别的用途，那么请根据您的实际情况选择对应的选项

![AppStore_IDFA_5](https://cdn.xinstall.com/iOS_SDK%E7%B4%A0%E6%9D%90/IDFA_5.png)

最后，在弹出的弹框中，选择 **“是，我们会将设备 ID 用于追踪目的”**，并点击【发布】按钮：

![AppStore_IDFA_6](https://cdn.xinstall.com/iOS_SDK%E7%B4%A0%E6%9D%90/IDFA_6.png)





# **四、如何测试功能**

参考官方文档 [测试集成效果](https://doc.xinstall.com/integrationGuide/comfirm.html)



# **五、更多 Xinstall 进阶功能**

若您想要自定义下载页面，或者查看数据报表等进阶功能，请移步 [Xinstall 官网](https://xinstall.com/) 查看对应文档。

若您在集成过程中如有任何疑问或者困难，可以随时联系 [Xinstall 官方客服](https://url.cn/q0z85YeZ?_type=wpa&qidian=true) 在线解决。
