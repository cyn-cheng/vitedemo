/*
title:没啥才艺，30行代码写了个春联数据爬虫
date:2022-01-08
keyword:爬虫,春联
*/

PK创意闹新春，我正在参加「春节创意投稿大赛」，详情请看：春节创意投稿大赛

## 1. 缘起
开头那句话是掘金的，人家平台总要有个口号。下面这句话是我的，我也一直有个口号：
> 我有两个爱好，一个是传统文化，一个是高新技术。

没错，我一直探索用高新技术来激活传统文化，用传统文化来滋养高新技术。

这不，我打算写一个基于TensorFlow的自动对春联的程序。经过尝试，目前已经完成了。

Input是人工输入的上联，Output是机器自动给出的下联。

```
Input: <start> 神 州 万 里 春 光 美 <end> [2, 61, 27, 26, 43, 4, 20, 78, 3] 
Output:<start> 祖 国 两 制 好 事 兴 <end> [2, 138, 11, 120, 428, 73, 64, 46, 3]

Input:  <start> 爆 竹 迎 新 春 <end> [2, 167, 108, 23, 9, 4, 3]
Output: <start> 瑞 雪 兆 丰 年 <end> [2, 92, 90, 290, 30, 8, 3]

Input:  <start> 金 牛 送 寒 去 <end> [2, 63, 137, 183, 302, 101, 3]
Output: <start> 玉 鼠 喜 春 来 <end> [2, 126, 312, 17, 4, 26, 3]

Input:  <start> 锦 绣 花 似 锦 <end> [2, 68, 117, 8, 185, 68, 3]
Output: <start> 缤 纷 春 如 风 <end> [2, 1651, 744, 4, 140, 7, 3]

Input:  <start> 春 风 送 暖 山 河 好 <end> [2, 4, 5, 183, 60, 7, 71, 45, 3]
Output: <start> 瑞 雪 迎 春 世 纪 新 <end> [2, 92, 90, 27, 4, 36, 99, 5, 3]

Input:  <start> 百 花 争 艳 春 风 得 意 <end> [2, 48, 8, 164, 76, 4, 5, 197, 50, 3]
Output: <start> 万 马 奔 腾 喜 气 福 多 <end> [2, 6, 28, 167, 58, 17, 33, 15, 113, 3]
```

## 2. 实现

人工智能的背后是大量数据的训练，而仅仅这些训练数据就让人很为难：找不到啊。

网络上有一个开源的对对联项目，里面有一个70万条的对联数据集，项目地址如下：GitHub - wb14123/couplet-dataset: Dataset for couplets. 70万条对联数据库。


但是，我并不满意，因为我要的是春联，不是对联。

对联虽然包含了春联，但是春联是带有民俗气息的，里面充满了喜庆祥和的味道。

于是我在网络上找到了一个春联网站 www.duiduilian.com/chunlian/ ，里面的内容质量不错。于是，我就写了个爬虫程序去采集数据。

### 2.1 分析元素

打开网址，浏览器按F12分析网页。

分析可见，我们关注的主体内容都在<div class="content_zw"></div>之间，而且一幅对联用<p></p>包裹，上下联用，分割。

这是极其标准的数据爬取的素材案例，或许这就是为了教学而设计的。

如果我们要获取春联内容的话，只需要通过网址加载下来html代码，然后取出正文部分，然后通过<p>标签分组，每副春联通过逗号“，”分上下联，最后存入文件就行了。

## 开干。

### 2.2 取出正文

首先加载网址，取出我们关注的正文。
```
import requests
import re

# 模拟浏览器发送http请求
response = requests.get(url)
# 设置编码方式
response.encoding='gbk'
# 获取整个网页html
html = response.text
# 根据标签分析，从html中获取正文
allText = re.findall(r'<div class="content_zw">.*?</div>', html, re.S)[0]
print(url+"\n allText:"+allText)
```

...待续