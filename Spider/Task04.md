#### 腾讯网热点新闻爬取

- 1.采用Selenium抓取数据，较为简单，重点在于滚轮滑动(JS注入)

```python
from selenium import webdriver
from lxml import etree
import time
url = 'https://news.qq.com/'
bro = webdriver.Chrome()
bro.get(url)
for i in range(5):
    bro.execute_script('window.scrollTo(0,document.body.scrollHeight)')
    time.sleep(3)
res = bro.page_source

tree = etree.HTML(res)
lis = tree.xpath('//ul[@class="list"]/li')
for li in lis:
    try: 
        title = li.xpath('./div[@class="detail"]/h3/a/text()')[0]
        url = li.xpath('./div[@class="detail"]/h3/a/@href')[0]
        print(title,url)
    except:
        continue
```

------

- **2.获取动态加载地址并发起请求(常用)**

- Ajax动态加载
- 找到动态加载页面url
- page参数需放入到url中，否则只能获取到第一页数据
  - **参数列表page一直为0(这是个坑，个人理解为一种反爬措施)**
- 结果保存到DataFrame中，并写入到csv文件
- 列名包括：'标题','父类别', '子类别','标签', '文章来源', '显示类型', '更新时间','链接'

```python
import requests
import time
import pandas as pd
import numpy as np
pre_url = 'https://pacaio.match.qq.com/irs/rcd?page='
headers = {
    'user-agent':
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.122 Safari/537.36'
}

info_list = []
for i in range(10):
    try:
        url = pre_url + str(i)
        page = str(i)
        params = {
            'cid': '137',
            'token': 'd0f13d594edfc180f5bf6b845456f3ea',
            'id': '',
            'ext': 'top',
            'expIds': '',
            # 'callback': '__jp1'
        }
        res = requests.get(url=url, headers=headers, params=params).json()
        for info in res['data']:
            result = {}
            result['标题'] = info['title']
            result['链接'] = info['vurl']
            result['父类别'] = info['category1_chn']
            result['子类别'] = info['category2_chn']
#             result['关键字'] = info['keywords']
            result['显示类型'] = info['showtype']
            result['文章来源'] = info['source']
            result['标签'] = info['tags']
            result['更新时间'] = info['update_time']
            info_list.append(result)
        time.sleep(2)
    except:
        print(i)
df = pd.DataFrame(info_list,columns=['标题','父类别', '子类别','标签', '文章来源', '显示类型', '更新时间','链接'])
df.index = np.arange(1,len(df)+1)
df.to_csv('QQ_news.csv')
print('数据采集完成')
```

结果输出

```python
,标题,父类别,子类别,标签,文章来源,显示类型,更新时间,链接
1,调查组定性河南4儿童土方内身亡事件：系人身伤亡刑事案件,社会,奇闻趣事,儿童,腾讯新闻,zt,2020-04-19 20:19:40,https://new.qq.com/zt/template/?id=TWF2020041900315800
2,广西边境查获13名非法入境外国人，现场画面曝光,其它,刑事犯罪,广西,梨视频,article,2020-04-25 21:12:51,https://new.qq.com/omn/20200425/20200425V0M94M00.html
3,河北广宗饮用水异常原因查明：3名嫌疑人被控制 企业法人投案自首,社会,刑事犯罪,广宗县;河北,成都商报红星新闻,article,2020-04-25 19:29:17,https://new.qq.com/omn/20200425/20200425A0MGXN00.html
4,长江流域抗生素污染调查：儿童孕妇尿液中检出多种抗生素,健康,学术健康,抗生素;长江流域;免疫;长江,瞭望,article,2020-04-25 21:58:05,https://new.qq.com/omn/20200425/20200425A07RED00.html
5,公安部对13人发A级通缉令已有6人落网，22岁女逃犯东莞被抓,社会,刑事犯罪,公安部;逃犯;东莞;金彩霞;陈春友,南方都市报,article,2020-04-25 21:04:03,https://new.qq.com/omn/20200425/20200425A0OHMS00.html
6,镉大米重现：“毒大米”为何屡禁不绝？,社会,食品安全,毒大米;市场监督管理局;益阳;大米;镇雄县,新京报评论,article,2020-04-25 17:09:15,https://new.qq.com/omn/20200425/20200425A0IYB100.html
7,青岛小珠山两处山火复燃 地形陡峭难以靠近救援队无奈折返,其它,其它,,新京报我们视频,article,2020-04-25 20:26:11,https://new.qq.com/omn/20200425/20200425V0NS7V00.html
8,湖南省2天5人被查，省纪委监委：反腐没有“暂停键”,时政,国内,湖南省纪委监委;反腐;湖南;刘和生;陈泽珲;巡视组,澎湃新闻,article,2020-04-25 16:25:52,https://new.qq.com/omn/20200425/20200425A0H7CO00.html
9,公积金要取消？多地减缓缴存！争议公积金存废：14.6万亿规模怎么替代,房产,政策法规,公积金;住房公积金贷款;商业贷款,21世纪经济报道,article,2020-04-25 16:31:25,https://new.qq.com/omn/20200425/20200425A0B4EQ00.html
10,“常阳患者”会传染吗？援鄂一线专家六问释疑,健康,疾病防治,苑晓冬,新京报,article,2020-04-25 16:28:28,https://new.qq.com/omn/20200425/20200425A0DOOF00.html
11,80后海归女老板：三个月损失超九千万 亲自开车送快餐,其它,其它,,中国人的一天,article,2020-04-25 16:23:17,https://new.qq.com/omn/20200424/20200424V0HGZU00.html
12,多地明确暑假时间，部分地区延期近一个月,教育,小学教育,暑假;中小学;特教学校,新京报,article,2020-04-25 18:37:57,https://new.qq.com/omn/20200425/20200425A0LHFY00.html
13,河北保定一家四口遇害：嫌疑人系住对门的亲戚，两家曾有矛盾,社会,刑事犯罪,保定;王会强;清苑;魏村,楚天都市报,article,2020-04-25 18:09:23,https://new.qq.com/omn/20200425/20200425A0KRTE00.html
14,海航流动性危机发酵：海航基础业绩大变脸，剩余地产资产归属将现终局,财经,沪深主板,海南省海航集团;海航基础;地产,每日经济新闻,article,2020-04-25 17:13:16,https://new.qq.com/omn/20200425/20200425A0IVM000.html
15,江西景德镇客车侧翻致6死事故画面曝光 客车避让轿车后失控,社会,交通事故,车祸;客车;江西景德镇,新京报我们视频,article,2020-04-25 10:52:46,https://new.qq.com/omn/20200424/20200424V0Q2K300.html
16,美国共和党“积极攻击中国”备忘录曝光，华春莹发推反击,时政,国际,美国共和党;华春莹;美国_时政,环球网,article,2020-04-25 16:13:25,https://new.qq.com/omn/20200425/20200425A0GRZ100.html
17,专家：武汉“封城”让全球减少1200万-4200万感染者,社会,慈善,封城;刘远立;武汉;北京协和医学院,经济观察报,article,2020-04-25 19:12:34,https://new.qq.com/omn/20200425/20200425A0LWXL00.html
18,伊朗首射军星后，美伊在波斯湾“危险接近”会否升级？,军事,环球军情,伊朗_军事;美伊;波斯湾;伊朗伊斯兰革命卫队;特朗普;美国海军,澎湃新闻,article,2020-04-25 15:10:57,https://new.qq.com/omn/20200425/20200425A0EQQR00.html
19,最新发现！鼻子或是新冠病毒感染的“毒库”,健康,学术健康,新冠病毒感染;鼻子;细胞,健康时报客户端,article,2020-04-25 13:21:44,https://new.qq.com/omn/20200425/20200425A0A5CC00.html
20,疫情中的日本黑帮：大发口罩财，比政府更早进入紧急状态,社会,国际社会,日本_社会;山口组;新冠病毒;新冠疫情,凤凰星,article,2020-04-25 14:23:50,https://new.qq.com/omn/20200424/20200424A0SWME00.html
21,台媒：美军驱逐舰通过台湾海峡后，反潜机又经巴士海峡进入南海,军事,军舰,巴士海峡;美国海军;台湾海峡;南海;反潜机;驱逐舰,环球时报国内,article,2020-04-25 13:36:07,https://new.qq.com/omn/20200425/20200425A0AUUP00.html
22,世卫组织：无证据表明新冠肺炎康复者能避免二次感染,健康,学术健康,新冠肺炎;世卫组织,新京报,article,2020-04-25 18:05:57,https://new.qq.com/omn/20200425/20200425A0KM8100.html
23,25省发布一季度GDP数据：湖北跌出万亿俱乐部，23省增速超全国,财经,宏观经济,gdp;gdp增速,澎湃新闻,article,2020-04-25 17:05:53,https://new.qq.com/omn/20200425/20200425A0IKFA00.html
24,河北保定发生重大刑案警方悬赏5万缉凶 村民：一家四人被杀,社会,刑事犯罪,保定;缉凶;悬赏,新京报我们视频,article,2020-04-25 09:49:15,https://new.qq.com/omn/20200425/20200425V05VKJ00.html
25,兰州娱乐场所陆续复工：限流限时营业 一客一消杀,社会,社会百态,兰州;城关区;ktv,中国新闻网,article,2020-04-25 16:59:03,https://new.qq.com/omn/20200425/20200425A0IE7S00.html
26,加拿大从中国采购口罩“不达标”？中国驻加使馆回应,时政,国内,使馆;医疗物资;中方县;多伦多市,新京报,article,2020-04-25 13:25:53,https://new.qq.com/omn/20200425/20200425A0AV7U00.html
27,云南旱情历史罕见：137条河道断流 201座水库干涸,时政,国内,水库;云南;云南省水利厅;普洱市;旱情,央视新闻客户端,article,2020-04-25 13:35:53,https://new.qq.com/omn/TWF20200/TWF2020042500352200.html
28,“五一”假期如何出行？各地景区恢复政策大汇总,旅行,出境游,旅游景区;杭州西湖景区,澎湃新闻,article,2020-04-25 13:00:19,https://new.qq.com/omn/20200425/20200425A05Z3500.html
29,疑因护士抱错致“错换人生28年” 涉事医院：若是医院的错绝不护短,社会,社会百态,河南大学淮河医院;开封市;郭郭,上游新闻,article,2020-04-25 14:56:59,https://new.qq.com/omn/20200425/20200425A0EHWH00.html
30,青岛小珠山山火已明显减弱：千余人彻夜扑救 已不见明火,社会,灾害事件,小珠山,梨视频,article,2020-04-25 11:43:32,https://new.qq.com/omn/20200425/20200425V06UUA00.html
31,有关后新冠时代的13个判断：世界将大不一样（下）,房产,政策法规,新冠病毒;美国_科技;数据中心,中国新闻周刊,article,2020-04-25 07:38:42,https://new.qq.com/omn/20200424/20200424A0N6WS00.html
32,部分途经墨西哥转机中国公民被遣返回出发地，中使馆发紧急提醒,时政,国内,墨西哥_社会;使馆,成都商报红星新闻,article,2020-04-25 13:05:16,https://new.qq.com/omn/20200425/20200425A05KWR00.html
33,张文宏回应“早餐喝不喝粥”争议：有讨论就是好事,科学,科普,张文宏;新冠疫苗,人民日报,article,2020-04-25 15:20:08,https://new.qq.com/omn/KLS20200/KLS2020042500429000.html
34,“甘肃庆阳女生遭猥亵坠亡”当事教师获刑2年 禁止从事相关职业3年,社会,刑事犯罪,猥亵;庆阳;甘肃省人民检察院;刑事申诉复查决定书,南方都市报,article,2020-04-25 15:03:04,https://new.qq.com/omn/20200425/20200425A0EGU900.html
35,全国高速公路道口恢复“落杆”状态，仍然免费通行,时政,重大民生,高速公路;通行费,上游新闻,article,2020-04-25 05:58:22,https://new.qq.com/omn/20200424/20200424A0SXDK00.html
36,被公安部通缉的“98年生女逃犯”已被刑拘：涉嫌参与传销非法拘禁,社会,刑事犯罪,a级通缉令;金彩霞;公安部;广东,南方都市报,article,2020-04-25 09:46:36,https://new.qq.com/omn/20200424/20200424A0UHUR00.html
37,滞留巴厘岛的中国人：航班多次取消，孕妇等到快生娃，仍不知归期,旅行,出境游,巴厘岛;中国驻登巴萨总领馆;印尼鹰航;印度尼西亚_社会,南方都市报,article,2020-04-25 10:55:02,https://new.qq.com/omn/20200425/20200425A07ZFW00.html
38,谷雨｜他每天往返70公里跨省上班 还不忘走前老婆床头放杯热水,社会,社会百态,北京;固安;燕郊;北漂;买房;通勤;谷雨;上班族;打工;跨省;跨城,腾讯新闻谷雨实验室,article,2020-04-25 14:30:41,https://new.qq.com/omn/20200425/20200425A063VT00.html
39,内蒙古：所有入境人员一律实行“14＋14＋2＋1”管控措施,时政,重大民生,内蒙古;新冠肺炎疫情;核酸检测;呼和浩特;口岸,中国新闻网,article,2020-04-25 06:01:33,https://new.qq.com/omn/20200425/20200425A0309A00.html
40,抗疫期间顶风公款聚餐 江苏响水公检法一把手齐被问责,时政,国内,江苏响水;沈爱东;县检察院;公安局;响水县;汪登波,澎湃新闻,article,2020-04-25 09:13:38,https://new.qq.com/omn/20200425/20200425A04IW800.html
41,抗疫期间顶风公款聚餐 江苏响水公检法一把手齐被问责,时政,国内,江苏响水;沈爱东;县检察院;公安局;响水县;汪登波,澎湃新闻,article,2020-04-25 09:13:38,https://new.qq.com/omn/20200425/20200425A04IW800.html
42,婴儿趴睡致死知情人曝惊人内幕：有话术洗脑压制母亲不能去帮孩子,社会,社会百态,发酵,F视频,article,2020-04-25 13:03:26,https://new.qq.com/omn/20200424/20200424V0SG5F00.html
43,一线丨小鹏汽车向特斯拉发严正声明：过去一年一直被骚扰、霸凌,汽车,科技,小鹏汽车;特斯拉;腾讯新闻;源代码;曹光植,腾讯新闻潜望,article,2020-04-25 10:13:58,https://new.qq.com/omn/20200425/20200425A06R4500.html
44,较真丨宝宝趴着睡能预防呕吐、助消化？这个“育儿经”真的靠不住,育儿,喂养,宝宝;婴儿;婴儿猝死综合征;趴睡;呼吸;助消化,较真,article,2020-04-25 10:06:22,https://new.qq.com/omn/20200425/20200425A05DO600.html
45,河北邢台多个村庄饮用水异常：发黄起沫村民被灼伤 鱼虾大片死亡,社会,社会百态,调查;村庄;污染,青蜂侠,article,2020-04-25 07:11:35,https://new.qq.com/omn/20200424/20200424V0T56E00.html
46,国家防办：松花江流域、海河流域、黄河流域中上游等可能发生较重汛情,时政,重大民生,淮河流域;松花江;黄河流域;应急管理部;海河,新华社新闻,article,2020-04-25 06:03:48,https://new.qq.com/omn/20200424/20200424A0U8IL00.html
47,“抗体血浆”竟成特效药！“换血”会成富人“续命爆款”吗？,健康,学术健康,瑞维拓;抗体;核苷酸,南风窗,article,2020-04-25 08:00:00,https://new.qq.com/omn/20200424/20200424A0SNOC00.html
48,贵阳三月大女婴熟睡时被老鼠咬伤  事发时奶奶正在屋外洗衣服,社会,社会百态,女婴;老鼠;咬伤,新京报我们视频,article,2020-04-24 17:10:22,https://new.qq.com/omn/20200424/20200424V0KWTZ00.html
49,河南河北两副省长先后被公诉：一个喝酒只喝茅台 一个搞迷信活动,时政,国内,省长;徐光;南一;河北省人民政府;茅台;李谦,封面新闻,article,2020-04-24 20:46:15,https://new.qq.com/omn/20200424/20200424A0RTYK00.html
50,被周扬青直接点名与罗志祥有染，蝴蝶姐姐发文道歉：诚心说声对不起,娱乐,港台娱乐,周扬青;罗志祥,腾讯新闻,zt,2020-04-26 00:00:10,https://new.qq.com/zt/template/?id=ENT2020042300235800
51,谷雨数据丨喊麦真的该被骂吗？分析100首麦词我们发现了这些,社会,社会百态,喊麦;东北地区;老铁;mc六道;美人;杨坤;谷雨;惊雷,腾讯新闻谷雨数据,article,2020-04-24 20:09:17,https://new.qq.com/omn/20200424/20200424A0F7GE00.html
52,公安部对13名涉黑恶逃犯发布A级通缉令 最小女逃犯生于1998年,社会,刑事犯罪,公安部;a级通缉令;逃犯;涉黑;身份证号码;陈春友,公安部刑侦局,article,2020-04-24 20:31:55,https://new.qq.com/omn/TWF20200/TWF2020042400906500.html
53,董明珠透露格力电器近况：一季度少卖300亿元，为呵护员工不会裁员,科技,科技大佬,董明珠;格力电器;格力;裁员;空气净化器,硅谷分析狮,article,2020-04-25 07:29:55,https://new.qq.com/omn/20200425/20200425A03RLC00.html
54,华春莹发推：中国提供美国的口罩，够每个美国人分7个,时政,国际,华春莹;美国_社会,观察者网,article,2020-04-24 19:40:48,https://new.qq.com/omn/20200424/20200424A0Q5H300.html
55,五一小长假将至，“报复性”旅游会来吗？,旅行,出境游,旅游景区;旅游消费,中国经济周刊,article,2020-04-24 23:28:06,https://new.qq.com/omn/20200424/20200424A0QGWY00.html
56,"上海楼市躁动：看房电话打300次才接通 有项目涉50万""茶水费""",财经,房地产业,深圳楼市,腾讯新闻,zt,2020-04-25 08:46:38,https://new.qq.com/zt/template/?id=FIN2020041900663300
57,黑人的噩梦：为民主党“度身打造”的病毒与特朗普的策略,时政,国际,民主党;共和党;covid-19,tuzhuxi,article,2020-04-24 19:34:59,https://new.qq.com/omn/20200424/20200424A0Q3JO00.html
58,江西丰城发电厂致73死事故宣判：28人获刑，工程总指挥判了18年,法规,法规,丰城市;发电站;江西,央视新闻,article,2020-04-24 17:55:29,https://new.qq.com/omn/TWF20200/TWF2020042400770300.html
59,济南一村支书甩开上访者致其骨折 获刑一年不服向高院申诉,社会,社会百态,申诉;村支书,新京报我们视频,article,2020-04-25 06:58:16,https://new.qq.com/omn/20200424/20200424V0TZKX00.html
60,黑龙江又现医院感染聚集性病例，已有14人感染包括两名医护人员,健康,两性健康,牡丹江市康安医院;无症状感染者;黑龙江卫健委;黑龙江;牡丹江;二级医院,北京日报客户端,article,2020-04-24 15:54:47,https://new.qq.com/omn/20200424/20200424A0HGR900.html
61,网传武汉一乘客红码乘车致全车人被隔离，官方回应,社会,社会百态,武汉,界面新闻,article,2020-04-24 16:50:19,https://new.qq.com/omn/20200424/20200424A0JJIC00.html
62,山东“省考”大幅扩招 今年公务员扩招会是趋势吗？,教育,教育之公务员,公务员;山东;国家公务员考试;笔试;考生;李曼卿,中国新闻网,article,2020-04-24 20:17:58,https://new.qq.com/omn/20200424/20200424A0R7HS00.html
63,四川副校长性侵事件：多名男生称曾遭体罚 给女学生的疑似情书曝光,其它,其它,体罚;性侵;情书;校长;四川,看度新闻,article,2020-04-24 17:21:49,https://new.qq.com/omn/20200424/20200424V0LI7T00.html
64,媒体：质疑张文宏可以，别扣“崇洋媚外”的帽子,科学,科普,张文宏;疫情;饮食;早餐,中青评论,article,2020-04-24 17:18:10,https://new.qq.com/omn/20200424/20200424A0LKYI00.html
65,三沙此时新设两个区，有何玄机？,时政,重大民生,三沙市;西沙群岛;南海;主权,环球时报评论,article,2020-04-24 17:36:37,https://new.qq.com/omn/20200424/20200424A0M85N00.html
66,辽宁一医生新冠病毒血清抗体IgM检测阳性 曾瞒报赴哈尔滨行程,健康,心理,辽宁省人民医院;新冠病毒;黑龙江省疾控中心;哈尔滨医科大学;辽宁,中国新闻网,article,2020-04-24 17:01:02,https://new.qq.com/omn/20200424/20200424A0GQ4Z00.html
67,北京新任命两名副市长 一个月内形成“一正十副”格局 ,时政,国内,北京;十五届人大常委会;北京市公安局;杨晋柏;中国南方电网;亓延军,新京报,article,2020-04-24 15:39:52,https://new.qq.com/omn/20200424/20200424A0G81100.html
68,缅甸武装冲突3发炮弹落入中国境内 有学校建筑物及车辆被子弹击中,军事,中国军情,武装冲突3;炮弹;缅甸;武装冲突;姐告,央视军事,article,2020-04-24 12:12:08,https://new.qq.com/omn/TWF20200/TWF2020042400478500.html
69,央行：中国城镇家庭住房拥有率96% 净资产均值289万,房产,楼盘销售,资不抵债;中央银行;住房;资产;金融;资产负债率,21世纪经济报道,article,2020-04-24 16:59:29,https://new.qq.com/omn/FIN20200/FIN2020042400562100.html
70,热点丨静默对抗示威者却被骂“你是病毒” 美国女护士霸气回应,其它,其它,示威者;护士,新京报我们视频,article,2020-04-24 15:16:12,https://new.qq.com/omn/20200424/20200424V0FONY00.html
71,美报告称越南黑客对中国防疫部门发动网络攻击，外交部回应,时政,重大民生,外交部;外交部发言人;越南_时政;耿爽;中方县,澎湃新闻,article,2020-04-24 16:27:40,https://new.qq.com/omn/20200424/20200424A0HXZI00.html
72,国产HPV疫苗5月起可接种 价格低于进口疫苗一半,社会,医疗新闻,宫颈癌疫苗;疫苗,光明网,article,2020-04-24 16:56:34,https://new.qq.com/omn/20200424/20200424A0K8K500.html
73,3名公务员借名买卖46套安置房被判贪污，律师：还有近百人也买了,房产,政策法规,拆迁安置房;余姚市;童鸿烽;拆迁;马丽萍,津云锋声,article,2020-04-24 13:54:05,https://new.qq.com/omn/20200424/20200424A0ACMR00.html
74,李国庆谈育儿丨整天吵架还不如离婚，别给孩子留下阴影,育儿,有娃家庭,李国庆;孩子;离婚;阴影,腾讯育儿,article,2020-04-24 14:58:40,https://new.qq.com/omn/BAB20200/BAB2020042400605500.html
75,镉米重出江湖？云南镇雄称销毁的近百吨湖南益阳大米系镉超标,财经,交通运输,市场监督管理局;云南镇雄;大米;益阳;云南;镇雄县,澎湃新闻,article,2020-04-24 12:05:27,https://new.qq.com/omn/20200424/20200424A096D200.html
76,煤炭反腐风暴下的内蒙古：10余厅官密集被查，一天3高官落马,时政,国内,内蒙古;反腐;内蒙古自治区党委;内蒙古纪委监委;纪委;李永先,中国新闻周刊,article,2020-04-24 12:43:24,https://new.qq.com/omn/20200424/20200424A0B94J00.html
77,2020大学生就业力报告出炉：偏爱新经济行业 平均期望薪酬6930元,教育,高校,2020大学生就业力报告;毕业,上游新闻,article,2020-04-24 14:34:08,https://new.qq.com/omn/20200424/20200424A0EOW500.html
78,一次性实现“绕落巡”三步走！中国首次火星探测为啥这么牛？,科学,太空,火星;火星车;探测器;北京航天飞行控制中心;月球;中国航天科技集团公司,科技日报,article,2020-04-24 11:10:03,https://new.qq.com/omn/20200424/20200424A08MQL00.html
79,用扬声器袭警罪名成立，香港立法会前议员被判140小时社会服务令,时政,港澳台时政,区诺轩;香港立法会;议员;高振邦;关志豪,环球网,article,2020-04-24 15:05:00,https://new.qq.com/omn/20200424/20200424A0G6MW00.html
80,耿爽喊话蓬佩奥：不要以为表态加个“可能”就可以任意栽赃,时政,国内,蓬佩奥;耿爽;武汉市卫健委;世卫组织;中方县,环球网,article,2020-04-24 16:24:08,https://new.qq.com/omn/20200424/20200424A0IRQM00.html
81,“口罩熔喷布之乡”造富神话破灭：生厂商称“一夜之间，血本无归”,社会,国际社会,熔喷布;扬中市;孙祺,财经杂志优选,article,2020-04-24 16:13:42,https://new.qq.com/omn/20200424/20200424A0IABB00.html
82,外籍人员李亨利勾结境外反华势力插手香港事务 被移送检察院起诉,社会,国际社会,李亨利;侦查终结;国安局;广州市人民检察院;国家安全局,南方PLUS,article,2020-04-24 09:44:39,https://new.qq.com/omn/20200424/20200424A02RBA00.html
83,由于疫情80个国家和关税地区实施出口限制 WTO呼吁提高透明度,时政,重大民生,世界贸易组织;出口限制;出口禁令和限制,第一财经,article,2020-04-24 14:36:21,https://new.qq.com/omn/20200424/20200424A0EK7000.html
84,瑞德西韦治疗新冠肺炎失败？吉利德科学：数据不足以支撑结论,健康,学术健康,瑞德西韦;吉利德科学;新冠病毒,新京报,article,2020-04-24 13:01:57,https://new.qq.com/omn/20200424/20200424A0BMMI00.html
85,美军驱逐舰昨日穿越台湾海峡 同一天辽宁舰编队现身台东部海域,军事,军舰,台湾海峡;辽宁号;驱逐舰;美国海军;航空母舰;军舰,环球时报国内,article,2020-04-24 10:27:10,https://new.qq.com/omn/20200424/20200424A06YHN00.html
86,民政部：全国各级慈善组织、红十字会已接收疫情防控捐款约419.94亿,社会,慈善,疫情防控;慈善组织;新冠肺炎疫情;民政部,南方都市报,article,2020-04-24 14:02:52,https://new.qq.com/omn/20200424/20200424A0BKNB00.html
87,湖南郴州北湖区原团委书记涉嫌猥亵女企业家 被警方逮捕,时政,重大民生,楚挺征;北湖区;郴州;北湖区委;郴州市公安局;北湖分局,澎湃新闻,article,2020-04-24 11:46:50,https://new.qq.com/omn/20200424/20200424A09K0L00.html
88,同行透露：董卿已从上海返京在央视积极工作，一切安好,娱乐,中国娱乐,董卿;央视;杜恩湖,封面新闻,article,2020-04-24 09:32:42,https://new.qq.com/omn/20200424/20200424A05MF100.html
89,13岁少年疑杀10岁堂妹，父母申请将他送少管所重新做人,社会,刑事犯罪,少管所;梅村村;郎溪县公安局;郎溪县;涛城镇,郑州晚报客户端,article,2020-04-24 08:57:07,https://new.qq.com/omn/20200424/20200424A04GPP00.html
90,定了！中国首次火星探测任务命名为“天问一号”,科学,太空,火星;航天;东方红一号;航天精神;国家航天局;行星,人民日报,article,2020-04-24 10:02:00,https://new.qq.com/omn/20200424/20200424A06GAZ00.html
91,什么将被病毒彻底改变？有关后新冠时代的13个判断（上）,时政,重大民生,新冠疫情;疫情,中国新闻周刊,article,2020-04-24 08:57:56,https://new.qq.com/omn/20200424/20200424A04MZG00.html
92,百度最大规模晋升！APP被罚前夜，提拔3位副总裁和51位总监,科技,科技大佬,百度;副总裁;百度app;微信;张珺;腾讯新闻;潜望,腾讯新闻潜望,article,2020-04-24 07:18:35,https://new.qq.com/omn/20200424/20200424A03C8P00.html
93,今年上半年中小学教师资格考试推迟至下半年一并组织实施,教育,资格考试,中小学教师;笔试;中国教育考试网,新京报,article,2020-04-24 12:02:02,https://new.qq.com/omn/20200424/20200424A0A92L00.html
94,美国科技巨头拟推出“健康码”：和中国版区别在哪 效果会如何？,科技,软件,健康码;谷歌;苹果;蓝牙,财经杂志优选,article,2020-04-24 11:38:37,https://new.qq.com/omn/20200423/20200423A0UC8A00.html
95,谷雨影像丨江南女儿：她40年前被遗弃，12年漫长寻亲终团聚,社会,社会百态,江南女儿;寻亲;蔡凤侠;江阴;弃婴;计划生育;弃女,腾讯新闻谷雨影像,article,2020-04-24 09:49:07,https://new.qq.com/omn/20200424/20200424A03TGB00.html
96,后新冠时期，中国外交需要有更加广阔的胸襟,时政,国内,外交;东盟,中国新闻周刊,article,2020-04-24 09:07:02,https://new.qq.com/omn/20200424/20200424A04VHK00.html
97,东方红一号生日快乐！50岁的它虽然不能再唱歌，但仍能飞行数百年,军事,航空航天,人造卫星;东方红一号;航天;火箭_军事武器;东方红,科技日报,article,2020-04-24 10:32:51,https://new.qq.com/omn/20200424/20200424A075PE00.html
98,猪瘟还没走马瘟又来？农业农村部：警惕非洲马瘟境外传入风险,社会,医疗新闻,猪瘟;非洲马瘟;农业农村部;世界动物卫生组织,每日经济新闻,article,2020-04-24 09:08:02,https://new.qq.com/omn/20200424/20200424A04Z0V00.html
99,中马船只在南海对峙美澳派军舰施压中国？马来西亚回应,军事,环球军情,马来西亚_军事;南海;希沙姆丁;军舰;中方县,环球网,article,2020-04-24 07:31:31,https://new.qq.com/omn/20200424/20200424A03FNX00.html
100,盖茨万字长文解答疫情一切：大规模疫苗接种可能需要18个月,健康,学术健康,比尔·盖茨;疫苗接种;疫情;新冠病毒;感染率;季节性,腾讯科技,article,2020-04-24 08:17:39,https://new.qq.com/omn/TEC20200/TEC2020042400206700.html
```

#### 知乎数据爬取

- url:‘https://www.zhihu.com/search?q=Datawhale&utm_content=search_history&type=content%27’

- 需要登录，暂时无法解决

- 动态加载地址获取

  - Ajax请求地址：**https://www.zhihu.com/api/v4/search_v3?t=general&q=Datawhale&correction=1&offset=0&limit=20&lc_idx=0&show_all_topics=0**
  - 变量

  ```python
  t: general
  q: Datawhale
  correction: 1
  offset: 0
  limit: 20
  lc_idx: 0
  show_all_topics: 0
  ```

  - 部分响应结果

  ```python
  {"paging":{"is_end":false,"next":"https://api.zhihu.com/search_v3?advert_count=0\u0026correction=1\u0026lc_idx=0\u0026limit=20\u0026offset=20\u0026q=Datawhale\u0026search_hash_id=662dfd8f1c48305d30a11297e35b37d0\u0026show_all_topics=0\u0026t=general\u0026vertical_info=0%2C0%2C1%2C0%2C0%2C0%2C0%2C0%2C0%2C1"},"data":[{"type":"search_result","highlight":{"description":"For the learner ，和学习者一起成长!","title":"\u003cem\u003eDatawhale\u003c/em\u003e"},"object":{"id":"1ab3e0838e6df34ba816c8ca15faeeb3","url_token":"datawhale2018","type":"people","url":"https://api.zhihu.com/people/1ab3e0838e6df34ba816c8ca15faeeb3","name":"\u003cem\u003eDatawhale\u003c/em\u003e","user_type":"people","headline":"For the learner ，和学习者一起成长!","gender":-1,"avatar_url":"https://pic3.zhimg.com/50/v2-82ed630a290ab9df4458eabffec17e2b_s.jpg","badge":[],"is_following":false,"is_followed":false,"follower_count":85,"voteup_count":22,"answer_count":6,"articles_count":0,"score":0.46690043807029724,"attached_info_bytes":"OpYBCgtwbGFjZWhvbGRlchIgNjYyZGZkOGYxYzQ4MzA1ZDMwYTExMjk3ZTM1YjM3ZDAYASIJMjMyNDI3NDQzKg1kYXRhd2hhbGUyMDE4QiAxYWIzZTA4MzhlNmRmMzRiYTgxNmM4Y2ExNWZhZWViM0oJRGF0YXdoYWxlUABYAWABmAEAoAEAqAEAsAEBuAEA6AFVgAIBuAIA"},"index":0,"id":1716563451},{"type":"search_result","highlight":{"description":"作者：\u003cem\u003eDatawhale\u003c/em\u003e 关于Kaggle瓜事件，最近得到很多竞赛圈朋友的关注，也导致很多参加竞赛的朋友深受其扰。事实上整个事件是因为kaggle比赛上两个队伍因为提交的sub一样","title":"\u003cem\u003eDatawhale\u003c/em\u003e：愿竞赛圈少一些人身攻击和热点炒作"},"object":{"id":"85401441","title":"\u003cem\u003eDatawhale\u003c/em\u003e：愿竞赛圈少一些人身攻击和热点炒作","type":"article","url":"https://api.zhihu.com/articles/85401441","excerpt":"作者：\u003cem\u003eDatawhale\u003c/em\u003e 关于Kaggle瓜事件，最近得到很多竞赛圈朋友的关注，也导致很多参加竞赛的朋友深受其扰。事实上整个事件是因为kaggle比赛上两个队伍因为提交的sub一样","voteup_count":12,"comment_count":0,"created_time":1570373135,"updated_time":1570373135,"content":"\u003cp\u003e 作者：\u003ca href=\"https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s%3Fsrc%3D11%26timestamp%3D1570372767%26ver%3D1896%26signature%3Di7mQIoDTVmndZVH1Trpou%2AxMThr%2AMqQgSgI%2A5nh%2Ak7lk-FEvRVPXG6adjvEWkfw9Ntxjoe32whVOfsTZ%2A-929ubdoVSLgcnEspOf%2AjdenHoiPrw40ht4aPYc2U%2AcYBYE%26new%3D1\" class=\" wrap external\" target=\"_blank\" rel=\"nofollow noreferrer\"\u003eDatawhale\u003c/a\u003e\u003cbr/\u003e \u003cbr/\u003e \u003c/p\u003e\u003cp\u003e关于Kaggle瓜事件，最近得到很多竞赛圈朋友的关注，也导致很多参加竞赛的朋友深受其扰。事实上整个事件是因为kaggle比赛上两个队伍因为提交的sub一样，被取消了成绩，不管原因是什么，有错认错，该批评则批评。\u003cbr/\u003e \u003c/p\u003e\u003cp\u003e但是杰少，鱼佬，有夕
  ```

  