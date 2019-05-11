# Bilibili 视频数据分析
![Language](https://img.shields.io/badge/Language-Python-yellowgreen.svg?style=flat-square)
![Data Storage Size](https://img.shields.io/badge/Data%20Storage%20Size%20-20.66GB-critical.svg?style=flat-square)
![Data Total Size in Memory](https://img.shields.io/badge/Data%20Total%20Size%20in%20Memory-44.77GB-informational.svg?style=flat-square)
![Number of Documents](https://img.shields.io/badge/Number%20of%20Documents-51746026-9bf.svg?style=flat-square)

本项目是使用 Redis + Scrapy 开发的集群式爬虫, 爬取了视频 Av 号 __1 - 51746026__ 的所有视频信息
- [Spider 爬虫](#Spider-爬虫)
    - [爬虫环境](#爬虫环境)
    - [工作流程](#工作流程)
- [数据分析](#数据分析)
	- [视频状态概况](#视频状态概况)
	- [视频质量分析](#视频质量分析)
	- [视频分类分析](#视频质量分析)
	- [视频分类年度变化图](#视频分类年度变化图)
	- [视频数量年度变化分析](#视频数量年度变化分析)
	- [视频投稿数量 Top 50](#视频投稿数量-top-50)
	- [视频收藏数量 Top 50](#视频收藏数量-top-50)
	- [视频分享数量 Top 50](#视频分享数量-top-50)
	- [视频弹幕数量 Top 50](#视频弹幕数量-top-50)
	- [视频投币数量 Top 50](#视频投币数量-top-50)
	- [视频标题分词分析](#视频标题分词分析)

    
项目块说明

目录 | 作者 | 说明
-|-|-
Video Count By Year | [@WuXieXie](https://github.com/WuXieXie) | 年度稿件数量 
Video Quantity Ranking | [@dirname](https://github.com/dirname) | 投稿数量 Top 50 
Video Ratio | [@WuXieXie](https://github.com/WuXieXie) | 稿件有效率 
Video Tag Ratio | [@WuXieXie](https://github.com/WuXieXie) | 稿件类型占比 
Video Tag Ratio By Year | [@WuXieXie](https://github.com/WuXieXie) | 年度稿件类型占比 

## Spider 爬虫
使用 Redis 管理递增 Av 号, 能减少网络堵塞等特定情况下数据的遗漏, 并允许多台机器同时工作爬行

### 爬虫环境
+ MongoDB Community 4.0
+ Scrapy 1.5.2
+ Redis 5.0.3
+ Python 3.7.3
+ CentOS 7

### 工作流程

![Spider chart](flow-chart.png)

## 数据分析
数据清洗完毕后, 数据概况
+ 总数据条数 `51746026` 条
+ 数据遗漏条数为 `0` 条
+ 采集字段为完整 `http://api.bilibili.com/x/web-interface/view?aid={$av}` 响应内容, 如下
```json
{"code":0,"message":"0","ttl":1,"data":{"aid":7777,"videos":2,"tid":30,"tname":"VOCALOID·UTAU","copyright":2,"pic":"http://i0.hdslb.com/bfs/archive/f6e477c256b53be9b973dcc8d6e0fc708b52a3c3.jpg","title":"[7777的怨念]リア充爆発しろ","pubdate":1274427960,"ctime":1497345655,"desc":"P1sm8630148手繪版 P2 sm8613443原曲 リア充=現充，即現實生活充實，大多數有妹子，成天沐浴在陽光之下的人生淫家，反義詞即我們這種成天坐在電腦前的家裡蹲死宅 ，此曲則是nico眾死宅發洩對現充表示詛咒、嘲諷(還有x慕)等等情緒的曲子。投稿一周即再生10w突破","state":0,"attribute":49152,"duration":93,"rights":{"bp":0,"elec":0,"download":1,"movie":0,"pay":0,"hd5":0,"no_reprint":0,"autoplay":1,"ugc_pay":0,"is_cooperation":0,"ugc_pay_preview":0},"owner":{"mid":925,"name":"911","face":"http://i1.hdslb.com/bfs/face/f08bbe3e475caad4f557237bb2ebb4b8c53e4e84.jpg"},"stat":{"aid":7777,"view":20135,"danmaku":259,"reply":309,"favorite":280,"coin":34,"share":49,"now_rank":0,"his_rank":0,"like":123,"dislike":0},"dynamic":"","cid":11638,"dimension":{"width":0,"height":0,"rotate":0},"no_cache":false,"pages":[{"cid":11638,"page":1,"from":"vupload","part":"sm8630148 手绘版","duration":93,"vid":"","weblink":"","dimension":{"width":0,"height":0,"rotate":0}},{"cid":11639,"page":2,"from":"vupload","part":"sm8613443 原曲","duration":90,"vid":"","weblink":"","dimension":{"width":0,"height":0,"rotate":0}}],"subtitle":{"allow_submit":false,"list":[]}}}
```

### 视频状态概况

__code 状态码__

使用聚合得到

![Code_Status](code-status.png)

占比图

![Code Plot diagram](code-plot.png)

### 视频质量分析

根据 `code = 0` 的占比, 同样可得出, 具有权限可观看的视频稿件数量为 __34,148,523__ 个, 视频不见了 `code -404` 的数量为 __4,184,122__ 个, 稿件不可见 `code=62002` 的数量为 __13,276,033__ 个

占比图

![Video Plot diagram](Video%20Ratio/稿件有效率.png)

能访问的视频达到 __66 %__ , 说明哔哩哔哩的视频内容是充实的

### 视频分类分析

总共有 __96 种__ 类别, 其中数量最多的是 `单机游戏`, 具体数量参考 [Video Tag Ration/data.txt](https://github.com/dirname/Bilibili-Av-Videos-Analysis/blob/master/Video%20Tag%20Ratio/data.txt)

图表

![Video Tag Ratio](Video%20Tag%20Ratio/稿件类型占比.png)

饼图

![Video Tag Ratio](Video%20Tag%20Ratio/show.png)

#### 视频分类年度变化图

![Video Tag Ratio](Video%20Tag%20Ratio%20By%20Year/稿件年占比.png)

### 视频数量年变化分析

年份 | 视频数量
-|-
2009 | 466
2010 | 23502
2011 | 79054
2012 | 123293
2013 | 240073
2014 | 425229
2015 | 857852
2016 | 2540810
2017 | 6326099
2018 | 14477304
2019 | 9054916

变化折线图

![Video Count of year](Video%20Count%20By%20Year/年稿件数.png)

可以看到从 __2009年__ - __2019年5月7日 19:34:52__, __2018年__ 的视频数量达到 `14,477,304`个, 处大幅度增长的趋势

### 视频投稿数量 Top 50

UID | 昵称 | 投稿数量
-|-|-
4652742 | leoleeyana | 38191
381738 | M.Scarlet | 24297
2370944 | 多玩首发帝 | 18449
477132 | TAKERA | 18257
382675 | 全麦面包V5 | 17800
535734 | 空想水晶诅咒绯红锁链 | 17251
4005201 | YCZGR | 16682
136001 | ldj7 | 14491
956291 | TF2Video | 14288
6049045 | 井尻晏菜 | 13506
10618733 | Oriequat | 11300
278174078 | daheyouda | 10758
221648 | 柚子木字幕组 | 10734
19653 | 小清水亜美 | 10344
60631692 | 非凡娱乐 | 9889
326632524 | 一点空间ydkj400 | 9592
18094086 | 楊沫沫 | 9532
111187301 | 特摄宅男狼少主 | 9258
44537723 | asdbao | 9147
390668899 | 好猫电娱 | 8910
29246903 | 柏原Dellen | 8746
40904879 | 磨蹭蹭蘑菇 | 8677
1324413 | 牛牛蝎羯 | 8633
2610451 | gy缪德塞克 | 8431
4676168 | Yikori | 8363
12730937 | jojo2w | 8257
928123 | 哔哩哔哩番剧 | 8168
1904149 | darkvroverSC2 | 8109
260439888 | 民福康三甲科主任 | 8044
22141207 | 独游魔盒 | 7738
8607832 | 丫头琦 | 7726
350227828 | 一点空间教育官网 | 7535
268536810 | 火播君 | 7461
429251 | 大宝剑联盟 | 7145
11060066 | 偶像船长 | 7027
11025317 | 斑鸠心平气和everyday | 6989
22972219 | yanghao98 | 6897
14997238 | DJTAKERA | 6848
51606885 | VIC56_CC | 6634
12691609 | BeautiesClub | 6619
797614 | 赫_一号机 | 6556
2062760 | 一把近战都不给六花 | 6508
60058 | 阿尔法小分队字幕组 | 6380
90452309 | 林北是国民女装大佬 | 6357
101710446 | 叭乐叭乐 | 6300
40568377 | 黄大咖2016 | 6294
325406 | 一直喵OneStraightCat | 6180
768041 | CJ木 | 6146
2303935 | GP81每日KPOP | 6088
218084 | 鱼翅旺馒 | 6075

图表

![Video Ranking](Video%20Quantity%20Ranking/show.png)

###  视频收藏数量 Top 50

|排行|av号|标题|投稿人|收藏数|
|----|----|----|----|----|
|1|36570401|【哔哩哔哩2019拜年祭】|哔哩哔哩弹幕网|1460526|
|2|19390801|【春晚鬼畜】赵本山：我就是念诗之王！【改革春风吹满地】|小可儿|1408150|
|3|21061574|【面筋哥×波澜哥】我的烤面筋，融化你的心！|还有一天就放假了|899792|
|4|1250357| 【古筝】千本樱——你可见过如此凶残的练习曲|墨韵Moyun|817003|
|5|3327562|【全明星Rap】黑喂狗！|伊丽莎白鼠|793577|
|6|40672186|英文版《改革春风吹满地》！！Chinese people so 牛逼！|A路人|791207|
|7|3534854|【更新1p，告别蝴蝶袖】美丽芭蕾 天鹅臂 累断手|废柴家里蹲|748051|
|8|810872|【炮姐/AMV】我永远都会守护在你的身边！|暗猫の祝福|677984|
|9|5407024|【中字】美丽芭蕾 狂瘦上半身合集 1.天鹅臂瘦肩膀 2.瘦手臂后侧（甩走蝴蝶袖）3.新娘纤臂（哑铃）|你瞅什么瞅_|673143|
|10|2271112|【循环向】跟着雷总摇起来！Are you OK！|Mr.Lemon|661565|
|11|13662970|【爱情/动画】你的名字。【2016】|bilibili电影|654096|
|12|4489689|【中字】Ballet Beautiful 美丽芭蕾第三套  瘦腿提臀|pll25586|581494|
|13|6235951|【魔道祖师】同道殊途【豪华人声版】|括号君|579223|
|14|2274779|【灵魂rap】如何让孩子爱上♂学习？|伊丽莎白鼠|574913|
|15|17027625|【哔哩哔哩2018拜年祭】|哔哩哔哩弹幕网|537303|
|16|8775849|五分钟在家瘦腰运动！快速瘦肚子小蛮腰，马甲线一周现形！适合初学者【周六野Zoey】|周六野Zoey|534216|
|17|4033926|【电音单曲】我是papi酱|伊丽莎白鼠|516923|
|18|1731700|【综漫 ASMV】 弱者（完整版）/The Weak (Full ver.)|概念の天使|501468|
|19|18193325|【五五开】穷开挂|茶几君梦二|473307|
|20|2648921|【鬼畜全明星】双♂人♂舞|伊丽莎白鼠|470447|
|21|39425207|她哭的那个瞬间 全世界都输了|鬼才才才|469969|
|22|170001|【MV】保加利亚妖王AZIS视频合辑|冰封.虾子|460920|
|23|46996647|【漫威/DC/踩点/1080p】前方高能！使人起鸡皮疙瘩的踩点与衔接的视觉盛宴！|轩龙鸽鸽|450222|
|24|6338954|央视直播失误爆笑集锦完整版|孤傲之巅|442468|
|25|4490081|【中字】Ballet Beautiful 美丽芭蕾 第四套 终极瘦腿|pll25586|435942|
|26|6117110|【极乐净土】咬人猫/有咩酱/赤九玖❤155小分队o(*≧▽≦)ツ|=咬人猫=|432381|
|27|6482189|【高能Rap】你从未看过的家有儿女|伊丽莎白鼠|431861|
|28|11395090|最有效的学生党减脂，每天只要15分钟 48小时持续燃脂|站在强迫之巅的少女|415710|
|29|4606803|主播真会玩鬼畜篇01：我是全英雄联盟最骚的骚猪!|起小点是大腿|410866|
|30|8879609|快速瘦小腿运动！摆脱萝卜腿让小腿更细长，拉伸小腿【周六野Zoey】|周六野Zoey|406411|
|31|49775093|【官方】吴亦凡《大碗宽面》MV（动画版）|大家的音乐姬|400365|
|32|3905462|【2016拜年祭单品】九九八十一【乐正绫 feat.洛天依】|没有龟壳的乌龟|397780|
|33|49842011|《复联4》前必看！一口气看完21部漫威电影，完整的时间线剧情讲解！|努力的Lorre|389021|
|34|5800106|【目前最全中字共69集附课表】美丽芭蕾Ballet Beautiful天鹅臂瘦腿瘦腹提臀背部雕刻全身燃脂今天我们都是废鹅|站在强迫之巅的少女|387266|
|35|35198390|【英雄联盟】POP/STARS - K/DA女团最新MV助力英雄联盟S8|英雄联盟|384112|
|36|3060477|【日语课程】标日初级精讲BY萌萌哒葉子先生（叶子老师完整课程及后续中高级课程请看详情介绍）|葉子先生酱|378372|
|37|4548006|【Photoshop 教程】史上最容易听懂的PS入门基础教程（更新完毕：多的是你不知道的事）|doyoudo|376126|
|38|2023391|【成龙】我的洗发液|绯色toy|369747|
|39|3037947|【小明&老王】此物天下绝响|此物天下绝响|369365|
|40|3521416|【哔哩哔哩2016拜年祭】|哔哩哔哩弹幕网|364081|
|41|1111459|这大概是最好的日语入门教学了吧----五十音学习|新河|363036|
|42|45560472|郭德纲于谦相声100段，睡前必听！|夜雨学堂爱相声|347625|
|43|1930028|【真·耳机福利】泽野弘之七曲融合剪辑，「拔剑」领衔|Riven兰格雷|345728|
|44|12384003|力荐！！【中英双语】Ballet Beautiful 美丽芭蕾 P5 消灭大腿外侧赘肉！塑形！|pll25586|343423|
|45|2098846|【去违和】周杰伦献唱核爆神曲aLIEz 与霍元甲的remix版本 aHUOz！|Bi哩Bi哩汪|341426|
|46|45629276|【老番茄】史上最骚杀手(第一集)|老番茄|339614|
|47|3818869|【康熙说唱王朝】部落到帝国·怒斥群臣|非桥段|339566|
|48|37866145|【1080/漫威/衔接踩点向】前方高能！带你体验完美流畅的衔接踩点视觉盛宴！|轩龙鸽鸽|337838|
|49|5263217|【小明v老王】大忠若奸|此物天下绝响|330763|
|50|48138432|【老番茄】史上最骚杀手(第三集)|老番茄|330263|

### 视频分享数量 Top 50

|排行|av号|标题|投稿人|分享数|
|----|----|----|----|----|
|1|19390801|【春晚鬼畜】赵本山：我就是念诗之王！【改革春风吹满地】|小可儿|868475|
|2|17027625|【哔哩哔哩2018拜年祭】|哔哩哔哩弹幕网|534283|
|3|40215859|BNJ-ZP测试用|默默的狗1|519200|
|4|40672186|英文版《改革春风吹满地》！！Chinese people so 牛逼！|A路人|494185|
|5|21061574|【面筋哥×波澜哥】我的烤面筋，融化你的心！|还有一天就放假了|474912|
|6|170001|【MV】保加利亚妖王AZIS视频合辑|冰封.虾子|421271|
|7|36570401|【哔哩哔哩2019拜年祭】|哔哩哔哩弹幕网|368176|
|8|3327562|【全明星Rap】黑喂狗！|伊丽莎白鼠|326014|
|9|49842011|《复联4》前必看！一口气看完21部漫威电影，完整的时间线剧情讲解！|努力的Lorre|318521|
|10|2274779|【灵魂rap】如何让孩子爱上♂学习？|伊丽莎白鼠|304747|
|11|6235951|【魔道祖师】同道殊途【豪华人声版】|括号君|292341|
|12|25211748|【纪录片】《人生一串》第一集之《无肉不欢》|哔哩哔哩纪录片|240243|
|13|39237975|我不仅开口说话和你吵架，还要把你周围的空气吃干净让你窒息...|狂风桑|236826|
|14|3534854|【更新1p，告别蝴蝶袖】美丽芭蕾 天鹅臂 累断手|废柴家里蹲|233040|
|15|810872|【炮姐/AMV】我永远都会守护在你的身边！|暗猫の祝福|221590|
|16|16716848|重庆版的小猪佩奇来了|果子哥哥工作室|218233|
|17|40180906|[中字] 如何唱出Lemon的精髓 @ガーリィレコード|神亞亞|213389|
|18|26361000|【7月】工作细胞 01【独家正版】|哔哩哔哩番剧|212002|
|19|6338954|央视直播失误爆笑集锦完整版|孤傲之巅|211268|
|20|18193325|【五五开】穷开挂|茶几君梦二|204228|
|21|3521416|【哔哩哔哩2016拜年祭】|哔哩哔哩弹幕网|202379|
|22|6482189|【高能Rap】你从未看过的家有儿女|伊丽莎白鼠|199432|
|23|41114598|某小学文化男子竟用文言文翻译出了你好骚啊|丰兄来了|196462|
|24|49775093|【官方】吴亦凡《大碗宽面》MV（动画版）|大家的音乐姬|195970|
|25|10729179|波澜哥原版视频 素材|jason-T0dad|191606|
|26|2271112|【循环向】跟着雷总摇起来！Are you OK！|Mr.Lemon|187744|
|27|2648921|【鬼畜全明星】双♂人♂舞|伊丽莎白鼠|185588|
|28|1250357| 【古筝】千本樱——你可见过如此凶残的练习曲|墨韵Moyun|180036|
|29|31252459|电视剧《火力种田王2》——悠悠球个人无规则格斗赛|泽野螳螂|177956|
|30|4606803|主播真会玩鬼畜篇01：我是全英雄联盟最骚的骚猪!|起小点是大腿|177319|
|31|8247204|【最强卖鞋哥】这双王八牌皮鞋，我买定了！|伊丽莎白鼠|176817|
|32|48155210|每天一遍，防止抑郁。|侯爵Marquis|168063|
|33|48788630|蔡徐坤被篮球暴打|黑玛瑙|164960|
|34|13662970|【爱情/动画】你的名字。【2016】|bilibili电影|161798|
|35|4033926|【电音单曲】我是papi酱|伊丽莎白鼠|156912|
|36|5407024|【中字】美丽芭蕾 狂瘦上半身合集 1.天鹅臂瘦肩膀 2.瘦手臂后侧（甩走蝴蝶袖）3.新娘纤臂（哑铃）|你瞅什么瞅_|156827|
|37|3770834|过♂年|泽野螳螂|151322|
|38|6643355|黑社会砸网吧 我从53秒笑到1分30秒|10969Kaze|149828|
|39|12755553|Hop MV版|系统空间不足ToT|149604|
|40|1358913|【合集】我们仍未知道那天所看见的花的名字|哔哩哔哩番剧|149433|
|41|42357872|【大事件】总结2018感谢有你陪伴，2019我们再出发！暴走·大事件·第六季· 05|暴走漫画|148798|
|42|16874218|【中文八级】两个国人展开了惊人的英语八级对话|某幻君|148762|
|43|35945993|【冷面作品】《痔青春·上集》孙笑川 X 吴亦凡|冷面Lim|148320|
|44|48828477|【蔡徐坤】放荡不羁电音之舞|枪弹轨迹|147762|
|45|20358400|【面筋哥】流浪兄弟烤面筋原素材，当他一开口全世界都兴奋了|王友锋|147081|
|46|21340266|人才！这是一个我看了从头笑到尾的视频哈哈哈哈哈|超级可爱鸡|145679|
|47|28879552|【王境泽】卡路里|Ryuuuuuuu|144896|
|48|8709991|【满汉】还不是因为你长得不好看【纯男声30P】震惊！29位大汉竟对狗做出这种事！！|满汉全席音乐团队|144520|
|49|40165534|深度揭露杨永信背后400亿“戒网瘾”市场，恶行堪比莆田系医院！|燃茶哥哥在此|143282|
|50|9372087|洛天依，言和 原创《达拉崩吧》|ilem|141560|

### 视频弹幕数量 Top 50

|排行|av号|标题|投稿人|弹幕数|
|----|----|----|----|----|
|1|3934631|【合集】某科学的超电磁炮 第一季|哔哩哔哩番剧|3699304|
|2|7248433|【哔哩哔哩2017拜年祭】|哔哩哔哩弹幕网|3552984|
|3|2203040|【合集】四月是你的谎言|哔哩哔哩番剧|3512480|
|4|17027625|【哔哩哔哩2018拜年祭】|哔哩哔哩弹幕网|3427109|
|5|4371182|【合集】魔卡少女樱 / 百变小樱|哔哩哔哩番剧|2907321|
|6|1561567|【散人】大型励志剧 娱乐圈小助理养成计划（4.18更新 番外叶琛线 叔叔的诡计 ）|逍遥散人|2384960|
|7|3521416|【哔哩哔哩2016拜年祭】|哔哩哔哩弹幕网|2368962|
|8|1925377|【合集】罪恶王冠 Guilty Crown|哔哩哔哩番剧|2306412|
|9|3749039|【合集】魔法禁书目录 第一季|哔哩哔哩番剧|2288945|
|10|2983852|【合集】某科学的超电磁炮S（第二季）|哔哩哔哩番剧|2213987|
|11|1999286|【哔哩哔哩2015拜年祭】|哔哩哔哩弹幕网|2208639|
|12|1358913|【合集】我们仍未知道那天所看见的花的名字|哔哩哔哩番剧|1849842|
|13|23273494|【合集】我的英雄学院 第二季|哔哩哔哩番剧|1798941|
|14|419366|【720P合集】中二病也要谈恋爱(附BD隐藏13话)【澄空字幕组】|空灵雨迹|1786757|
|15|3440269|【国产】亮剑   全30集   【2005】|迷影社|1769317|
|16|1358900|【合集】我的妹妹不可能那么可爱|哔哩哔哩番剧|1694267|
|17|9202079|【耽美BL】重生之《霸道总裁爱上我》换脸复仇|骷髅盟主|1666698|
|18|6175933|【合集】龙与虎！|哔哩哔哩番剧|1651867|
|19|3300397|【合集】LoveLive！第一季|哔哩哔哩番剧|1612111|
|20|148850|日常 00-26全【满弹幕合辑】|傲娇酱|1525045|
|21|9426860|【合集】约会大作战|哔哩哔哩番剧|1477072|
|22|3997347|【合集】魔法禁书目录Ⅱ 第二季|哔哩哔哩番剧|1476494|
|23|4044639|【合集】冰菓 Hyouka【独家正版】|哔哩哔哩番剧|1466573|
|24|2606270|【高级弹幕游戏】弹幕躲避|下限Nico|1449468|
|25|3316724|【合集】LoveLive！第二季|哔哩哔哩番剧|1442167|
|26|36570401|【哔哩哔哩2019拜年祭】|哔哩哔哩弹幕网|1412260|
|27|3277237|【旧版】游戏王DM（高清片源）|武藤亚图姆|1408764|
|28|1913027|【合集】夏目友人帐|哔哩哔哩番剧|1271365|
|29|1358922|【合集】我女友与青梅竹马的惨烈修罗场|哔哩哔哩番剧|1260094|
|30|5582780|【4月】Re：从零开始的异世界生活 18|TV-TOKYO|1220600|
|31|1817764|【AB向】同属性二次元人物PK【抖腿】|某科学的超电磁铁|1206675|
|32|12346469|【喜剧/家庭】家有儿女第一季 100集全【2004】|迷影社|1179890|
|33|15321105|【合集】我的英雄学院 第一季|哔哩哔哩番剧|1116531|
|34|1612165|【童年大合集】你以为我会给你放喜羊羊？|高铁老湿基|1116076|
|35|810872|【炮姐/AMV】我永远都会守护在你的身边！|暗猫の祝福|1102632|
|36|3060477|【日语课程】标日初级精讲BY萌萌哒葉子先生（叶子老师完整课程及后续中高级课程请看详情介绍）|葉子先生酱|1075503|
|37|1594747|【国产】【悬疑/爱情】红色 48集全（2014）（未删减版）（岁月静好 现世安稳）|花卷好吃|1048729|
|38|1029730|【弹幕游戏】名副其实的弹幕游戏|剧情帝|1045720|
|39|596381|【老E】这不是解说+回忆录+游戏蒙太奇+游戏评测+求生之路+无标题全系列|真誠最高|1028450|
|40|9654289|【合集】NO GAME NO LIFE 游戏人生|哔哩哔哩番剧|1015698|
|41|2478326|【合集】青春之旅|哔哩哔哩番剧|1013053|
|42|1999475|【国产】少年包青天 第一部 40集全【 2000】|迷影社|974175|
|43|4696940|【合集】野良神 第一季【独家正版】|哔哩哔哩番剧|967813|
|44|13662970|【爱情/动画】你的名字。【2016】|bilibili电影|964859|
|45|566480|【APH/RPG/合集】黑塔鬼 想与你一起逃离 正篇1-17前+番外+小休止|Young°|948124|
|46|2616233|【7月】雏蜂序章 上|有妖气动漫|931707|
|47|18089528|【SBS综艺】Running Man 2018合集完结（E384.180107~E432.181230）【TSKS】|一只南音呀|892627|
|48|3944301|【合集】境界的彼方|哔哩哔哩番剧|892436|
|49|423414|【白月字幕组满弹幕720p】邻座的怪同学【完结】|残星什么的就是残星|853028|
|50|1118201|【更新6P】莫名其妙就跟着唱了|科学超电磁炮F|844202|

### 视频投币数量 Top 50

|排行|av号|标题|投稿人|投币数|
|----|----|----|----|----|
|1|36570401|【哔哩哔哩2019拜年祭】|哔哩哔哩弹幕网|2044148|
|2|19390801|【春晚鬼畜】赵本山：我就是念诗之王！【改革春风吹满地】|小可儿|1921413|
|3|26725241|【敖厂长10】烂尾的游戏冒险(雅达利寻剑)|敖厂长|1420181|
|4|12719263|【敖厂长】史上最垃圾游戏判明!|敖厂长|1359501|
|5|17027625|【哔哩哔哩2018拜年祭】|哔哩哔哩弹幕网|1323910|
|6|40672186|英文版《改革春风吹满地》！！Chinese people so 牛逼！|A路人|1152588|
|7|21061574|【面筋哥×波澜哥】我的烤面筋，融化你的心！|还有一天就放假了|1095054|
|8|1250357| 【古筝】千本樱——你可见过如此凶残的练习曲|墨韵Moyun|1088638|
|9|7454054|【哔哩哔哩2017拜年祭】|拜年祭2017|941276|
|10|42321016|剧情刺激！场面爆笑！这谁顶得住啊！2019一月新番吐槽大盘点!|LexBurner|831883|
|11|6018306|【敖厂长】打脸!魂斗罗水下八关存在|敖厂长|815652|
|12|45629276|【老番茄】史上最骚杀手(第一集)|老番茄|808987|
|13|48138432|【老番茄】史上最骚杀手(第三集)|老番茄|802220|
|14|10310868|【敖厂长】让你耳朵怀孕的FC游戏|敖厂长|801688|
|15|3521416|【哔哩哔哩2016拜年祭】|哔哩哔哩弹幕网|782546|
|16|42466533|喜羊羊是一部怎样的动画？巅峰时期的喜羊羊与灰太狼有多强|不会甜的喜糖|748682|
|17|40165534|深度揭露杨永信背后400亿“戒网瘾”市场，恶行堪比莆田系医院！|燃茶哥哥在此|716186|
|18|43972960|【70w粉丝，激励计划是多少？】一天花完是什么体验？|宝剑嫂|713898|
|19|3327562|【全明星Rap】黑喂狗！|伊丽莎白鼠|662837|
|20|49775093|【官方】吴亦凡《大碗宽面》MV（动画版）|大家的音乐姬|651199|
|21|46295706|【老番茄】史上最骚杀手(第二集)|老番茄|648998|
|22|42357872|【大事件】总结2018感谢有你陪伴，2019我们再出发！暴走·大事件·第六季· 05|暴走漫画|637975|
|23|42038965|千万不要跟声优斗表情包，否则你将毫无胜算⑨|声鱼片儿|623824|
|24|810872|【炮姐/AMV】我永远都会守护在你的身边！|暗猫の祝福|576018|
|25|34160882|【李云龙】沙漠骆驼（全程高燃）|采紫葳的凌霄子|558712|
|26|44769755|【李云龙】出山|采紫葳的凌霄子|548450|
|27|39425207|她哭的那个瞬间 全世界都输了|鬼才才才|545388|
|28|17257576|【敖厂长】只能在白天通关的游戏|敖厂长|541897|
|29|49534383|【老番茄】史上最骚杀手(第四集)|老番茄|527834|
|30|39573474|【六小龄童】Something Just Like This|柴可夫司徒|522413|
|31|49842011|《复联4》前必看！一口气看完21部漫威电影，完整的时间线剧情讲解！|努力的Lorre|521646|
|32|7550874|【敖厂长】约P软件暗藏骗局|敖厂长|507825|
|33|33096112|何为营销号？用爱发电对UP主而言意味着什么？网络原创生态的窘迫现状及其成因【动话说#02】|低调的帅爷|503459|
|34|50276557|【老番茄】史上最骚杀手(第五集)|老番茄|497753|
|35|4033926|【电音单曲】我是papi酱|伊丽莎白鼠|491690|
|36|6235951|【魔道祖师】同道殊途【豪华人声版】|括号君|488736|
|37|32552326|为什么老外把中餐送到宇宙里？|信誓蛋蛋|484658|
|38|31252459|电视剧《火力种田王2》——悠悠球个人无规则格斗赛|泽野螳螂|484103|
|39|46996647|【漫威/DC/踩点/1080p】前方高能！使人起鸡皮疙瘩的踩点与衔接的视觉盛宴！|轩龙鸽鸽|483236|
|40|49470985|【鸡你太美】王妃|史蒂芬王富贵|472672|
|41|30165885|吐槽美国 ：为什么有钱人和流浪汉住在同一个小区？|信誓蛋蛋|460594|
|42|42250009|八重樱 · 桃源恋歌【次世代卡通渲染第三弹】|scyrax|454688|
|43|18193325|【五五开】穷开挂|茶几君梦二|450850|
|44|46900196|这集vlog我们拍了十年，致最美好的青春|AresserA-Vlog|447032|
|45|39715823|耗时一年用MC建出硬派赛博朋克城市-minecraft史诗工程【未知】-AZTTER|MC-AZTTER|446819|
|46|47403722|随机挑战！这是我做过最后悔的决定了！|中国BOY超级大猩猩|446025|
|47|23232168|【敖厂长】27年前竟有如此震撼的游戏|敖厂长|439222|
|48|47587498|《鬼畜宝藏》第一期——蓝洞外挂|泽野螳螂|433711|
|49|37866145|【1080/漫威/衔接踩点向】前方高能！带你体验完美流畅的衔接踩点视觉盛宴！|轩龙鸽鸽|427954|
|50|41114474|【什么鬼套路】LOL偷钱最脏套路 BOSS级别的老妇女|点滴菌|419804|

### 视频标题分词分析

Analyzing, coming soon........


### 视频弹幕、硬币、评论等属性数量分析

Analyzing, coming soon........