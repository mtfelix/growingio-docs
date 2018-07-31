# 推广网站

对于网站/H5 产品，GrowingIO通过 UTM 参数区分不同链接来源，进行访问，跳出率等行为统计。您也可以自行[参考学习资料进行 UTM 参数的配置](utm-parameters.md)，区别在于通过「广告监测」创建的链接可以统计链接点击数据。

* [1.链接新建与管理](web-marketing.md#1)
  * [1.1 新建链接](web-marketing.md#11)
  * [1.2 推广管理](web-marketing.md#12)
* [2.数据报表](web-marketing.md#2)
  * [2.1 基础数据](web-marketing.md#21)
  * [2.2 深度行为分析](web-marketing.md#22)
* [3.数据说明](web-marketing.md#3)

### 1.链接新建与管理 {#1}

#### 1.1 新建链接 {#11}

1. 点击进入 GIO 后台，广告监测，选择网站推广。
2. 进入“配置与管理”模块“推广管理”，点击“新建监测”。
3. 按照系统要求填写内容。监测链接为您自定义命名，示例：百度 618 投放。
4. 点击保存。
5. 获取监测链接（或二维码），用于投放。

![](https://docs.growingio.com/.gitbook/assets/1%20%283%29.png)

#### 1.2 推广管理 {#12}

可进行链接的增删改查。

![](https://docs.growingio.com/.gitbook/assets/4%20%282%29.png)

### 2.数据报表 {#2}

#### 2.1 基础数据 {#21}

#### 推广详细 {#推广详细}

1. 进入“基础数据”，“推广详细”模块
2. 推广数据可分天查看，也可查看今天分小时数据。今天数据统计有 1-2 小时延迟。
3. 有小箭头的字段支持排序。
4. 有筛选图标的字段支持筛选，支持的字段有：目标渠道，监测链接，推广活动。
5. 查询结果可直接下载。

![](https://docs.growingio.com/.gitbook/assets/2%20%285%29.png)

#### 2.2 深度行为分析 {#22}

如您需要将广告的维度数据与其他指标和维度数据进行关联分析，可以使用「事件分析」功能进行深度事件分析。

1. 新建事件分析;
2. 选择需要分析的指标和维度;
3. 引用广告的维度。以下维度可以被引用：网页监测链接&广告来源&广告名称&广告媒介&广内容&广告关键字;
4. 进行特定维度值的筛选;

![](https://docs.growingio.com/.gitbook/assets/3%20%284%29.png)

以上使用方式可以沿用在“漏斗分析”,“留存分析”,“用户分群”等模块。

### 3.数据说明 {#3}

1. 点击：链接的点击次数之和。
2. 访问用户量：对网站、App 有过访问的用户的数量。
3. 新访问用户量：对网站、App 有过访问的新用户数量。过去 365 天内对网站、App 没有过访问的用户定义为新用户。
4. 访问量：网站、App 的访问的数量。用户从进入网站（打开App）到离开为止的一个相对完整连续的操作过程定义为一个访问。当访问量被页面级维度分解时，访问量指对当前页面级维度的当前元素有过页面浏览（一次或多次，至少一次）的访问的次数。
5. 跳出率：在对网站的访问过程中，只有一个页面浏览的访问占所有访问的比率。当该指标被页面级维度分解时，指从当前页面级维度的当前元素进入访问里面，只有一个页面浏览的访问的比率。
6. 平均访问时长（分钟）：平均每次访问时长，以分钟作为单位展示。由 总访问时长（分钟）/访问量 计算得到。
7. 每次访问页面浏览量：平均每次访问带来的页面浏览的数量。当该指标被页面级维度分解时，指有过当前页面级维度的当前元素的页面浏览的那些访问对当前维度的当前元素的页面浏览量。
