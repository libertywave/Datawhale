### 数据解析

#### Xpath

##### 介绍

XPath即为XML路径语言（XML Path Language），它是一种用来确定XML文档中某部分位置的语言。在XPath中，有七种类型的节点：元素、属性、文本、命名空间、处理指令、注释以及文档（根）节点。XML文档是被作为节点树来对待的

##### 路径表达式

- nodename 选取此节点的所有子节点。
- / 从根节点选取。
- // 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。
- . 选取当前节点。
- .. 选取当前节点的父节点。
- @ 选取属性。
- /text() 提取标签下面的文本内容
  - /标签名 逐层提取
  - /标签名 提取所有名为<>的标签
  - //标签名[@属性=“属性值”] 提取包含属性为属性值的标签
  - @属性名 代表取某个属性名的属性值

##### 使用方法

- 导入库：from lxml import etree
- lxml将html文本转成xml对象
  - tree = etree.HTML(html)
- 用户名称：tree.xpath(’//div[@class=“auth”]/a/text()’)
- 回复内容：tree.xpath(’//td[@class=“postbody”]’) 因为回复内容中有换行等标签，所以需要用string()来获取数据。
  - string():tree.xpath(’//td[@class=“postbody”]’).xpath(‘string(.)’)
- Xpath中text()，string()，data()的区别如下：
  - text()仅仅返回所指元素的文本内容。
  - string()函数会得到所指元素的所有节点文本内容，这些文本讲会被拼接成一个字符串。
  - data()大多数时候，data()函数和string()函数通用，而且不建议经常使用data()函数，有数据表明，该函数会影响XPath的性能。

##### 案例

- 爬取丁香园用户名和回复内容
- url = ‘http://www.dxy.cn/bbs/thread/626626#626626’

```python
import requests
from lxml import etree
url = 'http://www.dxy.cn/bbs/thread/626626#626626'
res = requests.get(url).text
tree = etree.HTML(res)
divs = tree.xpath('//div[@class="postbox"]/div')
n = 1
for div in divs:
    user = div.xpath('.//div[@class="auth"]/a/text()')[0]
    content = div.xpath('.//td[@class="postbody"]')[0].xpath('string(.)').strip()
    print('user'+str(n),user,content)
    print('*'*100)
    n += 1
```

结果输出

```python
pythonuser1 楼医生 我遇到一个“怪”病人，向大家请教。她，42岁。反复惊吓后晕厥30余年。每次受响声惊吓后发生跌倒，短暂意识丧失。无逆行性遗忘，无抽搐，无口吐白沫，无大小便失禁。多次跌倒致外伤。婴儿时有惊厥史。入院查体无殊。ECG、24小时动态心电图无殊；头颅MRI示小软化灶；脑电图无殊。入院后有数次类似发作。请问该患者该做何诊断，还需做什么检查，治疗方案怎样？
*****************************************************************************************
user2 lion000 从发作的症状上比较符合血管迷走神经性晕厥，直立倾斜试验能协助诊断。在行直立倾斜实验前应该做常规的体格检查、ECG、UCG、holter和X-ray胸片除外器质性心脏病。贴一篇“口服氨酰心安和依那普利治疗血管迷走性晕厥的疗效观察”作者：林文华 任自文 丁燕生http://www.ccheart.com.cn/ccheart_site/Templates/jieru/200011/1-1.htm
*****************************************************************************************
user3 xghrh 同意lion000版主的观点：如果此患者随着年龄的增长，其发作频率逐渐减少且更加支持，不知此患者有无这一特点。入院后的HOLTER及血压监测对此患者只能是一种安慰性的检查，因在这些检查过程中患者发病的机会不是太大，当然不排除正好发作的情况。对此患者应常规作直立倾斜试验，如果没有诱发出，再考虑有无可能是其他原因所致的意识障碍，如室性心动过速等，但这需要电生理尤其是心腔内电生理的检查，毕竟是有一种创伤性方法。因在外地，下面一篇文章可能对您有助，请您自己查找一下。心理应激事件诱发血管迷走性晕厥1例 ，杨峻青、吴沃栋、张瑞云，中国神经精神疾病杂志， 2002 Vol.28 No.2
*****************************************************************************************
user4 keys 该例不排除精神因素导致的，因为每次均在受惊吓后出现。当然，在作出此诊断前，应完善相关检查，如头颅MIR(MRA),直立倾斜试验等。
*****************************************************************************************
```

#### BeautifulSoup

##### 介绍

- Beautiful Soup 是一个HTML/XML 的解析器，主要用于解析和提取 HTML/XML 数据。

- 它基于HTML DOM 的，会载入整个文档，解析整个DOM树，因此时间和内存开销都会大很多，所以性能要低于lxml。
- BeautifulSoup 用来解析 HTML 比较简单，API非常人性化，支持CSS选择器、Python标准库中的HTML解析器，也支持 lxml 的 XML解析器。
- 虽然说BeautifulSoup4 简单容易比较上手，但是匹配效率还是远远不如正则以及xpath的，一般不推荐使用，推荐正则的使用。

##### 基本元素

- Tag 标签，最基本的信息组织单元，分别用<>和</>标明开头和结尾；
- Name 标签的名字，<p>…</p>的名字是'p'，格式：<tag>.name;
- Attributes 标签的属性，字典形式组织，格式：<tag>.attrs;
- NavigableString 标签内非属性字符串，<>…</>中字符串，格式：<tag>.string;
- Comment 标签内字符串的注释部分，一种特殊的Comment类型;

##### 遍历方法

HTML基本格式:`<>…</>`构成了所属关系，形成了标签的树形结构

- 标签树的下行遍历
  - .contents 子节点的列表，将`<tag>`所有儿子节点存入列表
  - .children 子节点的迭代类型，与.contents类似，用于循环遍历儿子节点
  - .descendants 子孙节点的迭代类型，包含所有子孙节点，用于循环遍历
- 标签树的上行遍
  - .parent 节点的父亲标签
  - .parents 节点先辈标签的迭代类型，用于循环遍历先辈节点
- 标签树的平行遍历
  - .next_sibling 返回按照HTML文本顺序的下一个平行节点标签
  - .previous_sibling 返回按照HTML文本顺序的上一个平行节点标签
  - .next_siblings 迭代类型，返回按照HTML文本顺序的后续所有平行节点标签
  - .previous_siblings 迭代类型，返回按照HTML文本顺序的前续所有平行节点标签

##### 内容查找方法

- <>.find_all(name, attrs, recursive, string, **kwargs)
  - name : 对标签名称的检索字符串
  - attrs: 对标签属性值的检索字符串，可标注属性检索
  - recursive: 是否对子孙全部检索，默认True
  - string: <>…</>中字符串区域的检索字符串
    - `<tag>`(..) 等价于 `<tag>`.find_all(..)
    - soup(..) 等价于 soup.find_all(..)
- 扩展方法：
  - <>.find() 搜索且只返回一个结果，同.find_all()参数
  - <>.find_parents() 在先辈节点中搜索，返回列表类型，同.find_all()参数
  - <>.find_parent() 在先辈节点中返回一个结果，同.find()参数
  - <>.find_next_siblings() 在后续平行节点中搜索，返回列表类型，同.find_all()参数
  - <>.find_next_sibling() 在后续平行节点中返回一个结果，同.find()参数
  - <>.find_previous_siblings() 在前序平行节点中搜索，返回列表类型，同.find_all()参数
  - <>.find_previous_sibling() 在前序平行节点中返回一个结果，同.find()参数

##### 案例

- 中国大学排名定向爬取
- url = ‘http://www.zuihaodaxue.cn/zuihaodaxuepaiming2019.html’

```python
import pandas as pd
import numpy as np
import requests
from bs4 import BeautifulSoup

url = 'http://www.zuihaodaxue.cn/zuihaodaxuepaiming2019.html'
res = requests.get(url)
res.encoding = 'utf-8'
soup = BeautifulSoup(res.text,'lxml')
trs = soup.find('tbody',class_="hidden_zhpm").find_all('tr')
range_nums = []
names = []
scores = []
for tr in trs:
    td = tr.find_all('td')
    range_num = td[0].text
    name = td[1].text
    score = td[3].text
    range_nums.append(range_num)
    names.append(name)
    scores.append(score)
data = {
    '排名':range_nums,
    '学校名称':names,
    '总分':scores   
}
df = pd.DataFrame(data)
df[:30]
```

结果输出

![1587560218886](C:\Users\Vodka\AppData\Roaming\Typora\typora-user-images\1587560218886.png)

#### 正则表达式re

##### 介绍

典型的搜索和替换操作要求您提供与预期的搜索结果匹配的确切文本。虽然这种技术对于对静态文本执行简单搜索和替换任务可能已经足够了，但它缺乏灵活性，若采用这种方法搜索动态文本，即使不是不可能，至少也会变得很困难。

通过使用正则表达式，可以：

```python
- 测试字符串内的模式。
    例如，可以测试输入字符串，以查看字符串内是否出现电话号码模式或信用卡号码模式。这称为数据验证。
- 替换文本。
    可以使用正则表达式来识别文档中的特定文本，完全删除该文本或者用其他文本替换它。
- 基于模式匹配从字符串中提取子字符串。
    可以查找文档内或输入域内特定的文本。
```

可以使用正则表达式来搜索和替换标记。

- 正则表达式是用来简洁表达一组字符串的表达式
- 正则表达式是一种通用的字符串表达框架
- 正则表达式是一种针对字符串表达“简洁”和“特征”思想的工具
- 正则表达式可以用来判断某字符串的特征归属

##### 常用操作符

- `.` 表示任何单个字符
- `[ ]` 字符集，对单个字符给出取值范围 ，如`[abc]`表示a、b、c，`[a‐z]`表示a到z单个字符
- `[^ ]` 非字符集，对单个字符给出排除范围 ，如`[^abc]`表示非a或b或c的单个字符
- `*` 前一个字符0次或无限次扩展，如abc* 表示 ab、abc、abcc、abccc等
- `+` 前一个字符1次或无限次扩展 ，如abc+ 表示 abc、abcc、abccc等
- `?` 前一个字符0次或1次扩展 ，如abc? 表示 ab、abc
- `|` 左右表达式任意一个 ，如abc|def 表示 abc、def
- `{m}` 扩展前一个字符m次 ，如ab{2}c表示abbc
- `{m,n}` 扩展前一个字符m至n次（含n） ，如ab{1,2}c表示abc、abbc
- `^` 匹配字符串开头 ，如^abc表示abc且在一个字符串的开头
- `$` 匹配字符串结尾 ，如abc$表示abc且在一个字符串的结尾
- `( )` 分组标记，内部只能使用 | 操作符 ，如(abc)表示abc，(abc|def)表示abc、def
- `\d` 数字，等价于`[0‐9]`
- `\w` 单词字符，等价于`[A‐Za‐z0‐9_]`

##### 主要功能函数

- re.search() 在一个字符串中搜索匹配正则表达式的第一个位置，返回match对象
  - re.search(pattern, string, flags=0)
- re.match() 从一个字符串的开始位置起匹配正则表达式，返回match对象
  - re.match(pattern, string, flags=0)
- re.findall() 搜索字符串，以列表类型返回全部能匹配的子串
  - re.findall(pattern, string, flags=0)
- re.split() 将一个字符串按照正则表达式匹配结果进行分割，返回列表类型
  - re.split(pattern, string, maxsplit=0, flags=0)
- re.finditer() 搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象
  - re.finditer(pattern, string, flags=0)
- re.sub() 在一个字符串中替换所有匹配正则表达式的子串，返回替换后的字符串
  - re.sub(pattern, repl, string, count=0, flags=0)
  - flags : 正则表达式使用时的控制标记：
    - re.I --> re.IGNORECASE : 忽略正则表达式的大小写，`[A‐Z]`能够匹配小写字符
    - re.M --> re.MULTILINE : 正则表达式中的^操作符能够将给定字符串的每行当作匹配开始
    - re.S --> re.DOTALL : 正则表达式中的.操作符能够匹配所有字符，默认匹配除换行外的所有字符
- `.*` Re库默认采用贪婪匹配，即输出匹配最长的子串
- `*?` 只要长度输出可能不同的，都可以通过在操作符后增加?变成最小匹配

##### 案例

- 淘宝商品比价定向爬虫
- url = ‘[https://s.taobao.com/search?q=书包&js=1&stats_click=search_radio_all%25](https://s.taobao.com/search?q=书包&js=1&stats_click=search_radio_all%)’

```python
import re
import requests

def gethtml(url,page):
    '''发送请求，获得相应数据'''
    headers = {
        'user-agent':
        'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.113 Safari/537.36',
        'cookie':
        '_samesite_flag_=true; cookie2=19ea704f8eea5f913380243c158f5441; t=23b995e59da527df81cbac4a779c0c54; _tb_token_=e57b3317551; cna=O+4mF8Sc4XUCAW8S0jHsq83a; sgcookie=E22YW4HOBcTZ9RkC3VPH9; unb=720397092; uc3=nk2=D8rkuJBAYDzMt9Y%3D&vt3=F8dBxGR2VD2t1HffWFo%3D&lg2=W5iHLLyFOGW7aA%3D%3D&id2=VAMTd3hxwHm6; csg=be40c8ed; lgc=libertywave; cookie17=VAMTd3hxwHm6; dnk=libertywave; skt=5a1677daedce1709; existShop=MTU4NzU0NDIyMA%3D%3D; uc4=nk4=0%40Dene%2Bg4c%2BmGh%2Fz%2BMmzYura5TDoUX4A%3D%3D&id4=0%40VhpOmO3HqvSgIITBggOs1dcA0Xc%3D; publishItemObj=Ng%3D%3D; tracknick=libertywave; _cc_=URm48syIZQ%3D%3D; _l_g_=Ug%3D%3D; sg=e20; _nk_=libertywave; cookie1=VqghBYaCCxo%2B%2BT0FhkFqA6Kjzc4MeH%2BTSLx4PFz%2BeGw%3D; enc=A1wQ7V3mWP%2Fr7VROVf%2BhewX4aDMZOO3KgBDssEg9IY3Af2K%2Bxv%2B0EvRAb3EwFffDKZwWQGIiRMbTIRdX8GY3Zg%3D%3D; tfstk=cBNGBIGFtRk12ge_PFG_kij-v96daOWZfSPUT57Z4TOEDKVZ7sD9LLW6WwmTjSpf.; hng=CN%7Czh-CN%7CCNY%7C156; thw=cn; uc1=cookie16=Vq8l%2BKCLySLZMFWHxqs8fwqnEw%3D%3D&cookie21=WqG3DMC9FxUx&cookie15=UtASsssmOIJ0bQ%3D%3D&existShop=false&pas=0&cookie14=UoTUPcllLinFCw%3D%3D; mt=ci=86_1; v=0; l=eBjo1aolQ2E6cicDBO5CPurza77TZQAbzoVzaNbMiIHca1JR1iHCnNQcDiMwRdtjgt5j4eKyIQLleReyJPz38xgKqelyRs5mpFv68e1..; isg=BBYWuRUMVEHd_GDlsNIgKkoPZ8wYt1rxCuRq1oB9S_lYQ7Td6UcDAGBx29-vb1IJ; JSESSIONID=FFF700F41C47A05F95643C4C8EB08103'
    }
    params = {
        'q':'书包',
        's':44*page
        
    }
    try:
        res = requests.get(url,params=params,headers=headers)
        res.raise_for_status()
        res.encoding = 'utf-8'
        return res.text
    except:
        return '产生异常'


def parsehtml(html):
    '''通过正则解析数据'''
    glist = []
    price_list = re.findall(r'view_price":"(.*?)","view_fee', html)
    name_list = re.findall(r'raw_title":"(.*?)","pic_url":', html)
    for i, j in zip(price_list, name_list):
        glist.append([i, j])

#         print(glist)
    return glist


def printlist(info_list):
    '''结果输出，也可使用Pandas'''
    tplt = "{:^5}\t{:^10}\t{:^50}"
    print(tplt.format("序号", "商品价格", "商品名称"))
    count = 0
    for info in info_list:
        count += 1
        print(tplt.format(count, info[0], info[1]))
        
url = 'https://s.taobao.com/search?'
page = 3
info_list = []
for i in range(page):
    html = gethtml(url,page)
    info_list.extend(parsehtml(html))
printlist(info_list)
print('已完成')
```

结果输出

```python
 序号  	 商品价格   	                        商品名称                       
  1  	 1498.00  	      【明星同款】英国AnythingStudio小学生书包双肩包英伦日本风超轻       
  2  	  239.00  	         安踏双肩包2020潮牌大容量旅行背包男休闲简约学生书包运动背包男         
  3  	  139.00  	        阿迪达斯学生书包男女包初中高中大学生电脑包运动双肩背包FI7968         
  4  	  289.00  	      Samsonite/新秀丽儿童背包可爱卡通动物造型学生书包3D双肩包 U22      
  5  	  169.00  	       kk树小学生拉杆书包1-3-6年级女童6-12周岁儿童公主防水轻便背包        
  6  	  139.00  	          花花公子男士双肩包时尚潮流休闲初中学生书包大学生电脑旅行背包         
  7  	  169.00  	          稚行护脊书包儿童小学生男孩女孩潮牌书包趣味减负小孩上学双肩包         
  8  	  639.00  	          暇步士背包男双肩包真皮大容量休闲商务电脑包时尚潮流旅行包书包         
  9  	  498.00  	        ito双肩包男商务设计师电脑包14寸女潮流书包ins风小背包时尚百搭        
 10  	  179.00  	        TYAKASHA塔卡沙双肩包郊游系列学生上学户外旅行男女双肩背包书包        
 11  	  169.00  	         双肩包男士背包应急雨衣大容量出差旅行17寸笔记本电脑包商务书包          
 12  	 1388.00  	       英国埃涅瑟小学生书包进口品牌超轻儿童女1-3-4-6年级HIPPO经典款       
 13  	  269.90  	        迪卡侬防水双肩包旅行休闲包书包运动背包男女潜水户外骑行ITIWIT         
 14  	  389.00  	      NIKE耐克双肩包男包女包2020新款运动包学生书包旅行包背包BA6164       
 15  	  129.00  	        迪士尼小学生书包减负轻便儿童超轻1-3-4-6三到六年级护脊女男童5        
 16  	  159.90  	         迪卡侬运动双肩包电脑背包书包男女健身包旅行包休闲商务新款FICA         
 17  	  289.00  	        Viney双肩包女韩版百搭ins原宿大容量百搭背包书包时尚简约双肩包        
 18  	  488.00  	      香港tigerfamily小学生护脊书包 男女5-9年级初中学生减负双肩包       
 19  	  580.00  	      Samsonite/新秀丽2020新款双肩包女小包 时尚潮流百搭书包女TQ4      
 20  	  219.00  	       Hype双肩包男女背包2020新款韩版时尚百搭ins高中校园大学生书包        
 21  	  59.00   	         巴布豆旗舰店书包1-3年级护脊减负儿童书包男4-6小学生书包轻便         
 22  	  169.00  	         爱迪生男士双肩包2020新款超大容量旅行防水背包学生休闲电脑书包         
 23  	  369.00  	         迪士尼商店 唐老鸭85周年背包超萌可爱双肩包儿童书包小学生背包          
 24  	  169.00  	          不莱玫迪士尼合作款唐老鸭卡通可爱双肩包书包女韩版高中小背包男         
 25  	  998.00  	       Fion/菲安妮休闲双肩包潮流学生书包 2020新款女包尼龙黑色旅行包        
 26  	  259.00  	          爱登堡休闲商务双肩包女简约轻便14寸电脑包书包多功能旅游背包          
 27  	  229.00  	        NIKE耐克双肩包2020新款男包女包高中大学生书包大容量运动包背包
```

