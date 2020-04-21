## 爬虫准备知识

网络爬虫：是一种用来自动浏览万维网的网络机器人，自动抓取网页信息的代码，能够从网上获取自己需要的数据，可以简单理解成代替繁琐的复制粘贴操作的手段。

爬虫流程

![img](https://img-blog.csdn.net/20160813183505426)

### HTTP

HTTP是一个客户端（用户）和服务器端（网站）之间进行请求和应答的标准。通过使用网页浏览器、网络爬虫或者其他工具，客户端可以向服务器上的指定端口（默认端口为80）发起一个HTTP请求。这个客户端成为客户代理。应答服务器上存储着一些资源码，比如HTML文件和图像。这个应答服务器成为源服务器。在用户代理和源服务器中间可能存在多个“中间层”，比如代理服务器、网关或者隧道。

| HTTP状态码 |                   说明                   |
| :--------: | :--------------------------------------: |
|  **200**   |        **请求成功，获得响应内容**        |
|    404     |                  未响应                  |
|    502     |                 错误网关                 |
|    503     | 服务出错(一般为临时的，服务器过载或维护) |
|    400     |                 非法请求                 |

### 网页组成

- HTML：用来搭建整个网页的框架
- CSS：调整网页样式，包括界面颜色，模块大小、位置等
- JaveScript：让网页动起来
  - 实现网页数据动态交互
  - 网页动画

### 网页结构

```html
<!DOCTYPE  html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Demo</title>
    </head>
    <body>
        <div id="container">
            <div class="wrapper">
                <h1>Hello World</h1>
                <div>Hello Python.</div>
            </div>
        </div>
    </body>
</html>
```

首先，整个文档是以 DOCTYPE 来开头的，这里定义了文档类型是 html ，整个文档最外层的标签是 <html> ，并且结尾还以 </html> 来表示闭和。

这里简单讲一下，浏览器解析 HTML 的时候，并不强制需要每个标签都一定要有闭和标签，但是为了语义明确，最好每个标签都跟上对应的闭和标签。大家可以尝试删除其中的闭和标签进行尝试，并不会影响浏览器的解析。

整个 HTML 文档一般分为 head 和 body 两个部分，在 head 头中，我们一般会指定当前的编码格式为 UTF-8 ，并且使用 title 来定义网页的标题，这个会显示在浏览器的标签上面。

body 中的内容一般为整个 html 文档的正文，html的标签由六个标签构成，字体由大到小递减，换行标签为<br>，链接使用<a>首先，整个文档是以 DOCTYPE 来开头的，这里定义了文档类型是 html ，整个文档最外层的标签是 <html> ，并且结尾还以 </html> 来表示闭和。

这里简单讲一下，浏览器解析 HTML 的时候，并不强制需要每个标签都一定要有闭和标签，但是为了语义明确，最好每个标签都跟上对应的闭和标签。大家可以尝试删除其中的闭和标签进行尝试，并不会影响浏览器的解析。

整个 HTML 文档一般分为 head 和 body 两个部分，在 head 头中，我们一般会指定当前的编码格式为 UTF-8 ，并且使用 title 来定义网页的标题，这个会显示在浏览器的标签上面。

body 中的内容一般为整个 html 文档的正文，html的标签由六个标签构成，字体由大到小递减，换行标签为<br>，链接使用<a>来创建，href属性包含链接的URL地址。

- id 属性为元素提供在全文档内的唯一标识。它用于识别元素，以便样式表可以改变其外观属性，脚本可以改变、显示或删除其内容或格式化。对于添加到页面的url，它为元素提供了一个全局唯一识别，通常为页面的子章节。
- class 属性提供了一种将类似元素分类的方式，常被用于语义化或格式化。例如，一个html文档可以指定class="标记"来表明所有具有这一类值得元素都属于文档的主文本。格式化后，这样的元素可能会聚集在一起，并作为页面脚注而不会出现在html代码中。类值也可以多值声明。如class="标记 重要"将元素同时放入“标记”与“重要”两类中。
- style 属性可以将表现性质赋予一个特定原色。比起使用id或class属性从样式表中选择元素，“style”被认为是一个更好的做法。
- tile 属性用于给元素一个附加的说明。大多数浏览器中这一属性显示为工具提示。

##### HTML DOM 树

![DOM HTML tree](https://www.runoob.com/images/htmltree.gif)

### Python爬虫所需库

- requests:数据爬取，发送请求
- BeautifulSoup：数据解析，定位标签
- lxml：通过xpath路径解析
- re：正则表达式，作用万能，但书写较繁琐

### 爬虫案例

[^豆瓣]: 爬取豆瓣电影Top250（https://movie.douban.com/top250）

```python
# 导入所需包，本次通过xpath解析
import requests
from lxml import etree
import time   #计算爬取时间
start = time.time()
# 添加请求头，模拟浏览器访问，UA反爬机制
headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.113 Safari/537.36'}
url = 'https://movie.douban.com/top250'
# 共250条数据，每页25条
for page in range(0,250,25):
    # 传入参数，以此实现翻页
    params = {'start':page,
             'filter':''}
    res = requests.get(url=url,headers=headers,params=params).text
    tree = etree.HTML(res)
    lis = tree.xpath('//ol[@class="grid_view"]/li')
    for li in lis:
        # 获取电影名称，评分以及海报图片地址
        title = li.xpath('.//div[@class="hd"]/a/span[@class="title"][1]/text()')[0]
        star = li.xpath('.//div[@class="star"]/span[@class="rating_num"]/text()')[0]
        img = li.xpath('.//div[@class="pic"]/a/img/@src')[0]
        print(title,star,img)
print('爬取完成，用时{}秒'.format(time.time()-start))
```

结果输出：

```
肖申克的救赎 9.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p480747492.jpg
霸王别姬 9.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2561716440.jpg
阿甘正传 9.5 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1484728154.jpg
这个杀手不太冷 9.4 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p511118051.jpg
美丽人生 9.5 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2578474613.jpg
泰坦尼克号 9.4 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p457760035.jpg
千与千寻 9.4 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2557573348.jpg
辛德勒的名单 9.5 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p492406163.jpg
盗梦空间 9.3 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p513344864.jpg
忠犬八公的故事 9.4 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p524964016.jpg
海上钢琴师 9.3 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2574551676.jpg
楚门的世界 9.3 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p479682972.jpg
三傻大闹宝莱坞 9.2 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p579729551.jpg
机器人总动员 9.3 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1461851991.jpg
放牛班的春天 9.3 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1910824951.jpg
星际穿越 9.3 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2206088801.jpg
大话西游之大圣娶亲 9.2 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2455050536.jpg
熔炉 9.3 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1363250216.jpg
疯狂动物城 9.2 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2315672647.jpg
无间道 9.2 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2564556863.jpg
龙猫 9.2 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2540924496.jpg
教父 9.3 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p616779645.jpg
当幸福来敲门 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1312700628.jpg
怦然心动 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p501177648.jpg
触不可及 9.2 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1454261925.jpg
蝙蝠侠：黑暗骑士 9.2 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p462657443.jpg
控方证人 9.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1505392928.jpg
活着 9.2 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2513253791.jpg
乱世佳人 9.3 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1963126880.jpg
寻梦环游记 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2503997609.jpg
末代皇帝 9.2 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p452089833.jpg
摔跤吧！爸爸 9.0 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2457983084.jpg
指环王3：王者无敌 9.2 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1910825503.jpg
少年派的奇幻漂流 9.1 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1784592701.jpg
何以为家 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2555295759.jpg
飞屋环游记 9.0 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p485887754.jpg
十二怒汉 9.4 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2173577632.jpg
鬼子来了 9.2 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2553104888.jpg
天空之城 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1446261379.jpg
大话西游之月光宝盒 9.0 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2561721372.jpg
哈尔的移动城堡 9.1 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2174346180.jpg
素媛 9.2 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2118532944.jpg
天堂电影院 9.2 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2559577569.jpg
罗马假日 9.0 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2189265085.jpg
闻香识女人 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2550757929.jpg
辩护人 9.2 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2158166535.jpg
哈利·波特与魔法石 9.0 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2591591494.jpg
搏击俱乐部 9.0 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1910926158.jpg
我不是药神 9.0 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2561305376.jpg
死亡诗社 9.1 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2575465690.jpg
教父2 9.2 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2194138787.jpg
指环王2：双塔奇兵 9.1 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p909265336.jpg
狮子王 9.0 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2277799019.jpg
窃听风暴 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1808872109.jpg
大闹天宫 9.3 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2184505167.jpg
指环王1：魔戒再现 9.0 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1354436051.jpg
两杆大烟枪 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p792443418.jpg
美丽心灵 9.0 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1665997400.jpg
饮食男女 9.1 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1910899751.jpg
飞越疯人院 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p792238287.jpg
猫鼠游戏 9.0 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p453924541.jpg
黑客帝国 9.0 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p451926968.jpg
钢琴家 9.2 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p792376093.jpg
V字仇杀队 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1465235231.jpg
本杰明·巴顿奇事 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2192535722.jpg
看不见的客人 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2498971355.jpg
让子弹飞 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1512562287.jpg
西西里的美丽传说 8.9 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2441988159.jpg
小鞋子 9.2 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2173580536.jpg
海豚湾 9.3 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2559579779.jpg
拯救大兵瑞恩 9.0 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1014542496.jpg
情书 8.9 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p449897379.jpg
穿条纹睡衣的男孩 9.1 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1473670352.jpg
音乐之声 9.0 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2189265302.jpg
美国往事 9.2 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p477229647.jpg
绿皮书 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2549177902.jpg
致命魔术 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p480383375.jpg
海蒂和爷爷 9.2 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2554525534.jpg
低俗小说 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1910902213.jpg
七宗罪 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2219586434.jpg
沉默的羔羊 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1593414327.jpg
蝴蝶效应 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2209066019.jpg
春光乍泄 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p465939041.jpg
禁闭岛 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1832875827.jpg
被嫌弃的松子的一生 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p884763596.jpg
心灵捕手 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p480965695.jpg
布达佩斯大饭店 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2178872593.jpg
阿凡达 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2180085848.jpg
剪刀手爱德华 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p480956937.jpg
勇敢的心 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1374546770.jpg
摩登时代 9.3 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2263408369.jpg
天使爱美丽 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2447590313.jpg
喜剧之王 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2579932167.jpg
加勒比海盗 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1596085504.jpg
致命ID 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2558364386.jpg
断背山 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2154212680.jpg
杀人回忆 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2326071698.jpg
狩猎 9.1 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1546987967.jpg
幽灵公主 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1613191025.jpg
请以你的名字呼唤我 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2505525050.jpg
哈利·波特与死亡圣器(下) 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p917846733.jpg
阳光灿烂的日子 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2564685215.jpg
入殓师 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p594972928.jpg
重庆森林 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p792381411.jpg
第六感 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2220184425.jpg
小森林 夏秋篇 9.0 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2564498893.jpg
7号房的礼物 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1816276065.jpg
消失的爱人 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2221768894.jpg
红辣椒 9.0 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p456825720.jpg
爱在黎明破晓前 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2555762374.jpg
小森林 冬春篇 9.0 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2258078370.jpg
玛丽和马克思 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2162822165.jpg
侧耳倾听 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p456692072.jpg
一一 9.0 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2567845803.jpg
唐伯虎点秋香 8.6 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2357915564.jpg
告白 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p689520756.jpg
蝙蝠侠：黑暗骑士崛起 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1706428744.jpg
大鱼 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p692813374.jpg
阳光姐妹淘 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1374786017.jpg
倩女幽魂 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2414157745.jpg
超脱 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1305562621.jpg
射雕英雄传之东成西就 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2535922598.jpg
甜蜜蜜 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2223011274.jpg
驯龙高手 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2210954024.jpg
萤火之森 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1675053073.jpg
超能陆战队 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2224568669.jpg
无人知晓 9.1 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p661160053.jpg
幸福终点站 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p854757687.jpg
菊次郎的夏天 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p751835224.jpg
借东西的小人阿莉埃蒂 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p617533616.jpg
恐怖直播 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2016930906.jpg
爱在日落黄昏时 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1910924221.jpg
完美的世界 9.1 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p792403691.jpg
神偷奶爸 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p792776858.jpg
怪兽电力公司 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2513247938.jpg
玩具总动员3 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1283675359.jpg
风之谷 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1917567652.jpg
血战钢锯岭 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2398141939.jpg
傲慢与偏见 8.6 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p452005185.jpg
功夫 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2219011938.jpg
上帝之城 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p455677490.jpg
时空恋旅人 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2070153774.jpg
教父3 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2169664351.jpg
电锯惊魂 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2565332644.jpg
喜宴 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2173713676.jpg
人生果实 9.5 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2544912792.jpg
天书奇谭 9.2 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2515539487.jpg
谍影重重3 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p792223507.jpg
英雄本色 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2504997087.jpg
被解救的姜戈 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1959232369.jpg
岁月神偷 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p456666151.jpg
七武士 9.2 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2565471701.jpg
哪吒闹海 9.1 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2516566783.jpg
我是山姆 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p652417775.jpg
疯狂原始人 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1867084027.jpg
纵横四海 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2272146906.jpg
头号玩家 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2516578307.jpg
三块广告牌 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2510081688.jpg
心迷宫 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2276780256.jpg
萤火虫之墓 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1157334208.jpg
达拉斯买家俱乐部 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2166160837.jpg
釜山行 8.5 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2360940399.jpg
真爱至上 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p475600770.jpg
荒蛮故事 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2584519452.jpg
东邪西毒 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1982176012.jpg
贫民窟的百万富翁 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2434249040.jpg
记忆碎片 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p641688453.jpg
爆裂鼓手 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2220776342.jpg
你的名字。 8.4 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2395733377.jpg
黑天鹅 8.6 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2549648344.jpg
花样年华 8.6 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1910828286.jpg
卢旺达饭店 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p470419493.jpg
哈利·波特与阿兹卡班的囚徒 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1910812549.jpg
忠犬八公物语 9.2 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1576418852.jpg
黑客帝国3：矩阵革命 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p443461818.jpg
头脑特工队 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2266293606.jpg
模仿游戏 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2255040492.jpg
一个叫欧维的男人决定去死 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2406624993.jpg
雨人 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p942376281.jpg
你看起来好像很好吃 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p709670262.jpg
未麻的部屋 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1351050722.jpg
无敌破坏王 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1735642656.jpg
哈利·波特与密室 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1082651990.jpg
恋恋笔记本 8.5 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p483604864.jpg
冰川时代 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1910895719.jpg
海街日记 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2232247487.jpg
新世界 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1903379979.jpg
海边的曼彻斯特 8.6 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2421855655.jpg
二十二 8.7 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2457609817.jpg
虎口脱险 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2399597512.jpg
房间 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2259715855.jpg
恐怖游轮 8.5 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p462470694.jpg
惊魂记 9.0 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1021883305.jpg
奇迹男孩 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2507709428.jpg
魔女宅急便 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p456676352.jpg
人工智能 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p792257137.jpg
雨中曲 9.0 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1612355875.jpg
疯狂的石头 8.5 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p712241453.jpg
罗生门 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2564689879.jpg
海洋 9.1 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2559581324.jpg
爱在午夜降临前 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2074715729.jpg
终结者2：审判日 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1910909085.jpg
小偷家族 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2530599636.jpg
魂断蓝桥 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2351134499.jpg
燃情岁月 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1023654037.jpg
初恋这件小事 8.4 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p767451487.jpg
穿越时空的少女 8.6 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2079334286.jpg
可可西里 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2414771522.jpg
绿里奇迹 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p767586451.jpg
2001太空漫游 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2560717825.jpg
牯岭街少年杀人事件 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p848381236.jpg
完美陌生人 8.5 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2522331945.jpg
无耻混蛋 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2575043939.jpg
城市之光 9.3 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2170238828.jpg
阿飞正传 8.5 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2525770523.jpg
新龙门客栈 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1421018669.jpg
源代码 8.4 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p988260245.jpg
香水 8.5 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2441127736.jpg
谍影重重2 8.6 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p667644866.jpg
青蛇 8.5 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p584021784.jpg
谍影重重 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1597183981.jpg
地球上的星星 8.9 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1973489335.jpg
战争之王 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p792282381.jpg
猜火车 8.5 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p513567548.jpg
血钻 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1244017073.jpg
色，戒 8.4 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p453716305.jpg
遗愿清单 8.6 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p708613284.jpg
大佛普拉斯 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2505928032.jpg
步履不停 8.8 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2561539680.jpg
朗读者 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1140984198.jpg
疯狂的麦克斯4：狂暴之路 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2236181653.jpg
彗星来的那一夜 8.5 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2187896734.jpg
浪潮 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1344888983.jpg
小萝莉的猴神大叔 8.4 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2510956726.jpg
再次出发之纽约遇见你 8.5 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2250287733.jpg
聚焦 8.8 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2263822658.jpg
驴得水 8.3 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2393044761.jpg
东京物语 9.2 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p1925331564.jpg
追随 8.9 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2561545031.jpg
一次别离 8.7 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2189835254.jpg
千钧一发 8.8 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2195672555.jpg
我爱你 9.0 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p1075591188.jpg
九品芝麻官 8.5 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p648370300.jpg
黑鹰坠落 8.7 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1910900710.jpg
四个春天 8.9 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2540578887.jpg
哈利·波特与火焰杯 8.5 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2220723219.jpg
发条橙 8.5 https://img9.doubanio.com/view/photo/s_ratio_poster/public/p529908155.jpg
网络谜踪 8.6 https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2542848758.jpg
E.T. 外星人 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p984732992.jpg
黑客帝国2：重装上阵 8.6 https://img3.doubanio.com/view/photo/s_ratio_poster/public/p443461390.jpg
爬取完成，用时4.589800834655762秒
```

### API接口使用

接口为网站为用户提供的可直接获取数据的一种途径，更加方便、快捷，以百度地图接口为例：

```python
import requests

def getUrl(*address):
    ak = ''  ## 填入你的api key
    if len(address) < 1:
        return None
    else:
        for add in address:   
            url = 'http://api.map.baidu.com/geocoding/v3/?address={0}&output=json&ak={1}'.format(add,ak)  
            yield url
            
def getPosition(url):
    '''返回经纬度信息'''
    res = requests.get(url)
    #print(res.text)
    json_data = eval(res.text)
    
    if json_data['status'] == 0:
        lat = json_data['result']['location']['lat'] #纬度
        lng = json_data['result']['location']['lng'] #经度
    else:
        print("Error output!")
        return json_data['status']
    return lat,lng

if __name__ == "__main__":
    address = ['北京市清华大学','北京市北京大学','保定市华北电力大学','上海市复旦大学','武汉市武汉大学']
    for add in address:
        add_url = list(getUrl(add))[0]
        print('url:', add_url)
        try:
            lat,lng = getPosition(add_url)
            print("{0}|经度:{1}|纬度:{2}.".format(add,lng,lat))
        except Error as e:
            print(e)
```

结果输出

```python
北京市清华大学|经度:116.33337396094367|纬度:40.009645090734296.
北京市北京大学|经度:116.31683256328296|纬度:39.99887680537622.
保定市华北电力大学|经度:115.52130317483764|纬度:38.89477430426888.
上海市复旦大学|经度:121.74295536914276|纬度:31.06665792321301.
武汉市武汉大学|经度:114.37292090919235|纬度:30.543803317143624.
```

### 动态加载

为了避免每个页面都使用单独的url，每次只需加载部分数据，也是常见的反爬机制。

常见网站有微博、今日头条等新闻类网站。