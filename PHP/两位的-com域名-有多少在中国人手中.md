---
title: '两位的.com域名,有多少在中国人手中?'
date: 2018-07-04 21:58:21
tags: PHP
---

<img src="两位的-com域名-有多少在中国人手中/a.png" width = 70% height = 70% />





域名,是网民接入互联网的桥梁.好的域名,尤其在移动互联网到来之前,形如商业地产中的"金角银边",是最核心的资产.



普通用户到达目标网站无非有如下几种方式:搜索引擎,网址导航或浏览器收藏夹,直接输入域名.显然,最稳妥方式是最后一种.

<img src="两位的-com域名-有多少在中国人手中/c.png" width = 80% height = 80% />

<br>



这其间有太多的故事,如中华网的上市,开心网的陨落,这也是许多商业网站,不惜血本买入域名的原因.

再加上蔡文胜们的发迹,`qq.com,jd.com,mi.com,so.com`, 以及"ali家族"的来龙去脉,ms.com 归属大摩(morganstanley)而非微软(microsoft),这些总为人津津乐道的"网事",都让逐渐不在主流的域名江湖不乏谈资和故事.


<img src="两位的-com域名-有多少在中国人手中/i.png" width = 80% height = 80% />


因为先入为主的"历史原因",company后缀`.com`,是最广泛流行的通用域名,任何一家商业公司,恐怕都梦寐能拥有对应的.com域名.而因为用户输入原因,短域名价值显然悠于长域名,愈短价值愈大.

本文旨在统计所有两位数纯字母和纯数字域名.com域名,也即26`*`26 + 10 `*` 10 = 676 + 100 = 776个,有多少个为中国人所有.

几点说明:

- 1.判断标准:访问该页面,如果源码含中文字符,则认为归属中国人.

- 2.虽持有者为防止名下价值千金的域名被仲裁,大多做一些内容,表征非投资而在正常使用,但也不乏许多国人持有的域名,就处在闲置未解析状态

关于第2点,可查看[kq.com仲裁案](https://news.ename.cn/yuminggushi_19700822_104128_1.html),持有该域名的是我一位山东老乡,据称他还持有4-6个二位纯字母`.com`域名.他曾将 wx.com 做成一个做电器"维修"业务的网站,明显"醉翁之意不在酒".

---

开始编码之前,先将人尽皆知的国内两位字母/数字`.com`的域名列出:

[bl.com](https://www.bl.com)<br>
[qq.com](https://www.qq.com/) <br>
[yy.com](https://www.yy.com/) <br>
[jd.com](https://www.jd.com/) <br>
[mi.com](https://www.mi.com)<br>
[so.com](https://www.so.com) <br>
[le.com](https://www.le.com) <br>
[we.com](https://www.we.com) <br>
[lu.com](https://www.lu.com/) <br>
[ly.com](https://www.ly.com/) <br>

[51.com](https://www.51.com) <br>
[58.com](https://www.58.com) <br>


---

正式编码,选用于我开发效率最高的php,暂不考虑运行效率,
因部分网站如`bl.com`存在主页为二级域名`www.bl.com`的情况,故添加逻辑,`xx.com`或`www.xx.com`,有一者返回值中含中文字符,即认为符合条件

代码如下:

```PHP
<?php

function getLetterDomain()
{
    $arr = [];
    for ($x = ord('a'); $x <= ord('z'); $x++) {
        $one = chr($x);
        for ($y = ord('a'); $y <= ord('z'); $y++) {
            $two = chr($y);
            $domain = $one . $two . ".com";
            array_push($arr, $domain);
        }
    }
    
    $letterFile = "letter.txt";
    if (($file = fopen($letterFile, "w+")) === FALSE) {
        echo("创建可写文件：" . $letterFile . "失败");
        exit();
    }
    echo("创建可写文件" . $letterFile . "成功！");
    
    foreach ($arr as $k => $value) {
        $rs1 = curl($value);
        if (preg_match('/[\x{4e00}-\x{9fa5}]/u', $rs1) === 1) {
            fwrite($file, "第$k 个域名$value 为中文网站");
            fwrite($file, str_repeat("@", 100));
            fwrite($file, "\n");
            continue;
        }

        $rs2 = curl('www.' . $value);
        if (preg_match('/[\x{4e00}-\x{9fa5}]/u', $rs2) === 1) {
            fwrite($file, "第$k 个域名$value 为中文网站");
            fwrite($file, str_repeat("@", 100));
            fwrite($file, "\n");
        }
    }
}


function getNumDomain()
{
    $arr = [];
    for ($x = 0; $x <= 9; $x++) {
        for ($y = 0; $y <= 9; $y++) {
            $domain = $x . $y . ".com";
            array_push($arr, $domain);
        }
    }

    $numFile = "numdomain.txt";
    if (($file = fopen($numFile, "w+")) === FALSE) {
        echo("创建可写文件：" . $numFile . "失败");
        exit();
    }
    echo("创建可写文件" . $numFile . "成功！");

    foreach ($arr as $k => $value) {
        $rs1 = curl($value);
        if (preg_match('/[\x{4e00}-\x{9fa5}]/u', $rs1) === 1) {
            fwrite($file, "第$k 个域名$value 为中文网站");
            fwrite($file, str_repeat("@", 100));
            fwrite($file, "\n");
            continue;
        }

        $rs2 = curl('www.' . $value);
        if (preg_match('/[\x{4e00}-\x{9fa5}]/u', $rs2) === 1) {
            fwrite($file, "第$k 个域名$value 为中文网站");
            fwrite($file, str_repeat("@", 100));
            fwrite($file, "\n");
        }
    }
}

function curl($domain)
{
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $domain);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);

    //若给定url自动跳转到新的url,有了下面参数可自动获取新url内容：302跳转
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);  ### 该条设置十分重要,相当于curl的-L参数;但curl -L 61.com依然无法得到预期返回,上面方法中的添加www操作仍然不能少;但添加该设置后,一般可不用再尝试https,基本都会自动跳过去
    //可参考http://ju.outofmemory.cn/entry/95924 以及 https://www.cnblogs.com/sien6/p/6964227.html

    //可以抓取头信息，并对header信息的状态码（301，302）进行判断。如需跳转，则从Location中获取到Location，再次进行抓取，直至状态码为200状态。最后再对目标页面内容进行抓取https://www.jianshu.com/p/2c4e78fb4328

    // 在尝试连接时等待的秒数
    curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 30);
    // 最大执行时间
    curl_setopt($ch, CURLOPT_TIMEOUT, 30);
    $output = curl_exec($ch);
    curl_close($ch);

    return $output;
}

getLetterDomain();
getNumDomain();

```

---



[使用go重构,见此](http://www.dashen.tech/2018/07/05/%E7%94%A8go%E9%87%8D%E6%9E%84%E4%B8%8A%E7%AF%87%E4%BB%A3%E7%A0%81/)


----


#### ~~!第0 个域名~~ [aa.com](https://www.aa.com/) 


美国航空官网

非中国人持有

<img src="两位的-com域名-有多少在中国人手中/extra41.png" width = 80% height = 80% />


----

#### ~~!第20 个域名~~ [au.com](https://www.au.com/) 

日本某3C电商网站...日本文字中有相当部分和汉字写法一致,于是很好奇,在编码上,这两个看上去一样的文字,会不会对应相同的编码

非中国人持有

<img src="两位的-com域名-有多少在中国人手中/extra29.png" width = 80% height = 80% />


---


#### 第21 个域名 [av.com](http://av.com)

有些惊讶,没想到第一个为国人持有的二位字母.com域名是这个..在我带着一丝忐忑打开时,还好并没有出现预想中的画面^_^

<img src="两位的-com域名-有多少在中国人手中/1.png" width = 80% height = 80% />

---


#### ~~!第26 个域名~~ ~~[ba.com](http://ba.com)~~

该网站为一英文网站,会跳转到YouTube.因为油管相关推荐中含中文,故误认为该域名符合条件.

----

#### ~~!第40 个域名~~ [bo.com](http://bo.com)

英国航空官网

非中国人持有


<img src="两位的-com域名-有多少在中国人手中/extra42.png" width = 80% height = 80% />



---




#### 第48 个域名 [bw.com](https://www.bw.com/) 

少不了中国人身影的币圈 

<img src="两位的-com域名-有多少在中国人手中/extra29.png" width = 80% height = 80% />

----

#### 第51 个域名 [bz.com](http://bz.com/)

这张图片之后还会出现...

<img src="两位的-com域名-有多少在中国人手中/extra7.png" width = 80% height = 80% />


----

#### 第56 个域名 [ce.com](http://ce.com/)


返回值中含`这个网站可出售`

---

#### 第59 个域名 [ch.com](https://www.ch.com/)

春秋航空官网,此域名早知道,但未写在前面


<img src="两位的-com域名-有多少在中国人手中/extra25.png" width = 80% height = 80% />

----


#### ~~!第62 个域名~~ [ck.com](https://www.ck.com/)

`Calvin Klein`服装品牌官网,在华有业务,此域名非中国人所有 



----

#### 第76 个域名 [cy.com](http://www.cy.com/)

原搜狐的游戏事业部

<img src="两位的-com域名-有多少在中国人手中/extra16.png" width = 80% height = 80% />


----

#### 第83 个域名 [df.com](http://df.com/)

或许是马来西亚的网站

<img src="两位的-com域名-有多少在中国人手中/extra14.png" width = 80% height = 80% />


----


#### 第91 个域名 [dn.com](https://www.dn.com/)

杭州也有两位字母的.com域名了...

<img src="两位的-com域名-有多少在中国人手中/extra15.png" width = 80% height = 80% />


----


#### 第109 个域名 [ef.com](http://www.ef.com) 

英孚英语官网

<img src="两位的-com域名-有多少在中国人手中/extra43.png" width = 80% height = 80% />



---

#### ~~!第111 个域名~~ [eh.com](https://www.eh.com/) 

日本一家造酒的企业~

仍然是汉语和日语有部分字完全一样

<img src="两位的-com域名-有多少在中国人手中/extra38.png" width = 80% height = 80% />

---

#### 第135 个域名[ff.com](http://ff.com)

faraday future

下周回国贾跃亭..

<img src="两位的-com域名-有多少在中国人手中/extra39.png" width = 80% height = 80% />

----

#### 第143 个域名 [fn.com](https://www.fn.com/)

FINTECH NEWS,

金融科技类资讯

<img src="两位的-com域名-有多少在中国人手中/extra.png" width = 80% height = 80% />

---

#### 第144 个域名 [fo.com](https://fo.com/why) 


`泰国 数字资产交易平台`,真的是泰国人办的吗..

<img src="两位的-com域名-有多少在中国人手中/extra26.png" width = 80% height = 80% />


---

#### 第162 个域名 [gg.com](http://gg.com/)

以为这个理应在对待域名不惜血本的谷歌手里...(谷歌持有g.cn,亚马逊持有z.cn,京东持有3.cn--当时其主域名为360buy.com)

<img src="两位的-com域名-有多少在中国人手中/2.png" width = 80% height = 80% />

----

#### 第164 个域名 [gi.com](https://gi.com/) 

跳转到摩托罗拉,鉴于被联想购买,所以此域名也算归属于中国(假定该域名属于摩托)

<img src="两位的-com域名-有多少在中国人手中/extra35.png" width = 80% height = 80% />


----


#### 第179 个域名 [gx.com](http://gx.com/)

这张图片之后依然会出现...

<img src="两位的-com域名-有多少在中国人手中/extra8.png" width = 80% height = 80% />

---

#### 第181 个域名 [gz.com](http://www.gz.com/)

深圳某杀马特公司

<img src="两位的-com域名-有多少在中国人手中/3.png" width = 80% height = 80% />


---

#### 第188 个域名 [hg.com](http://hg.com/) 

"欢购",深圳一家电商网站,"专营酒类和滋补品"

<img src="两位的-com域名-有多少在中国人手中/extra21.png" width = 80% height = 80% />


----


#### 第205 个域名 [hx.com](http://hx.com/) 
微信图片++++



----

#### 第218 个域名 [ik.com](http://www.ik.com/)

畅联（IKGlobal）,总部位于香港的一家业务"涉及全球数据中心资源、全球网络专线、国际互联网带宽、IP地址、网络安全及防御清洗中心服务、云计算及数据加速、区块链和专业咨询及项目实施等内容"的公司.

<img src="两位的-com域名-有多少在中国人手中/4.png" width = 80% height = 80% />


---

#### 第231 个域名 [ix.com](https://ix.com/)

疑似属于新加坡

<img src="两位的-com域名-有多少在中国人手中/extra18.png" width = 80% height = 80% />


----

#### 第235 个域名 [jb.com](http://jb.com/)

依然是之前那张出现两次的微信图片...而且还会再出现



----

#### 第244 个域名 [jk.com](http://www.jk.com/)

武汉的一家药品采购网站,界面浓浓山寨风...暴殄这个`健康`简拼的金字域名

<img src="两位的-com域名-有多少在中国人手中/5.png" width = 80% height = 80% />

---

#### 第245 个域名 [jl.com](http://jl.com/)

微信图片



----


#### 第247 个域名 [jn.com](http://jn.com/)

微信图片..

----

#### 第249 个域名 [jp.com](http://jp.com/) 

依然是之前那张出现三次的微信图片...而且还会再出现

----

#### 第262 个域名 [kc.com](http://kc.com/)

`矿池`的简拼.之前做P2P的千金买下`we.com`,现在是链圈币圈的舞台了...

<img src="两位的-com域名-有多少在中国人手中/6.png" width = 80% height = 80% />

---

#### 第263 个域名 [kd.com](http://kd.com/) 

微信图片

----

#### 第264 个域名 [ke.com](https://ke.com/)

贝壳官网

<img src="两位的-com域名-有多少在中国人手中/extra17.png" width = 80% height = 80% />


----


#### 第265 个域名 [kf.com](http://kf.com/)

微信图片...

----


#### 第266 个域名 [kg.com](https://www.kg.com/) 


区块链+++

千氪,36kr,钛媒体,化学元素在媒体中的应用...


<img src="两位的-com域名-有多少在中国人手中/extra17.png" width = 80% height = 80% />


----

#### 第270 个域名 [kk.com](http://kk.com/)

百度的天津子公司,主营游戏业务

<img src="两位的-com域名-有多少在中国人手中/7.png" width = 80% height = 80% />

---

#### 第272 个域名 [km.com](http://km.com/)

上海的一家公司

<img src="两位的-com域名-有多少在中国人手中/8.png" width = 80% height = 80% />

---
#### 第277 个域名 [kr.com](http://kr.com/) 

那张微信图片+..


----

#### 第301 个域名 [lp.com](http://lp.com/)

那张微信图片 ++..

----

#### 第305 个域名 [lt.com](http://lt.com/) 

跳到厦门某域名交易商

<img src="两位的-com域名-有多少在中国人手中/extra28.png" width = 80% height = 80% />


----

#### ~~!第331 个域名~~ [mt.com](https://www.mt.com)

梅特勒-托利多（METTLER TOLEDO）行业领先的精密仪器及衡器制造商与服务提供商，产品应用于实验室、制造商和零售服务业。

应该又是某跨国企业


----

#### ~~!第346个域名~~ [ni.com](http://www.ni.com/)

上海恩艾仪器有限公司,美国国家仪器有限公司(National Instruments Corporation.)在华分公司


---- 

#### 第365 个域名 [ob.com](http://ob.com/)

那张微信图片 +++..

----

#### 第395 个域名 [pf.com](http://pf.com/)

微信图片 +++


----

#### 第399 个域名 [pj.com](https://www.pj.com/)

又一家P2P...

<img src="两位的-com域名-有多少在中国人手中/extra2.png" width = 80% height = 80% />

---

#### 第409 个域名 [pt.com](https://pt.com/index.php?c=content&a=list&catid=1)

此域名可出售

----

#### 第417 个域名 [qb.com](https://www.qb.com/) 

又又一家数字货币...

<img src="两位的-com域名-有多少在中国人手中/extra30.png" width = 80% height = 80% />


----

#### 第419 个域名 [qd.com](http://qd.com/) 

微信图片

----

#### 第420 个域名 [qe.com](http://qe.com/)

`您正在访问的域名可以转让`

---

#### 第425 个域名 [qj.com](http://qj.com/) 

疑似归王新所有..

<img src="两位的-com域名-有多少在中国人手中/extra9.png" width = 80% height = 80% />

----

#### 第428 个域名 [qm.com](http://qm.com/) 

确信归王新所有..


----


#### 第429 个域名 [qn.com](http://qn.com/) 

微信图片..

----

#### 第436 个域名 [qu.com](http://qu.com/)

`此域名有可能可以出售!`

英文翻译得蜜汁传神,托儿班水平

---

#### 第439 个域名 [qx.com](http://qx.com/)

齐心文具的官网...

一直觉得和"晨光""真彩"甚至"得力"相比,这个品牌一直算二线,然鹅...从域名看绝对的头部企业...

而反观市值近400亿的晨光,求mg.com不得,用的域名是`mg-pen.com` 和 `mg.cn`

<img src="两位的-com域名-有多少在中国人手中/9.png" width = 80% height = 80% />

---

#### 第456 个域名 [ro.com](https://ro.com/) 

上海的又又又一家游戏公司


<img src="两位的-com域名-有多少在中国人手中/extra22.png" width = 80% height = 80% />

----

#### 第457 个域名 [rp.com](http://rp.com/)

`您正在访问的域名可以转让！`

---


#### 第467 个域名 [rz.com](http://rz.com/)

那张微信图片++



----

#### 第475 个域名 [sh.com](https://www.sh.com/)

不评论,需谨慎

<img src="两位的-com域名-有多少在中国人手中/10.png" width = 80% height = 80% />

---

#### 第491 个域名 [sx.com](http://sx.com/) 

这就属于那种随便展示点东西,表明我并不是投资而在使用..

<img src="两位的-com域名-有多少在中国人手中/11.png" width = 80% height = 80% />

---


#### 第495 个域名 [tb.com](http://tb.com/)

微信图片++

参考fb.com,vk.com,这个域名阿里不应该买下来吗...


<img src="两位的-com域名-有多少在中国人手中/extra6.png" width = 80% height = 80% />

----

#### ~~!第498 个域名~~ [te.com](https://te.com)

泰科,是世界上最大的无源电子组件制造商之一.

市值近300亿美刀

跨国公司,此域名非中国人所有

<img src="两位的-com域名-有多少在中国人手中/extra33.png" width = 80% height = 80% />

---

#### 第512 个域名 [ts.com](http://ts.com/)

微信图片++


----

#### 第519 个域名 [tz.com](http://tz.com/)

<img src="两位的-com域名-有多少在中国人手中/extra11.png" width = 80% height = 80% />


----

#### 第521 个域名 [ub.com](http://ub.com/) 

UnitedBitcoin

Another One...

<img src="两位的-com域名-有多少在中国人手中/12.png" width = 80% height = 80% />

---

#### 第564 个域名 [vs.com](http://www.vs.com/)


成都的一家互联网金融服务公司,侧重期货

<img src="两位的-com域名-有多少在中国人手中/13.png" width = 80% height = 80% />

---

#### 第569 个域名 [vx.com](http://www.vx.com/) 

也属于那种随便展示点东西,表明我并不是投资而在使用..

<img src="两位的-com域名-有多少在中国人手中/14.png" width = 80% height = 80% />


----

#### 第581 个域名 [wj.com](http://wj.com/)

疑似又属王新所有


-----

#### 第598 个域名 [xa.com](https://www.xa.com/)

广州的一家做农业无人机的公司..但为啥在最下面的友链中有一个"山东生活网"?空让我一阵瞎激动

<img src="两位的-com域名-有多少在中国人手中/extra19.png" width = 80% height = 80% />

----

#### 第601 个域名 [xd.com](http://www.vx.com/)

上海的一家游戏公司

<img src="两位的-com域名-有多少在中国人手中/extra3.png" width = 80% height = 80% />

---

#### 第603 个域名 [xf.com](https://www.xf.com/)

苏州的一家游戏公司

<img src="两位的-com域名-有多少在中国人手中/extra34.png" width = 80% height = 80% />


----

#### 第605 个域名 [xh.com](http://xh.com/)


<img src="两位的-com域名-有多少在中国人手中/extra10.png" width = 80% height = 80% />

----

#### 第608 个域名 [xk.com](http://xk.com/)

一口价出售中！

域名Domain Name:xk.com

售价Listing Price:CNY 17777777.00

----

#### 第611 个域名 [xn.com](http://xn.com/) 

跳到某博彩网站..

----


#### 第614 个域名 [xq.com](https://xq.com/) 

个人网站,属于之前所说"随便做点东西,表明我在使用"~

<img src="两位的-com域名-有多少在中国人手中/extra24.png" width = 80% height = 80% />

----


#### 第617 个域名 [xt.com](https://www.xt.com/)


数字货币,agggggain..

<img src="两位的-com域名-有多少在中国人手中/extra37.png" width = 80% height = 80% />

----


#### 第618 个域名 [xu.com](http://www.xu.com/)

福建某电商公司

<img src="两位的-com域名-有多少在中国人手中/15.png" width = 80% height = 80% />


----

#### 第620 个域名 [xw.com](https://www.xw.com/)

数字货币+3


<img src="两位的-com域名-有多少在中国人手中/16.png" width = 80% height = 80% />

----

#### 第622 个域名 [xy.com](https://www.xy.com/)

上海的又一家游戏公司..

<img src="两位的-com域名-有多少在中国人手中/extra4.png" width = 80% height = 80% />


----

#### 第625 个域名 [yb.com](http://yb.com/)

微信图片++



----

#### 第632 个域名 [yi.com](http://yi.com/)

不评论

<img src="两位的-com域名-有多少在中国人手中/17.png" width = 80% height = 80% />

----
#### 第636 个域名 [ym.com](https://www.ym.com/)

然鹅,还是不如`4.cn`知名度更大

<img src="两位的-com域名-有多少在中国人手中/extra5.png" width = 80% height = 80% />


----

#### 第640 个域名 [yq.com](http://yq.com/) 

`此域名有可能可以出售!`

----


#### 第646 个域名 [yw.com](https://www.yw.com/)

与之前的xq.com系出一人..

<img src="两位的-com域名-有多少在中国人手中/extra27.png" width = 80% height = 80% />

----


#### 第649 个域名 [yz.com](http://yz.com/)

微信图片


----


#### ~~!第655 个域名~~ [zf.com](https://zf.com/)

采埃孚(ZF Friedrichshafen AG),是德国的一家汽车零部件生产商

此域名非中国人持有

<img src="两位的-com域名-有多少在中国人手中/extra36.png" width = 80% height = 80% />


---

#### 第656 个域名 [zg.com](https://www.zg.com/)

币圈+++

<img src="两位的-com域名-有多少在中国人手中/extra32.png" width = 80% height = 80% />


----


#### 第658 个域名 [zi.com](https://zi.com/)

字.com,

这个我喜欢,多给几个展示位~

<img src="两位的-com域名-有多少在中国人手中/18.png" width = 80% height = 80% />

<img src="两位的-com域名-有多少在中国人手中/19.png" width = 80% height = 80% />

<img src="两位的-com域名-有多少在中国人手中/20.png" width = 80% height = 80% />

----


#### 第659 个域名 [zj.com](http://zj.com/) 

又一个归属杭州的两位.com

<img src="两位的-com域名-有多少在中国人手中/extra20.png" width = 80% height = 80% />

----

#### 第660 个域名 [zk.com](http://zk.com/)


<img src="两位的-com域名-有多少在中国人手中/extra12.png" width = 80% height = 80% />


---

#### 第661 个域名 [zl.com](http://zl.com/)
`这个网站可出售。`

----

#### 第666 个域名 [zq.com](http://zq.com/) 

大概也是王新所有

<img src="两位的-com域名-有多少在中国人手中/21.png" width = 80% height = 80% />

----

#### 第667 个域名 [zr.com](http://zr.com/)

`您正在访问的域名可以转让！This domain is for sale.`


----

#### 第669 个域名 [zt.com](https://zt.com/)

数字货币+++

大概是中通快递梦寐以求的域名...

<img src="两位的-com域名-有多少在中国人手中/extra40.png" width = 80% height = 80% />


----

#### 第674 个域名 [zy.com](http://zy.com/)



<img src="两位的-com域名-有多少在中国人手中/22.png" width = 80% height = 80% />


------

**接下来是两位纯数字.com域名:**

#### [17.com](https://www.17.com/) 

17汽车网..

<img src="两位的-com域名-有多少在中国人手中/extra47.png" width = 80% height = 80% />


----

#### [20.com](http://20.com/) 

<img src="两位的-com域名-有多少在中国人手中/extra45.png" width = 80% height = 80% />


----

#### [23.com](http://23.com/) 

<img src="两位的-com域名-有多少在中国人手中/23.png" width = 80% height = 80% />

----

#### [28.com](http://www.28.com/) 

28商机网..

<img src="两位的-com域名-有多少在中国人手中/extra46.png" width = 80% height = 80% />

----



#### [32.com](http://32.com/)

还是刚才这家

<img src="两位的-com域名-有多少在中国人手中/24.png" width = 80% height = 80% />

---

 #### [35.com](https://www.35.com/) 

三五互联

<img src="两位的-com域名-有多少在中国人手中/extra44.png" width = 80% height = 80% />

----

 #### [37.com](https://www.37.com/) 


 三七互娱,算是上榜的上海一票游戏公司中最知名的了

<img src="两位的-com域名-有多少在中国人手中/extra13.png" width = 80% height = 80% />


----

#### [49.com](http://49.com/) 

`您正在访问的域名可以转让！This domain is for sale.`

----


#### [54.com](http://www.54.com/)

重庆的一家游戏公司

<img src="两位的-com域名-有多少在中国人手中/25.png" width = 80% height = 80% />

----




#### [56.com](http://www.56.com/)

56视频,像是人人网时代的产物,还有什么爆米花网,基本都落寞了..

<img src="两位的-com域名-有多少在中国人手中/extra49.png" width = 80% height = 80% />

----



#### [61.com](http://www.61.com/)

淘米网,记得是腾讯一离职员工办的,曾多次路过,在漕河泾那两座腾讯大楼旁,和爱奇艺的分公司紧挨着~

专注于儿童,这个域名也较贴切...但为啥都是游戏!

<img src="两位的-com域名-有多少在中国人手中/26.png" width = 80% height = 80% />

----


#### [62.com](https://62.com/)

<img src="两位的-com域名-有多少在中国人手中/27.png" width = 80% height = 80% />

----

#### [63.com](http://63.com/) 


以后玩私服传奇时不用搜索了,直接记住这个域名...

<img src="两位的-com域名-有多少在中国人手中/28.png" width = 80% height = 80% />


想不到,61/62/63.com全在中国人手中

----



#### [65.com](http://www.65.com/)

广州的一家游戏公司

<img src="两位的-com域名-有多少在中国人手中/29.png" width = 80% height = 80% />

----

#### [67.com](http://67.com/)


<img src="两位的-com域名-有多少在中国人手中/30.png" width = 80% height = 80% />

----

#### [69.com](http://www.69.com/)

`此域名有可能可以出售!`

60-69之间的10个域名,6个归中国人,对`6`的迷恋,可见一斑...


----

#### [79.com](http://www.79.com/)

<img src="两位的-com域名-有多少在中国人手中/31.png" width = 80% height = 80% />

----

#### [86.com](http://www.86.com/)

跳转到`http://www.0001.com/`

展示`此域名出售中！This Domain Is For Sale!`


----

#### [88.com](http://www.88.com/)


`域名联系QQ: 3254337092点击这里给我发消息
微信:caifu9928 Email: 3254337092@qq.com`


----


#### [90.com](https://www.90.com/) 

跳转到某博彩网站...

----

#### [91.com](https://www.91.com/) 

百度历史上最大规模收购,19亿美元买下91..而且除却该域名,其他都雨打风吹去了..

<img src="两位的-com域名-有多少在中国人手中/32.png" width = 80% height = 80% />

----


#### [95.com](http://www.95.com/) 

跳转到某网址导航

----

#### [97.com](http://97.com/) 


跳转/可以出售

----


#### [98.com](https://98.com/) 

`98.com`

`
数字9，谐音“就”和“久”，数字8，谐音“发”
2018年11月份售价：198,000,000

2018年12月份售价：178,000,000

2019年 1月份售价：158,000,000...`

----

#### [99.com](https://www.99.com/) 

卖掉91的网龙,还有99.com

<img src="两位的-com域名-有多少在中国人手中/extra48.png" width = 80% height = 80% />

----


毕业前几年的薪资,基本都投在这上面了..也算是一段回忆
<br>
<br>
<img src="两位的-com域名-有多少在中国人手中/b.png" width = 80% height = 80% />

<br>
<br>

<img src="两位的-com域名-有多少在中国人手中/d.png" width = 80% height = 80% />

<br>
<br>

<img src="两位的-com域名-有多少在中国人手中/e.png" width = 80% height = 80% />

<br>
<br>

<img src="两位的-com域名-有多少在中国人手中/f.png" width = 80% height = 80% />

<br>
<br>

<img src="两位的-com域名-有多少在中国人手中/g.png" width = 80% height = 80% />

<br>
<br>

<img src="两位的-com域名-有多少在中国人手中/h.png" width = 80% height = 80% />

<br>
<br>

<img src="两位的-com域名-有多少在中国人手中/j.png" width = 80% height = 80% />

<br>
<br>


还有几点值得一记:

pc.com归属intel;

ti.com归属德州仪器;

gi.com归属摩托罗拉;

ue.com为Ultimate Ears(罗技旗下高端定制监听入耳式耳机、无线蓝牙音箱品牌， 品牌总部位于美国加州Irvine和Newark市)所有;

ko.com为可口可乐所有;

fs.com为一家做服务器IDC设备的跨国公司所有,中文名"飞速",feisu.com亦归其所有;

hm.com为HM所有;

ua.com为Under Armour所有;

ck.com为Calvin Klein所有;


hw.com为哈佛西湖中学所有,该校不在波士顿,而在西海岸的加州洛杉矶;

bd.com为一家全球化的医疗技术公司所有,其在中国名称为碧迪医疗器械(上海)有限公司所有,如上外资企业在华,大多总部放在上海,但在互联网时代,这些"传统企业"大多不算知名;






