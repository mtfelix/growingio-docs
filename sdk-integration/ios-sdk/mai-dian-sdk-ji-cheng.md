---
description: >-
  埋点 SDK 的目标用户是使用第三方插件开发的 APP， 比如使用 Weex、APICloud
  等。在这些平台中，我们无法自动采集用户的点击事件和页面浏览事件等，需要依赖调用自定义事件和变量 API来进行数据采集。 如果您的 APP 使用 iOS
  原生开发，并且希望自动采集用户的点击事件、页面浏览事件等无埋点事件， 请集成 iOS 无埋点SDK 。
---

# IOS埋点SDK

## 埋点 SDK集成 

### 1. 选择集成方式

#### （1）使用 CocoaPods 快速集成

* 添加`pod 'GrowingCoreKit'`到 Podfile 中
* 执行`pod update`，不要用`--no-repo-update`选项
* 直接进行第 2 步 “[设置 URL Scheme](mai-dian-sdk-ji-cheng.md#2-she-zhi-url-scheme)”

#### （2）手动集成 SDK 

* 下载 2.6.3 版 iOS SDK以下包：[GrowingHeader ](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-PublicHeader-2.6.3.zip)，[GrowingCoreKit](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-CoreKit-2.6.3.zip)
* 解压 iOS SDK 压缩文件
*  将Growing.h,GrowingCoreKit.framework添加到iOS工程中。

{% hint style="warning" %}
#### **提醒:**  记得勾选 "Copy items if needed"
{% endhint %}

•  添加依赖, 在项目中添加以下库文件

| **库名称** | **类型** |
| :--- | :--- |
| Foundation.framework | 基础依赖库 |
| Security.framework | 用于SSL连接 |
| CoreTelephony.framework | 用于读取运营商名称 |
| SystemConfiguration.framework | 用于判断网络状态 |
| AdSupport.framework | 用于来源管理激活匹配 |
| libicucore.tbd | 用于WebSocket |
| libsqlite3.tbd | 存储日志 |
| CoreLocation.framework | 用于读取地理位置信息（如果您的app有权限） |

{% hint style="warning" %}
#### 提醒：添加项目依赖库的位置在项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries
{% endhint %}

  添加编译参数，注意大小写:

![](../../.gitbook/assets/image%20%28156%29.png)

### **2.设置URL Scheme**

#### **\(1\) 获取URL Scheme**

•   添加新产品：登录官网-&gt; 点击项目选择框-&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 点击“新建应用”-&gt;选择添加iOS 应用-&gt; 填写“应用名称“，点击下一步-&gt;在第二段中标黄字体。

•   现有产品：登录官网-&gt; 点击项目选择框-&gt; 点击“项目管理” -&gt; 点击“应用管理” -&gt; 找到对应产品的URL Scheme

![](../../.gitbook/assets/image%20%28147%29.png)

#### **\(2\) 添加URL Scheme（growing.xxxxxxxxxxxxxxxx）到项目中**

#### **\(3\) 在AppDelegate 中添加代码**

```objectivec
- (BOOL)application:(UIApplication*)application openURL:(NSURL*)url sourceApplication:(NSString*)sourceApplication annotation:(id)annotation
{
    if([Growing handleUrl:url]){
        return YES;
    }
    ...
    return NO;
}
```

{% hint style="warning" %}
### 提醒：

* 若您在 AppDelegate 中实现了以下一个或多个方法，请在已实现的函数中，调用`[Growing handleUrl:]`

  ```objectivec
  - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(id)annotation
  - (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url
  - (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options
  ```

* 若以上所有方法均未实现，请实现以下方法并调用`[Growing handleUrl:]`

  ```objectivec
  - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(id)annotation
  ```

* 实际情况可能很复杂，请在调试时确保函数`[Growing handleUrl:]`会被执行到
{% endhint %}

### **3.添加初始化函数**

在AppDelegate 中引入`#import "Growing.h"`并添加启动方法

```objectivec
#import "Growing.h"
- (BOOL)application:(UIApplication*)application
    didFinishLaunchingWithOptions:(NSDictionary*)launchOptions {
      ...
      //启动GrowingIO
     [Growing startWithAccountId:@"xxxxxxxxxxxxxxxx"]; //替换为您的ID
     // 其他配置
     // 开启Growing调试日志可以开启日志
     // [Growing setEnableLog:YES];
  }
```

**请确保将代码添加在上面描述的位置，添加到其他函数中或者异步block 中可能导致数据不准确！**

至此，您的SDK安装就成功了。

## 自定义事件和变量 API  <a id="zi-ding-yi-shi-jian-he-bian-liang-api"></a>



      您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些[自定义事件](https://docs.growingio.com/docs/~/drafts/-LI499co1_eo3lOYex8t/primary/data-defination/events-metrics/manual-metrics)和[变量](https://docs.growingio.com/docs/~/drafts/-LI499co1_eo3lOYex8t/primary/data-defination/dimensions/manual-dimensions)，下面介绍自定义事件和变量 API 使用方法。

SDK 提供多种不同类型的API，请根据您的实际需要正确地调用。

```objectivec
// 发送自定义事件 API
+ (void)track:(NSString *)eventId;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number andVariable:(NSDictionary<NSString *, NSObject *> *)variable;
+ (void)track:(NSString *)eventId withVariable:(NSDictionary<NSString *, NSObject *> *)variable;
​
// 发送页面级变量 API
+ (void)setPageVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariable:(NSDictionary<NSString *, NSObject *> *)variable toViewController:(UIViewController *)viewController;
​
// 发送转化变量 API
+ (void)setEvarWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setEvarWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setEvar:(NSDictionary<NSString *, NSObject *> *)variable;
​
// 发送用户变量 API
+ (void)setPeopleVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setPeopleVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setPeopleVariable:(NSDictionary<NSString *, NSObject *> *)variable;
​
// 访问用户变量 API
+ (void)setVisitor:(NSDictionary<NSString *, NSObject *> *)variable;
​
// 设置登录用户ID API
+ (void)setUserId:(NSString *)userId;
​
// 清除登录用户ID API
+ (void)clearUserId;
```

### track <a id="track"></a>

发送一个自定义事件。在添加所需要发送的事件代码之前，需要在打点管理用户界面声明事件以及事件级变量。

#### 参数说明： <a id="can-shu-shuo-ming"></a>

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| eventId | String | 是 | 事件标识符 |
| number | Number | 否 | 事件的数值，没有number参数时，事件默认加1；当出现number参数时，事件自增number的数值。 |
| eventLevelVariable | JSON Object | 否 | 事件发生时所伴随的维度信息。 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">限制条件</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">eventId</td>
      <td style="text-align:left">英文数字组合的字符串，不能为 nil 或者""，长度小于等于50，且不能含有特殊字符</td>
    </tr>
    <tr>
      <td style="text-align:left">number</td>
      <td style="text-align:left">正整数或浮点数</td>
    </tr>
    <tr>
      <td style="text-align:left">eventLevelVariable</td>
      <td style="text-align:left">
        <p>不能为nil；eventLevelVariable 内部不允许含有<code>JSONObject</code>或者<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> 长度限制小于等于50，<code>value</code> 长度限制小等于1000，值不能为空串，也就是""。</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// track API原型
+ (void)track:(NSString *)eventId;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number;
+ (void)track:(NSString *)eventId withNumber:(NSNumber *)number andVariable:(NSDictionary<NSString *, NSObject *> *)variable;
+ (void)track:(NSString *)eventId withVariable:(NSDictionary<NSString *, NSObject *> *)variable;
```

```objectivec
// track API调用示例一
[Growing track:@"registerSuccess"];
```

```objectivec
// track API调用示例二
[Growing track:@"registerSuccess" withVariable:@{@"gender":@"male", @"age":@"21"}];
```

```objectivec
// track API调用示例三
[Growing track:@"loanAmount" withNumber:@800000 andVariable:@{@"loanType":@"houseMortgage", @"province":@"Zhejiang"}];
```

### setPageVariable

发送页面级别的信息，在添加代码之前必须在打点管理界面上声明页面级变量。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSON Object | 否 | 页面级别的信息 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">限制条件</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">key</td>
      <td style="text-align:left">不能为 nil 或者""，长度小于等于50</td>
    </tr>
    <tr>
      <td style="text-align:left">value</td>
      <td style="text-align:left">不能为 nil 或者""，若为字符串则长度应小于等于 1000</td>
    </tr>
    <tr>
      <td style="text-align:left">pageLevelVariable</td>
      <td style="text-align:left">
        <p>不能为nil; pageLevelVariables 内部不允许含有<code>JSONObject</code>或者<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> 长度限制小于等于50，<code>value</code> 长度限制小等于1000，值不能为空串，也就是""。</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// setPageVariable API原型
+ (void)setPageVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue toViewController:(UIViewController *)viewController;
+ (void)setPageVariable:(NSDictionary<NSString *, NSObject *> *)variable toViewController:(UIViewController *)viewController;
```

```objectivec
// setPageVariable API调用示例一
[Growing setPageVariableWithKey:@"author" andStringValue:@"Zhang San" toViewController:myViewController];
```

```objectivec
// setPageVariable API调用示例二
[Growing setPageVariable:@{@"pageName":@"Home Page", @"author":@"Zhang San"} toViewController:myViewController];
```

### setEvar

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 转化变量的标识符 |
| Value | String | 否 | 转化变量的值 |
| conversionVariables | JSON Object | 否 | 转化变量用于高级归因分析 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">限制条件</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">key</td>
      <td style="text-align:left">不能为 nil 或者""，长度小于等于50</td>
    </tr>
    <tr>
      <td style="text-align:left">Value</td>
      <td style="text-align:left">变量不为nil或者""，若为字符串则长度应小于等于 1000</td>
    </tr>
    <tr>
      <td style="text-align:left">conversionLevelVariable</td>
      <td style="text-align:left">
        <p>不能为nil; conversionLevelVariable 内部不允许含有<code>JSONObject</code>或者<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> 长度限制小于等于50，<code>value</code> 长度限制小等于1000，值不能为空串，也就是""。</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// setEvar API原型
+ (void)setEvarWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setEvarWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setEvar:(NSDictionary<NSString *, NSObject *> *)variable;
```

```objectivec
// setEvar API调用示例一
[Growing setEvarWithKey:@"campaignId" andStringValue:@"1234567890"];
```

```objectivec
// setEvar API调用示例二
[Growing setEvar:@{@"campaignId":@"12345", @"campaignOwner":@"Li Si"}];
```

### setPeopleVariable

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明用户变量。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| customerVariables | JSON Object | 否 | 用户变量用于用户信息相关的分析 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">限制条件</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">key</td>
      <td style="text-align:left">不能为nil或""，长度小于等于50</td>
    </tr>
    <tr>
      <td style="text-align:left">value</td>
      <td style="text-align:left">变量不为nil或者""，若为字符串则长度应小于等于 1000</td>
    </tr>
    <tr>
      <td style="text-align:left">customerVariables</td>
      <td style="text-align:left">
        <p>不能为nil;customerVarialbes 内部不允许含有<code>JSONObject</code>或者<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> 长度限制小于等于50，<code>value</code> 长度限制小等于1000，值不能为空串，也就是""。</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// setPeopleVariable API原型
+ (void)setPeopleVariableWithKey:(NSString *)key andStringValue:(NSString *)stringValue;
+ (void)setPeopleVariableWithKey:(NSString *)key andNumberValue:(NSNumber *)numberValue;
+ (void)setPeopleVariable:(NSDictionary<NSString *, NSObject *> *)variable;
```

```objectivec
// setPeopleVariable API调用示例一
[Growing setPeopleVariableWithKey:@"gender" andStringValue:@"male"];
```

```objectivec
// setPeopleVariable API调用示例二
[Growing setPeopleVariable:@{@"gender":@"male", @"age":@"25"}];
```

### setVisitor

#### 2.4.0以上版本支持

当用户未登录时，定义用户属性变量，也可用于A/B测试上传标签。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| variable | JSON Object | 是 | 访问用户信息 |

**参数限制条件：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">限制条件</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">variable</td>
      <td style="text-align:left">
        <p>不能为<code>nil;variable</code> 内部不允许含有<code>JSONObject</code>或者<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> 长度限制小于等于50，<code>value</code> 长度限制小等于1000，值不能为空串，也就是""。</p>
      </td>
    </tr>
  </tbody>
</table>```objectivec
// setVisitor 访问用户变量 API原型
+ (void)setVisitor:(NSDictionary<NSString *, NSObject *> *)variable;
```

```text
// setVisitor API调用示例
[Growing setVisitor:@{@"gender":@"male", @"age":@"25"}];
```

### setUserId

当用户登录之后调用setUserId API，设置登录用户ID。

#### 参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| userId | String | 是 | 用户的登录用户ID |

**参数限制条件：**

| 参数名称 | 限制条件 |
| :--- | :--- |
| userId | 英文数字组合的字符串，长度小于等于1000，且不能含有特殊字符，不允许传空、`""` 或者`nil`，如有清除操作，请调用 `clearUserId` 方法 |

```objectivec
// setUserId API原型
+ (void)setUserId:(NSString *)userId;
```

```objectivec
// setuserId API调用示例
[Growing setUserId:@"1234567890"];
```

{% hint style="warning" %}
如果您的应用是App，且每次用户升级App版本时无需重新登录的话，建议在用户每次升级App版本后初次访问时重新调用上述 setUserId 方法。
{% endhint %}

### clearUserId

当用户登出之后调用clearUserId，清除已经设置的登录用户ID。

```objectivec
// clearUserId API原型
+ (void)clearUserId;
```

```objectivec
// clearUserId API调用示例
[Growing clearUserId];
```

## 埋点SDK支持的其他接口

{% hint style="danger" %}
**GrowingIO 埋点 SDK 仅自动采集设备信息和您埋点内容数据，对比无埋点 SDK ，埋点 SDK 少很多 API， 请勿在埋点 SDK 中调用无埋点 SDK 接口。**
{% endhint %}

埋点SDK除了支持上面的打点事件，还支持以接口，接口详情请参考[IOS SDK API](ios-sdk-api.md)中的说明：

```objectivec
1，如果需要采样设置一个采样值  0.01即1% 0.001即1‰  最多支持小数点后5位
+ (void)startWithAccountId:(NSString*)accountId withAppId:(NSString*)appId withSampling:(CGFloat)sampling;
+ (void)startWithAccountId:(NSString*)accountId withSampling:(CGFloat)sampling;

2，默认采样100%
+ (void)startWithAccountId:(NSString*)accountId withAppId:(NSString*)appId;
+ (void)startWithAccountId:(NSString*)accountId;

3，命令行输出调试日志
+ (void)setEnableLog:(BOOL)enableLog;
+ (BOOL)getEnableLog;

4，若使用加密功能,请在UI元素初始化之前设置此函数
+ (void)setEncryptStringBlock:(NSString*(^)(NSString*string))block;

5，以下函数设置后会覆盖原有设置
// 并且只会在第一次安装后调用以保证同一设备的设备ID相同
// 请在方法startWithAccountId之前调用
// 使用自定义的ID 自定义ID长度不可大于64 否则会被抛弃NSUUID的UUIDString长度为36
+ (void)setDeviceIDModeToCustomBlock:(NSString*(^)(void))customBlock;

6，deeplink广告落地页参数回调设置
+ (void)registerDeeplinkHandler:(void(^)(NSDictionary*params, NSError*error))handler;

7，Universallink广告落地页参数回调设置
+ (void)registerUniversallinkHandler:(void(^)(NSDictionary*params, NSError*error))handler;

8，该函数请在main函数第一行调用APP启动后将不允许修改采集模式
+ (void)setAspectMode:(GrowingAspectMode)aspectMode;
+ (GrowingAspectMode)getAspectMode;

9，是否允许发送基本性能诊断信息，默认为开
+ (void)setEnableDiagnose:(BOOL)enable;

10，全局不发送统计信息
+ (void)disable;

11，设置发送数据的时间间隔（单位为秒）
+ (void)setFlushInterval:(NSTimeInterval)interval;
+ (NSTimeInterval)getFlushInterval;

12，设置每天使用数据网络（2G、3G、4G）上传的数据量的上限（单位是KB）
+ (void)setDailyDataLimit:(NSUInteger)numberOfKiloByte;
+ (NSUInteger)getDailyDataLimit;

13，设置数据收集平台服务器地址
+ (void)setTrackerHost:(NSString*)host;

14，设置设备报活服务器地址
+ (void)setReportHost:(NSString*)host;

15，设置数据查看平台服务器地址
+ (void)setDataHost:(NSString*)host;

16，设置数据后台服务器地址
+ (void)setGtaHost:(NSString*)host;

17，设置数据后台服务器地址
+ (void)setWsHost:(NSString*)host;

18，设置zone信息
+ (void)setZone:(NSString*)zone;

19，设置GDPR 生效
+ (void)disableDataCollect;

20，设置GDPR 失效
+ (void)enableDataCollect;

21，获取当前设备id
+ (NSString*)getDeviceId;

22，获取当前uid
+ (NSString*)getVisitUserId;

23，获取当前访问id
+ (NSString*)getSessionId;
```



