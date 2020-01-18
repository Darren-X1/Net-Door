---
layout: post
title: Surge.conf规则分享
description: 
category: blog
---
# Surge规则分享-以DlerCloud+自建节点为例
### 基础玩法
* `😄token😄`替换成DlerCloud官网上你账户下的`token`
* `😭token😭`替换成你创建的**Secret**`gist`中raw链接前一个`token`
### 进阶玩法
* ` 🇭🇰Dler-IPLC,🇭🇰Dler-BGP,🇯🇵Dler-Relay,🇺🇳Dler-Relay,🇺🇳Dler-Bronze,👤Personal`替换成自己喜欢的名字，并把`policy-path=url`中**url**替换成你机场的surge list订阅链接或通过API转后的list链接
* 注意：`♺PROXY = select,`后的选项也要对应更改，否则会报错
```
[General]
loglevel = notify
skip-proxy = 192.168.0.0/16, 193.168.0.0/24, 10.0.0.0/8, 172.16.0.0/12, 100.64.0.0/10, 17.0.0.0/8, 127.0.0.1, localhost, *.local
bypass-system = true
internet-test-url = http://www.qualcomm.cn/generate_204
proxy-test-url = http://www.qualcomm.cn/generate_204
test-timeout = 3
dns-server = system, 119.29.29.29, 208.67.222.222, 1.2.4.8, 182.254.116.116,
allow-wifi-access = true
ipv6 = false
external-controller-access = 112233@0.0.0.0:6170
http-listen = 0.0.0.0:5881
socks5-listen = 0.0.0.0:5882
wifi-access-http-port = 5883
wifi-access-socks5-port = 5884
use-default-policy-if-wifi-not-primary = true
shared-jsvm-context = true
show-error-page-for-reject = true
[Proxy]
# ♺♳♴♵♶♷♸♹

[Proxy Group]
♺PROXY = select, 🇭🇰Dler-IPLC,🇭🇰Dler-BGP,🇯🇵Dler-Relay,🇺🇳Dler-Relay,🇺🇳Dler-Bronze,👤Personal
🇭🇰Dler-IPLC = url-test, policy-path=https://dler.cloud/subscribe/😄token😄?protocols=ss&list=surge&class=gold&area=hk, update-interval=0, url=http://www.qualcomm.cn/generate_204, interval = 600, tolerance = 50
🇭🇰Dler-BGP = url-test, policy-path=https://dler.cloud/subscribe/😄token😄?protocols=ss&list=surge&class=silver&area=hk, update-interval=0, url=http://www.qualcomm.cn/generate_204, interval = 600, tolerance = 50
🇯🇵Dler-Relay = url-test, policy-path=https://dler.cloud/subscribe/😄token😄?protocols=ss&list=surge&type=relay&area=jp, update-interval=0, url=http://www.qualcomm.cn/generate_204, interval = 600, tolerance = 50
🇺🇳Dler-Relay = select, policy-path=https://dler.cloud/subscribe/😄token😄?protocols=ss&list=surge&type=relay&noarea=hk+jp&noisp=back, update-interval=0
🇺🇳Dler-Bronze = select, policy-path=https://dler.cloud/subscribe/😄token😄?protocols=ss&list=surge&class=bronze, update-interval=0
🇨🇳Dler-Back = select, policy-path=https://dler.cloud/subscribe/😄token😄?protocols=ss&list=surge&isp=back, update-interval=0
# 🏳️Dler-Vmess = select, policy-path=https://dler.cloud/subscribe/😄token😄?protocols=vmess&list=surge&class=gold, update-interval=0
👤Personal = fallback, policy-path=https://gist.githubusercontent.com/Darren-X1/😭token😭/raw/Personal, update-interval=0, url=http://www.qualcomm.cn/generate_204
♳Final = select,♺PROXY,DIRECT
♴GlobalTV = select,DIRECT,♺PROXY
♵AsianTV = select,DIRECT,♴GlobalTV,♺PROXY
♶AdBlock = select,REJECT-TINYGIF,REJECT,DIRECT
♷Domestic = select,DIRECT,♺PROXY
♸Speedtest = select,♺PROXY,DIRECT
♹Telegram = select,♺PROXY,DIRECT
Apple = select,DIRECT,♺PROXY
ⓌWiFi = ssid, default = ♳Final, cellular = ♳Final

[Rule]
# Rulesets
RULE-SET,SYSTEM,Apple
RULE-SET,https://raw.githubusercontent.com/lhie1/Rules/master/Surge/Surge%203/Provider/Apple.list,Apple
RULE-SET,https://raw.githubusercontent.com/lhie1/Rules/master/Surge/Surge%203/Provider/Reject.list,♶AdBlock
RULE-SET,https://raw.githubusercontent.com/lhie1/Rules/master/Surge/Surge%203/Provider/AsianTV.list,♵AsianTV
RULE-SET,https://raw.githubusercontent.com/lhie1/Rules/master/Surge/Surge%203/Provider/GlobalTV.list,♴GlobalTV
RULE-SET,https://raw.githubusercontent.com/lhie1/Rules/master/Surge/Surge%203/Provider/Telegram.list,♹Telegram
RULE-SET,https://raw.githubusercontent.com/lhie1/Rules/master/Surge/Surge%203/Provider/Speedtest.list,♸Speedtest
RULE-SET,https://raw.githubusercontent.com/lhie1/Rules/master/Surge/Surge%203/Provider/Netease%20Music.list,DIRECT
RULE-SET,https://raw.githubusercontent.com/lhie1/Rules/master/Surge/Surge%203/Provider/Proxy.list,♺PROXY
RULE-SET,https://raw.githubusercontent.com/lhie1/Rules/master/Surge/Surge%203/Provider/Domestic.list,♷Domestic
RULE-SET,LAN,DIRECT
DOMAIN,dler.cloud,DIRECT
DOMAIN-KEYWORD,announce,DIRECT
DOMAIN-KEYWORD,torrent,DIRECT
DOMAIN-KEYWORD,tracker,DIRECT
DOMAIN-SUFFIX,smtp,DIRECT
URL-REGEX,(Subject|HELO|SMTP),DIRECT
URL-REGEX,(api|ps|sv|offnavi|newvector|ulog.imap|newloc)(.map|).(baidu|n.shifen).com,DIRECT
URL-REGEX,(.+.|^)(360|so|qihoo|360safe|qhimg|360totalsecurity|yunpan).(cn|com),DIRECT
URL-REGEX,(.+.)?(torrent|announce.php?passkey=|tracker|BitTorrent|bt_key|ed2k|find_node|get_peers|info_hash|magnet:|peer_id=|sandai|Thunder|XLLiveUD|xunlei)(..+)?,DIRECT
IP-CIDR,1.1.1.1/24,DIRECT,no-resolve
GEOIP,CN,DIRECT
FINAL,♳Final,dns-failed

[Snell Server]
interface = 0.0.0.0
port = 5886
psk = bwh123456
obfs = off

[SSID Setting]
"X-iPhone" cellular-mode=true

[URL Rewrite]
# TikTok (By Choler & Paris ^_^)
(?<=(carrier|account|sys)_region=)CN JP 307
(?<=(carrier|account|sys|sim)_region=)cn in 307

# > Google_Service_HTTPS_Jump
^https?://(www.)?g.cn https://www.google.com 302
^https?://(www.)?google.cn https://www.google.com 302

# > Anti_ISP_JD_Hijack
^https?://coupon.m.jd.com/ https://coupon.m.jd.com/ 302
^https?://h5.m.jd.com/ https://h5.m.jd.com/ 302
^https?://item.m.jd.com/ https://item.m.jd.com/ 302
^https?://m.jd.com/ https://m.jd.com/ 302
^https?://newcz.m.jd.com/ https://newcz.m.jd.com/ 302
^https?://p.m.jd.com/ https://p.m.jd.com/ 302
^https?://so.m.jd.com/ https://so.m.jd.com/ 302
^https?://union.click.jd.com/jda? http://union.click.jd.com/jda?adblock= header
^https?://union.click.jd.com/sem.php? http://union.click.jd.com/sem.php?adblock= header
^https?://www.jd.com/ https://www.jd.com/ 302

# > Anti_ISP_Taobao_Hijack
^https?://m.taobao.com/ https://m.taobao.com/ 302

# > Wiki
^https?://.+.(m.)?wikipedia.org/wiki http://www.wikiwand.com/en 302
^https?://zh.(m.)?wikipedia.org/(zh-hans|zh-sg|zh-cn|zh(?=/)) http://www.wikiwand.com/zh 302
^https?://zh.(m.)?wikipedia.org/zh-[a-zA-Z]{2,} http://www.wikiwand.com/zh-hant 302

# > NOMO
^https://nomo.dafork.com/api/v3/iap/ios_product_list https://files.catbox.moe/fgmkpy.json 302

# > Other
^https?://cfg.m.ttkvod.com/mobile/ttk_mobile_1.8.txt http://ogtre5vp0.bkt.clouddn.com/Static/TXT/ttk_mobile_1.8.txt header
^https?://cnzz.com/ http://ogtre5vp0.bkt.clouddn.com/background.png? header
^https?://m.qu.la/stylewap/js/wap.js http://ogtre5vp0.bkt.clouddn.com/qu_la_wap.js 302
^https?://m.yhd.com/1/\? http://m.yhd.com/1/?adbock= 302
^https?://n.mark.letv.com/m3u8api/ http://burpsuite.applinzi.com/Interface header
^https?://sqimg.qq.com/ http://sqimg.qq.com/ 302
^https?://static.m.ttkvod.com/static_cahce/index/index.txt http://ogtre5vp0.bkt.clouddn.com/Static/TXT/index.txt header
^https?://www.iqshw.com/d/js/m http://burpsuite.applinzi.com/Interface header
^https?://www.iqshw.com/d/js/m http://rewrite.websocket.site:10/Other/Static/JS/Package.js? header

# > TikTok Internation
# (.*video_id=\w{32})(.*watermark=)(.*) url 302 $1
# (?<=(carrier|sys)_region=)CN url 307 JP
# (?<=version_code=)\d{1,}.\d{1}\.\d{1} url 307 8.0.0

# > Anti_ISP_JavaScript_Injection
^https?://c.minisplat.cn - reject
^https?://c1.minisplat.cn - reject
^https?://cache.changjingyi.cn - reject
^https?://cache.gclick.cn - reject

# > Anti_ISP_Safari_Baidu_CPM_Hijack
^https?://m.coolaiy.com/b.php - reject
^https?://www.babyye.com/b.php - reject
^https?://www.gwv7.com/b.php - reject
^https?://www.likeji.net/b.php - reject

# > ChinaRailcom
^https?://211.98.70.226:8080/ - reject
^https?://211.98.71.195:8080/ - reject
^https?://211.98.71.196:8080/ - reject

# > 腾讯
^https?://.+.mp4\?cdncode=.+&sdtfrom=v3004 - reject
^https?://.+/hls.cache.p4p/ - reject
^https?://.+/music/common/upload/t_splash_info - reject
^https?://.+/omts.tc.qq.com/ - reject
^https?://.+/tips/fcgi-bin/fcg_get_advert - reject
^https?://.+/variety.tc.qq.com/ - reject
^https?://.+/vlive.qqvideo.tc.qq.com/ - reject
^https?://3gimg.qq.com/tencentMapTouch/app/activity/ - reject
^https?://api5.futunn.com/ad/ - reject
^https?://bla.gtimg.com/qqlive/\d{6}.+.png - reject
^https?://imgcache.qq.com/qqlive/ - reject
^https?://lives.l.qq.com/livemsg\?sdtfrom= - reject
^https?://mmgr.gtimg.com/gjsmall/qiantu/upload/ - reject
^https?://mp.weixin.qq.com/mp/(ad_complaint|ad_video|advertisement_report|report) - reject
^https?://mtteve.beacon.qq.com/analytics - reject
^https?://qt.qq.com/lua/mengyou/get_splash_screen_info - reject
^https?://r.inews.qq.com/(adsBlacklist|getBannerAds|getFullScreenPic|getNewsRemoteConfig|getQQNewsRemoteConfig|searchHotCatList|upLoadLoc) - reject
^https?://r.inews.qq.com/getSplash\?apptype=ios&startarticleid=&__qnr= - reject
^https?://splashqqlive.gtimg.com/website/\d{6} - reject
^https?://ssl.kohsocialapp.qq.com:10001/game/buttons - reject
^https?://szextshort.weixin.qq.com/cgi-bin/mmoc-bin/ad/ - reject
^https?://vv.video.qq.com/getvmind\? - reject
^https?://y.gtimg.cn/music/common/upload/targeted_ads - reject

# > 新浪
^https?://api.weibo.cn/2/statuses/extend\?gsid= - reject
^https?://edit.sinaapp.com/ua\?t=adv - reject
^https?://free.sinaimg.cn/u1.img.mobile.sina.cn - reject
^https?://simg.s.weibo.com/.+_ios\d{2}.gif - reject
^https?://storage.wax.weibo.com/\w+.(png|jpg|mp4) - reject
^https?://u1.img.mobile.sina.cn/public/files/image/\d{3}x\d{2,4}.+(png|jpg|mp4) - reject

# > 优酷
.+&duration=\d{2}& - reject
^https?://(iyes|(api|hd).mobile).youku.com/(adv|common/v3/hudong/new) - reject
^https?://.+.mp4\?ccode=0902 - reject
^https?://.+.mp4\?sid= - reject
^https?://ad.api.3g.youku.com - reject
^https?://api.appsdk.soku.com/(bg|tag)/r - reject
^https?://api.k.sohu.com/api/channel/ad/ - reject
^https?://api.mobile.youku.com/layout/search/hot/word - reject
^https?://m.youku.com/video/libs/iwt.js - reject
^https?://pic.k.sohu.com/img8/wb/tj/ - reject
^https?://r.l.youku.com/rec_at_click - reject
^https?://r1.ykimg.com/\w{30,35}.jpg - reject
^https?://r1.ykimg.com/material/.+/\d{3,4}-\d{4} - reject
^https?://r1.ykimg.com/material/.+/\d{6}/\d{4}/ - reject
^https?://ups.youku.com/(.*)needad=1& ^https?://ups.youku.com/$1needad=0& 302
^https?://vali.cp31.ott.cibntv.net/youku - reject

# > 网易
^https?://.+.127.net/ad - reject
^https?://.+/eapi/(ad|evenet|log)/ - reject
^https?://c.m.163.com/nc/gl/ - reject
^https?://client.mail.163.com/apptrack/confinfo/searchMultiAds - reject
^https?://dsp-impr2.youdao.com/adload.s\? - reject
^https?://g1.163.com/madfeedback - reject
^https?://img1.126.net/.+dpi=\w{7,8} - reject
^https?://img1.126.net/channel14/ - reject
^https?://interface.music.163.com/eapi/ad/ - reject
^https?://mimg.127.net/external/smartpop-manger.min.js - reject
^https?://nex.163.com/q - reject
^https?://oimage([a-z])([0-9]).ydstatic.com/.+?&product=adpublish - reject
^https?://p[4](c)?.music.126.net/\w+==/10995\d{13}.jpg$ - reject
^https?://sp.kaola.com/api/openad - reject
^https?://support.you.163.com/xhr/boot/getBootMedia.json - reject

# > 知乎
^https?://api.zhihu.com/ab/api - reject
^https?://api.zhihu.com/ad-style-service - reject
^https?://api.zhihu.com/app_config - reject
^https?://api.zhihu.com/appview/api/v4/answers.+recommendations - reject
^https?://api.zhihu.com/banner - reject
^https?://api.zhihu.com/launch - reject
^https?://api.zhihu.com/market/popover - reject
^https?://api.zhihu.com/real_time - reject
^https?://api.zhihu.com/search/preset_words - reject
^https?://api.zhihu.com/search/top_search - reject
^https?://api.zhihu.com/zst/events - reject
^https?://www.zhihu.com/api/v4/community-ad/ - reject
^https?://www.zhihu.com/terms/privacy/confirm - reject

# > 追书神器
^https?://(api|b).zhuishushenqi.com/advert - reject
^https?://api.zhuishushenqi.com/notification/shelfMessage - reject
^https?://api.zhuishushenqi.com/recommend - reject
^https?://api.zhuishushenqi.com/splashes/ios - reject
^https?://api01pbmp.zhuishushenqi.com/gameAdvert - reject
^https?://mi.gdt.qq.com/gdt_mview.fcg - reject
# > Upgrade
^https?://api.zhuishushenqi.com/user/bookshelf-updated - reject
^https?://itunes.apple.com/lookup\?id=575826903 - reject

# > 爱奇艺
^https?://.+/cdn/qiyiapp/\d{8}/.+&dis_dz= - reject
^https?://.+/cdn/qiyiapp/\d{8}/.+&z=\w - reject
^https?://.+/videos/other/ - reject
^https?://iface2.iqiyi.com/fusion/3.0/fusion_switch - reject

# > 搜狐
^https?://agn.aty.sohu.com/m? - reject
^https?://api.k.sohu.com/api/news/adsense - reject
^https?://hui.sohu.com/predownload2/? - reject
^https?://m.aty.sohu.com/openload? - reject
^https?://mbl.56.com/config/v1/common/config.union.ios.do? - reject
^https?://mmg.aty.sohu.com/mqs? - reject
^https?://mmg.aty.sohu.com/pvlog? - reject
^https?://photocdn.sohu.com/tvmobilemvms - reject
^https?://pic.k.sohu.com/img8/wb/tj/ - reject
^https?://s.go.sohu.com/adgtr/\?gbcode= - reject

# > 百度
(ps|sv|offnavi|newvector|ulog.imap|newloc)(.map)?.(baidu|n.shifen).com - reject
^https?://afd.baidu.com/afd/entry - reject
^https?://als.baidu.com/clog/clog - reject
^https?://baichuan.baidu.com/rs/adpmobile/launch - reject
^https?://bj.bcebos.com/fc-feed/0/pic/ - reject
^https?://c.tieba.baidu.com/\w+/\w+/(sync|newRnSync|newlog|mlog) - reject
^https?://c.tieba.baidu.com/c/p/img\?src= - reject
^https?://c.tieba.baidu.com/c/s/logtogether\?cmd= - reject
^https?://fcvbjbcebos.baidu.com/.+.mp4 - reject
^https?://gss0.bdstatic.com/.+/static/wiseindex/img/bd_red_packet.png - reject
^https?://issuecdn.baidupcs.com/issue/netdisk/guanggao/ - reject
^https?://sm.domobcdn.com/ugc/\w/ - reject
^https?://tb1.bdstatic.com/tb/cms/ngmis/adsense/*.jpg - reject
^https?://tb2.bdstatic.com/tb/mobile/spb/widget/jump - reject
^https?://update.pan.baidu.com/statistics - reject
^https?://wapwenku.baidu.com/view/fengchao/ - reject
^https?://wapwenku.baidu.com/view/fengchaoTwojump/ - reject
^https?://wenku.baidu.com/shifen/ - reject

# > 百度地图
^https?://.+/client/phpui2/ - reject

# > 百度贴吧
^https?://c.tieba.baidu.com/c/s/splashSchedule - reject
^https?://cover.baidu.com/cover/page/dspSwitchAds/ - reject

# > 墨迹天气
^https?://ad.api.moji.com/ad/log/stat - reject
^https?://ast.api.moji.com/assist/ad/moji/stat - reject
^https?://cdn.moji.com/adlink/avatarcard - reject
^https?://cdn.moji.com/adlink/common - reject
^https?://cdn.moji.com/adlink/splash/ - reject
^https?://cdn.moji.com/advert/ - reject
^https?://cdn2.moji002.com/webpush/ad2/ - reject
^https?://fds.api.moji.com/card/recommend - reject
^https?://show.api.moji.com/json/showcase/getAll - reject
^https?://stat.moji.com - reject
^https?://storage.360buyimg.com/kepler-app - reject
^https?://ugc.moji001.com/sns/json/profile/get_unread - reject

# > 小米
^https?://api.m.mi.com/v1/app/start - reject
^https?://api.jr.mi.com/v1/adv/ - reject

# > 中国电信
^https?://image1.chinatelecom-ec.com/images/.+/\d{13}.jpg - reject

# > 中国联通
^https?://m.client.10010.com/mobileService/(activity|customer)/(accountListData|get_client_adv|get_startadv) - reject
^https?://m.client.10010.com/uniAdmsInterface/(getHomePageAd|getWelcomeAd) - reject
^https?://m1.ad.10010.com/noticeMag/images/imageUpload/2\d{3} - reject
^https?://res.mall.10010.cn/mall/common/js/fa.js?referer= - reject

# > 凤凰网
^https?://api.newad.ifeng.com/ClientAdversApi1508\?adids= - reject
^https?://c1.ifengimg.com/.+_w1080_h1410.jpg - reject
^https?://exp.3g.ifeng.com/coverAdversApi\?gv=. - reject
^https?://ifengad.3g.ifeng.com/ad/pv.php\?stat= - reject
^https?://iis1.deliver.ifeng.com/getmcode\?adid= - reject

# > 京东
^https?://.+/client?functionId=lauch/lauchConfig&appName=paidaojia - reject
^https?://111.13.29.201/client.action\?functionId=start - reject
^https?://api.m.jd.com/client.action\?functionId=start - reject
^https?://bdsp-x.jd.com/adx/ - reject
^https?://m.360buyimg.com/mobilecms/s640x1136_jfs/ - reject
^https?://ms.jr.jd.com/gw/generic/base/na/m/adInfo - reject
^https?://img12.360buyimg.com.+width=\d{4}&height=\d{4} - reject

# > 淘宝
^https?://gw.alicdn.com/tfs/.+-1125-1602 - reject

# > 豆瓣
^https?://(\d{1,3}.){1,3}\d{1,3}/view/dale-online/dale_ad/ - reject
^https?://api.douban.com/v2/app_ads/common_ads - reject
^https?://frodo.douban.com/api/v2/movie/banner - reject
^https?://img\d.doubanio.com/view/dale-online/dale_ad/ - reject

# > 斗鱼
^https?://capi.douyucdn.cn/lapi/sign/app(api)?/getinfo\?client_sys=ios - reject
^https?://capi.douyucdn.cn/api/ios_app/check_update - reject
^https?://capi.douyucdn.cn/api/v1/getStartSend?client_sys=ios - reject
^https?://douyucdn.cn/.+/appapi/getinfo - reject
^https?://rtbapi.douyucdn.cn/japi/sign/app/getinfo - reject
^https?://staticlive.douyucdn.cn/.+/getStartSend - reject
^https?://staticlive.douyucdn.cn/upload/signs/ - reject

# > 饿了么
^https?://elemecdn.com/.+/sitemap - reject
^https?://fuss10.elemecdn.com/.+/w/640/h/\d{3,4} - reject
^https?://fuss10.elemecdn.com/.+/w/750/h/\d{3,4} - reject
^https?://fuss10.elemecdn.com/.+.mp4 - reject
^https?://m.elecfans.com/static/js/ad.js - reject
^https?://www1.elecfans.com/www/delivery/ - reject

# > 头条
^https?://p\d.pstatp.com/origin - reject
^https?://pb\d.pstatp.com/origin - reject

# > 咸鱼
^https?://gw.alicdn.com/mt/ - reject
^https?://gw.alicdn.com/tfs/.+\d{3,4}-\d{4} - reject
^https?://gw.alicdn.com/tps/.+\d{3,4}-\d{4} - reject

# > 喜马拉雅
^https?://adse.+.com/[a-z]{4}/loading\?appid= - reject
^https?://adse.ximalaya.com/ting/feed\?appid= - reject
^https?://adse.ximalaya.com/ting/loading\?appid= - reject
^https?://adse.ximalaya.com/ting\?appid= - reject
^https?://fdfs.xmcdn.com/group21/M03/E7/3F/ - reject
^https?://fdfs.xmcdn.com/group21/M0A/95/3B/ - reject
^https?://fdfs.xmcdn.com/group22/M00/92/FF/ - reject
^https?://fdfs.xmcdn.com/group22/M05/66/67/ - reject
^https?://fdfs.xmcdn.com/group22/M07/76/54/ - reject
^https?://fdfs.xmcdn.com/group23/M01/63/F1/ - reject
^https?://fdfs.xmcdn.com/group23/M04/E5/F6/ - reject
^https?://fdfs.xmcdn.com/group23/M07/81/F6/ - reject
^https?://fdfs.xmcdn.com/group23/M0A/75/AA/ - reject
^https?://fdfs.xmcdn.com/group24/M03/E6/09/ - reject
^https?://fdfs.xmcdn.com/group24/M07/C4/3D/ - reject
^https?://fdfs.xmcdn.com/group25/M05/92/D1/ - reject

# > 掌阅
^https?://book.img.ireader.com/group6/M00 - reject

# > 易车
^https?://api.ycapp.yiche.com/appnews/getadlist - reject
^https?://api.ycapp.yiche.com/yicheapp/getadlist - reject
^https?://api.ycapp.yiche.com/yicheapp/getappads/ - reject
^https?://cheyouapi.ycapp.yiche.com/appforum/getusermessagecount - reject

# > YouTube
^https?://.+.googlevideo.com/.+&oad - reject
^https?://.+.googlevideo.com/.+ctier - reject
^https?://.+.googlevideo.com/ptracking\?pltype=adhost - reject
^https?://.+.youtube.com/api/stats/.+adformat - reject
^https?://.+.youtube.com/api/stats/ads - reject
^https?://.+.youtube.com/get_midroll_ - reject
^https?://.+.youtube.com/pagead/ - reject
^https?://.+.youtube.com/ptracking\? - reject
^https?://m.youtube.com/_get_ads - reject
^https?://pagead2.googlesyndication.com/pagead/ - reject
^https?://premiumyva.appspot.com/vmclickstoadvertisersite - reject
^https?://s0.2mdn.net/ads/ - reject
^https?://stats.tubemogul.com/stats/ - reject
^https?://youtubei.googleapis.com/.+ad_break - reject
^https?://youtubei.googleapis.com/youtubei/.+(ad|log)_ - reject

# > Youtube++
^https?://api.catch.gift/api/v3/pagead/ - reject

# > 网喵
^https?://.+0013.+/upload/activity/app_flash_screen_ - reject

# > 天山直播
http?://www.tsytv.com.cn/api/app/ios/ads - reject

# > 肯德基
^https?://res.kfc.com.cn/advertisement/ - reject

# > 首约汽车
^https?://img.yun.01zhuanche.com/statics/app/advertisement/.+-750-1334 - reject

# > 神舟汽车
^https?://img01.10101111cdn.com/adpos/share/ - reject

# > 流量银行
^https?://bank.wo.cn/v9/getstartpage - reject

# > 海盐
^https?://img.ihytv.com/material/adv/img/ - reject

# > 美团
^https?://img.meituan.net/midas/ - reject
^https?://p\d.meituan.net/(mmc|wmbanner)/ - reject

# > QQ Pim
^https?://mmgr.gtimg.com/gjsmall/qqpim/public/ios/splash/.+/\d{4}_\d{4} - reject

# > 界面新闻
^https?://img.jiemian.com/ads/ - reject

# > 汽车之家
^https?://adproxy.autohome.com.cn/AdvertiseService/ - reject
^https?://app2.autoimg.cn/appdfs/ - reject

# > 起点读书
^https?://mage.if.qidian.com/Atom.axd/Api/Client/GetConfIOS - reject

# > 当当
^https?://img\d{2}.ddimg.cn/upload_img/.+/670x900 - reject
^https?://img\d{2}.ddimg.cn/upload_img/.+/750x1064 - reject
^https?://mapi.dangdang.com/index.php\?action=init&user_client=iphone - reject

# > 国泰君安证券
^https?://dl.app.gtja.com/dzswem/kvController/ - reject
^https?://dl.app.gtja.com/operation/config/startupConfig.json - reject

# > 来疯直播
^https?://api.laifeng.com/v1/start/ads - reject

# > 抖音
^https?://(.*)\.snssdk\.com/aweme/v2/ https://$1.snssdk.com/aweme/v1/ 302
^https?://.+.pstatp.com/img/ad - reject
^https?://.+.snssdk.com/api/ad/ - reject
^https?://aweme.snssdk.com/aweme/v1/aweme/stats/ - reject
^https?://aweme.snssdk.com/aweme/v1/device/update/ - reject
^https?://aweme.snssdk.com/aweme/v1/screen/ad/ - reject
^https?://aweme.snssdk.com/service/1/app_logout/ - reject
^https?://aweme.snssdk.com/service/2/app_log - reject
^https?://frontier.snssdk.com/ - reject
^https?://sf\w-ttcdn-tos.pstatp.com/obj/web.business.image - reject

# > 下厨房
^https?://api.xiachufang.com/v2/ad/ - reject

# > Facebook
^https?://connect.facebook.net/en_US/fbadnw.js - reject

# > 快递100
^https?://qzonestyle.gtimg.cn/qzone/biz/gdt/mob/sdk/ios/v2/ - reject
^https?://cdn.kuaidi100.com/images/open/appads - reject
^https?://p.kuaidi100.com/mobile/mainapi.do - reject

# > Mi
^https?://api.m.mi.com/.+/app/start - reject
^https?://api-mifit.huami.com/discovery/mi/discovery/homepage_ad\? - reject
^https?://api-mifit.huami.com/discovery/mi/discovery/sleep_ad\? - reject
^https?://api-mifit.huami.com/discovery/mi/discovery/sport_ad\? - reject
^https?://api-mifit.huami.com/discovery/mi/discovery/sport_summary_ad\? - reject
^https?://api-mifit.huami.com/discovery/mi/discovery/sport_training_ad\? - reject
^https?://api-mifit.huami.com/discovery/mi/discovery/step_detail_ad\? - reject
^https?://api-mifit.huami.com/discovery/mi/discovery/training_video_ad\? - reject

# > Weico
^https?://overseas.weico.cc/portal.php\?a=get_coopen_ads - reject

# > StarFans
^https?://g.cdn.pengpengla.com/starfantuan/boot-screen-info/ - reject

# > Discuz
^https?://discuz.gtimg.cn/cloud/scripts/discuz_tips.js - reject

# > 果盘游戏
^https?://sapi.guopan.cn/get_buildin_ad - reject

# > 驾考宝典
^https?://789.kakamobi.cn/.+adver - reject
^https?://smart.789.image.mucang.cn/advert - reject

# > 招商银行
^https?://pic1cdn.cmbchina.com/appinitads/ - reject

# > Cmblife
^https?://mlife.cmbchina.com/ClientFace(Service)?/getAdvertisement.json - reject
^https?://mlife.cmbchina.com/ClientFace(Service)?/preCacheAdvertise.json - reject
^https?://resource.cmbchina.com/fsp/File/ClientFacePublic/.+.gif - reject

# > ElongClient
http?://123.59.30.10/adv/advInfos - reject

# > AiRav
^https?://bbs.airav.cc/data/.+.jpg - reject
^https?://image.airav.cc/AirADPic/.+.gif - reject
^https?://m.airav.cc/images/Mobile_popout_cn.gif - reject

# > 花生地铁
^https?://cmsapi.wifi8.com/v1/emptyAd/info - reject
^https?://cmsapi.wifi8.com/v2/adNew/config - reject
^https?://cmsfile.wifi8.com/uploads/png/ - reject

# > AppSo
^https?://sso.ifanr.com/jiong/IOS/appso/splash/ - reject

# > 懒人听书
^https?://118.178.214.118/yyting/advertclient/ClientAdvertList.action - reject
^https?://dapis.mting.info/yyting/advertclient/ClientAdvertList.action - reject

# > 91Porn
^https?://192.133.+.mp4$ - reject

# > 熊猫直播
^https?://static.api.m.panda.tv/index.php\?method=clientconf.firstscreen&__version=(play_cnmb|(\d+.){0,3}\d+)&__plat=ios&__channel=appstore - reject

# > 微吼
^https?://api.app.vhall.com/v5/000/webinar/launch - reject

# > 天天狼人杀
^https?://img.53site.com/Werewolf/AD/ - reject
^https?://werewolf.53site.com/Werewolf/.+/getAdvertise.php - reject
^https?://werewolf.53site.com/Werewolf/.+/getShareVideodb.php - reject

# > Apple
^https?://a.applovin.com/.+/ad - reject

# > 微医
^https?://app.wy.guahao.com/json/white/dayquestion/getpopad - reject
^https?://kano.guahao.cn/.+\?resize=\d{3}-\d{4} - reject

# > 车来了
^https?://api.chelaile.net.cn/adpub/ - reject
^https?://api.chelaile.net.cn/goocity/advert/ - reject
^https?://atrace.chelaile.net.cn/adpub/ - reject
^https?://atrace.chelaile.net.cn/exhibit\?&adv_image - reject
^https?://pic1.chelaile.net.cn/adv/ - reject

# > 健康160
^https?://images.91160.com/primary/ - reject

# > 1钱包
^https?://d.1qianbao.com/youqian/ads/ - reject

# > 火猫直播
^https?://api.huomao.com/channels/loginAd - reject

# > 快看漫画
^https?://api.kkmh.com/v\d/(ad|advertisement)/ - reject

# > 虎扑
^https?://i1.hoopchina.com.cn/blogfile/.+_\d{3}x\d{4} - reject

# > 乐视TV
^https?://.+/letv-gug/ - reject

# > 芒果TV
^https?://pcvideoyd.titan.mgtv.com/pb/ - reject

# > Kecheng Gezi
^https?://classbox2.kechenggezi.com/api/v1/sponge/pull\?request_time= - reject

# > 当当阅读
^https?://e.dangdang.com/media/api.+\?action=getDeviceStartPage - reject

# > 什么值得买
^https?://api.smzdm.com/v1/util/loading - reject
^https?://api.smzdm.com/v2/util/banner - reject

# > 飞常准
^https?://app.veryzhun.com/ad/admob - reject

# > 凤凰秀
^https?://api.fengshows.com/api/launchAD - reject

# > 人人视频
^https?://img.rr.tv/banner/.+.jpg - reject

# > 人人影视
^https?://ctrl.(playcvn|zmzapi).net/app/(ads|init) - reject

# > 老司机
^https?://api.laosiji.com/user/startpage/ - reject

# > 同花顺 Pro
^https?://adm.10jqka.com.cn/interface/getads.php - reject

# > 杭州市民卡
^https?://smkmp.96225.com/smkcenter/ad/.+/adBanner - reject

# > 杭州公交
^https?://m.ibuscloud.com/v2/app/getStartPage - reject

# > 埋堆堆
^https?://api.mddcloud.com.cn/api/ad/getClassAd.action - reject
^https?://api.mddcloud.com.cn/api/advert/getHomepage.action - reject

# > 叨鱼
^https?://daoyu.sdo.com/api/userCommon/getAppStartAd - reject

# > Keep
^https?://api.gotokeep.com/ads - reject
^https?://static1.keepcdn.com/.+\d{3}x\d{4} - reject

# > iSafePlay
^https?://aarkissltrial.secure2.footprint.net/v1/ads - reject
^https?://rm.aarki.net/v1/ads - reject

# > 超级课程表
^https?://182.92.244.70/d/json/ - reject

# > 飞猪
^https?://acs.m.taobao.com/gw/mtop.trip.activity.querytmsresources/1.0\?type=originaljson - reject

# > Finger Driver
^https?://.+/videos/KnifeHit_4/gear3/ - reject

# > 驾图
^https?://images.kartor.cn/.+.html - reject

# > 动卡空间
^https?://m.creditcard.ecitic.com/citiccard/mbk/.+./appStartAdv - reject

# > 好奇心日报
^https?://app3.qdaily.com/app3/boot_advertisements.json - reject

# > 分期乐
^https?://fm.fenqile.com/routev2/other/getfloatAd.json - reject
^https?://fm.fenqile.com/routev2/other/startImg.json - reject

# > Vip mobile
^https?://.+/vips-mobile/router.do\?api_key= - reject

# > 顺丰蜂巢
^https?://consumer.fcbox.com/v1/ad/OpeningAdInfo/ - reject

# > 威锋
^https?://api.feng.com[\s\S]*?Claunch_screen - reject
^https?://fengplus.feng.com/index.php\?r=api/slide/.+Ads - reject

# > 咪咕
^https?://.+/img/ad.union.api/ - reject
^https?://.+/v1/iflyad/ - reject
^https?://ggic.cmvideo.cn/ad/ - reject
^https?://ggic2.cmvideo.cn/ad/ - reject
^https?://ggv.cmvideo.cn/v1/iflyad/ - reject

# > 太平洋电脑网
^https?://agent-count.pconline.com.cn/counter/adAnalyse/ - reject
^https?://ivy.pchouse.com.cn/adpuba/ - reject

# > 开源中国
^https?://www.oschina.net/action/apiv2/get_launcher - reject

# > ofo
^https?://activity2.api.ofo.com/ofo/Api/v2/ads - reject
^https?://ma.ofo.com/ads - reject
^https?://supportda.ofo.com/adaction\? - reject

# > 四季线上影视
^https?://service.4gtv.tv/4gtv/Data/ADLog - reject
^https?://service.4gtv.tv/4gtv/Data/GetAD - reject

# > 爱回收
^https?://gw.aihuishou.com/app-portal/home/getadvertisement - reject

# > 58同城
^https?://app.58.com/api/log/ - reject

# > 多看
^https?://www.duokan.com/pictures? - reject
^https?://www.duokan.com/promotion_day - reject

# > TikTok
^https?://api\d?.tiktokv.com/api/ad/ - reject
^https?://api\d?.musical.ly/api/ad/ - reject

# > 漫画人
^https?://mangaapi.manhuaren.com/v1/public/getStartPageAds - reject

# > 秒拍
^https?://b-api.ins.miaopai.com/1/ad/ - reject

# > 迅雷
^https?://images.client.vip.xunlei.com/.+/advert/ - reject

# > 天气通
^https?://tqt.weibo.cn/.+advert.index - reject
^https?://tqt.weibo.cn/overall/redirect.php\?r=tqt_sdkad - reject
^https?://tqt.weibo.cn/overall/redirect.php\?r=tqtad - reject

# > 运动世界
^https?://.+.iydsj.com/api/.+/ad - reject

# > 雅思
^https?://cdn.tiku.zhan.com/banner - reject

# > 美味不用等
^https?://capi.mwee.cn/app-api/V12/app/getstartad - reject

# > AcFun
^https?://aes.acfun.cn/s\?adzones - reject

# > 讯飞
^https?://imeclient.openspeech.cn/adservice/ - reject

# > Yahoo
^https?://m.yap.yahoo.com/v18/getAds.do - reject

# > 抱抱
^https?://www.myhug.cn/ad/ - reject

# > 麻花影视
^https?://.+/api/app/member/ver2/user/login/ - reject

# > 直播吧
^https?://a.qiumibao.com/activities/config.php - reject
^https?://.+/allOne.php\?ad_name - reject

# > 穷游
^https?://open.qyer.com/qyer/startpage/ - reject
^https?://open.qyer.com/qyer/config/get - reject
^https?://media.qyer.com/ad/ - reject

# > 肆客足球
^https?://api.qiuduoduo.cn/guideimage - reject

# > 萤石云视频
^https?://i.ys7.com/api/ads - reject

# > 电视家
^https?://api.gaoqingdianshi.com/api/v2/ad - reject

# > 虎扑
^https?://i\d.hoopchina.com.cn/blogfile//d+//d+/BbsImg.(?<=(big.(png|jpg)))$ - reject
^https?://games.mobileapi.hupu.com/.+/(search|interfaceAdMonitor|status|hupuBbsPm)/(hotkey|init|hupuBbsPm). - reject
^https?://games.mobileapi.hupu.com/interfaceAdMonitor - reject

# > 高德
^https?://m5.amap.com/ws/valueadded/ - reject

# > 虾米音乐
^https?://pic.xiami.net/images/common/uploadpic[\s\S]*?.jpg$ - reject

# > 作业帮
^https?://img.zuoyebang.cc/zyb-image[\s\S]*?.jpg - reject

# > bilibili
^https?://api.bilibili.com/pgc/season/rank/cn - reject
^https?://app.bilibili.com/x/v2/rank.*rid=168 - reject
^https?://app.bilibili.com/x/v2/rank.*rid=5 - reject
^https?://app.bilibili.com/x/v2/search/defaultword - reject
^https?://app.bilibili.com/x/v2/search/hot - reject
^https?://app.bilibili.com/x/v2/search/recommend - reject

# > 一点万象
^https?://app.mixcapp.com/mixc/api/v2/ad - reject

# > WiFi共享大师
^https?://nochange.ggsafe.com/ad/ - reject

# > 蜗牛睡眠
^https?://snailsleep.net/snail/v1/adTask/ - reject
^https?://snailsleep.net/snail/v1/screen/qn/get\? - reject

# > 小睡眠
^https?://api.psy-1.com/cosleep/startup - reject

# > Yahoo!
^https?://m.yap.yahoo.com/v18/getAds.do - reject

# > WeDoctor
^https?://app.wy.guahao.com/json/white/dayquestion/getpopad - reject

# > 无他
^https?://api-release.wuta-cam.com/ad_tree - reject
^https?://res-release.wuta-cam.com/json/ads_component_cache.json - reject

# > 向日葵
^https?://slapi.oray.net/client/ad - reject

# > 识货
^https?://www.shihuo.cn/app3/saveAppInfo - reject

# > AbemaTV Unlock
^https?://api.abema.io/v\d/ip/check - reject

# > Other
^https?://.+allOne.php\?ad_name=main_splash_ios - reject
^https?://.+nga.cn.+\bhome.+\b=ad - reject
^https?://.+resource=article/recommend\&accessToken= - reject
^https?://113.200.76.*:16420/sxtd.bike2.01/getkey.do - reject
^https?://cdn.api.fotoable.com/Advertise/ - reject
^https?://counter.ksosoft.com/ad.php - reject
^https?://creatives.ftimg.net/ads - reject
^https?://dd.iask.cn/ddd/adAudit - reject
^https?://g.tbcdn.cn/mtb/ - reject
^https?://huichuan.sm.cn/jsad - reject
^https?://iflow.uczzd.cn/log/ - reject
^https?://iphone265g.com/templates/iphone/bottomAd.js - reject
^https?://m.+.china.com.cn/statics/sdmobile/js/ad - reject
^https?://m.+.china.com.cn/statics/sdmobile/js/mobile.advert.js - reject
^https?://m.+.china.com.cn/statics/sdmobile/js/mobileshare.js - reject
^https?://m.elecfans.com/static/js/ad.js - reject
^https?://mobile-pic.cache.iciba.com/feeds_ad/ - reject
^https?://nga.cn.+\bhome.+\b=ad - reject
^https?://overseas.weico.cc/portal.php\?a=get_coopen_ads - reject
^https?://player.hoge.cn/advertisement.swf - reject
^https?://ress.dxpmedia.com/appicast/ - reject
^https?://s3.pstatp.com/inapp/TTAdblock.css - reject
^https?://sdk.99shiji.com/ad/ - reject
^https?://statc.mytuner.mobi/media/banners/ - reject
^https?://static.cnbetacdn.com/assets/adv - reject
^https?://static.iask.cn/m-v20161228/js/common/adAudit.min.js - reject
^https?://v.17173.com/api/Allyes/ - reject
^https?://wmedia-track.uc.cn - reject
^https?://www.ft.com/__origami/service/image/v2/images/raw/https%3A%2F%2Fcreatives.ftimg.net%2Fads* - reject
^https?://www.lianbijr.com/adPage/ - reject


[Script]
# > Weibo
http-response ^https?://m?api\.weibo\.c(n|om)/2/(statuses/(unread|extend|positives/get|(friends|video)(/|_)timeline)|stories/(video_stream|home_list)|(groups|fangle)/timeline|profile/statuses|comments/build_comments|photo/recommend_list|service/picfeed|searchall|cardlist|page|\!/photos/pic_recommend_status) requires-body=1,script-path=https://raw.githubusercontent.com/yichahucha/surge/master/wb_ad.js
http-response ^https?://(sdk|wb)app\.uve\.weibo\.com(/interface/sdk/sdkad.php|/wbapplua/wbpullad.lua) requires-body=1,script-path=https://raw.githubusercontent.com/yichahucha/surge/master/wb_launch.js

# > WeChat
http-request ^https://mp\.weixin\.qq\.com/mp/getappmsgad script-path=https://Choler.github.io/Surge/Script/WeChat.js

# > Youtube
http-request ^https://[\s\S]*\.googlevideo\.com/.*&(oad|ctier) script-path=https://Choler.github.io/Surge/Script/YouTube.js

# > Aweme
http-response ^https://[\s\S]*\/aweme/v1/(feed|aweme/post|follow/feed)/ requires-body=1,max-size=-1,script-path=https://Choler.github.io/Surge/Script/Aweme.js
http-response ^https?://[a-z]*\.snssdk\.com/bds/feed/stream/ requires-body=1,max-size=-1,script-path=https://Choler.github.io/Surge/Script/Super.js

# > Toutiao
http-response ^https?://[\s\S]*\.snssdk\.com/api/news/feed/v88/ requires-body=1,max-size=-1,script-path=https://Choler.github.io/Surge/Script/Toutiao.js

# > Kaola
http-request ^https://sp\.kaola\.com/api/openad$ script-path=https://Choler.github.io/Surge/Script/Kaola.js

# > QQ News
http-response https://r\.inews\.qq.com\/get(QQNewsUnreadList|RecommendList) requires-body=1,max-size=-1,script-path=https://Choler.github.io/Surge/Script/QQNews.js

# > RRad
http-response ^https://api\.rr\.tv/v3plus/index/(channel|todayChoice)$ requires-body=1,max-size=-1,script-path=https://Choler.github.io/Surge/Script/RRad.js

# > Zhihu
http-response ^https://api.zhihu.com/moments\?(action|feed_type) requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20zhihu%20feed.js
http-response ^https://api.zhihu.com/topstory/recommend requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20zhihu%20recommend.js
http-response ^https://api.zhihu.com/v4/questions requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20zhihu%20answer.js
http-response ^https://api.zhihu.com/people/ requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20zhihu%20people.js

# > Bilibili
http-response https://app.bilibili.com/x/v2/space\?access_key requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20bilibili%20space.js
http-response https://app.bilibili.com/x/resource/show/tab\?access_key requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20bilibili%20tab.js
http-response https://app.bilibili.com/x/v2/feed/index\?access_key requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20bilibili%20feed.js
http-response https://app.bilibili.com/x/v2/account/mine\?access_key requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20bilibili%20account.js
http-response https://app.bilibili.com/x/v2/view\?access_key requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20bilibili%20view%20relate.js
http-response https://api.live.bilibili.com/xlive/app-room/v1/index/getInfoByRoom\?access_key requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/Surge-Script/master/surge%20bilibili%20live.js
http-response https://api.bilibili.com/pgc/view/app/season requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/UnblockBangumi/master/surge/surge%20bilibili%20season.js
http-response https://api.bilibili.com/pgc/player/api/playurl requires-body=1,max-size=0,script-path=https://raw.githubusercontent.com/onewayticket255/UnblockBangumi/master/surge/surge%20bilibili%20playurl.js


# > Display Netflix TV series and movie's IMDb ratings, Douban ratings, rotten tomato and country/region
http-request ^https?://ios\.prod\.ftl\.netflix\.com/iosui/user/.+path=%5B%22videos%22%2C%\d+%22%2C%22summary%22%5D script-path=https://raw.githubusercontent.com/yichahucha/surge/master/nf_rating.js
http-response ^https?://ios\.prod\.ftl\.netflix\.com/iosui/user/.+path=%5B%22videos%22%2C%\d+%22%2C%22summary%22%5D requires-body=1,script-path=https://raw.githubusercontent.com/yichahucha/surge/master/nf_rating.js

# > Display commodity historical price - JD
http-response ^https?://api\.m\.jd\.com/client\.action\?functionId=(wareBusiness|serverConfig) requires-body=1,script-path=https://raw.githubusercontent.com/yichahucha/surge/master/jd_price.js

# > Display commodity historical price - Taobao
http-response ^https://trade-acs.m.taobao.com/gw/mtop.taobao.detail.getdetail requires-body=1,script-path=https://raw.githubusercontent.com/yichahucha/surge/master/tb_price.js

[MITM]
skip-server-cert-verify = true
tcp-connection = false

hostname= api*.tiktokv.com, api*.amemv.com, *.tiktokcdn.com, api*.musical.ly, api.m.jd.com,trade-acs.m.taobao.com,*.360buyimg.com,*.amemv.com,*.chelaile.net.cn,*.cnbetacdn.com,*.didistatic.com,*.doubanio.com,*.google-analytics.com,*.iydsj.com,*.k.sohu.com,*.kfc.com,*.kingsoft-office-service.com,*.meituan.net,*.ofo.com,*.pixiv.net,*.pstatp.com,*.snssdk.com,*.uve.weibo.com,*.wikipedia.org,*.wikiwand.com,*.ydstatic.com,*.youdao.com,*.youtube.com,*.zhuishushenqi.com,*.zymk.cn,101.201.62.22,113.105.222.132,113.96.109.*,118.178.214.118,119.18.193.135,121.14.89.216,121.9.212.178,123.59.31.1,14.21.76.30,153.3.236.81,180.101.212.22,183.232.237.194,183.232.246.225,183.60.159.227,218.11.3.70,59.151.53.6,59.37.96.220,789.kakamobi.cn,a.apicloud.com,a.applovin.com,a.qiumibao.com,a.sfansclub.com,a.wkanx.com,aarkissltrial.secure2.footprint.net,acs.m.taobao.com,act.vip.iqiyi.com,activity2.api.ofo.com,adm.10jqka.com.cn,adproxy.autohome.com.cn,adse.ximalaya.com,afd.baidu.com,api*.musical.ly,api*.tiktokv.com,api.abema.io,api.app.vhall.com,api.bilibili.com,api.chelaile.net.cn,api.daydaycook.com.cn,api.douban.com,api.feng.com,api.fengshows.com,api.gotokeep.com,api.huomao.com,api.intsig.net,api.jr.mi.com,api.jxedt.com,api.k.sohu.com,api.kkmh.com,api.laifeng.com,api.live.bilibili.com,api.m.jd.com,api.m.mi.com,api.mddcloud.com.cn,api.mgzf.com,api.psy-1.com,api.rr.tv,api.smzdm.com,api.tv.sohu.com,api.wallstreetcn.com,api.weibo.cn,api.xiachufang.com,api.zhihu.com,api.zhuishushenqi.com,api5.futunn.com,api-mifit.huami.com,api-mifit-cn.huami.com,api-release.wuta-cam.com,app.10086.cn,app.58.com,app.api.ke.com,app.bilibili.com,app.m.zj.chinamobile.com,app.mixcapp.com,app.variflight.com,app.wy.guahao.com,app2.autoimg.cn,appsdk.soku.com,atrace.chelaile.net.cn,b.zhuishushenqi.com,c.m.163.com,cap.caocaokeji.cn,capi.douyucdn.cn,capi.mwee.cn,cdn.kuaidi100.com,cdn.moji.com,channel.beitaichufang.com,classbox2.kechenggezi.com,client.mail.163.com,cms.daydaycook.com.cn,connect.facebook.net,consumer.fcbox.com,creatives.ftimg.net,creditcard.ecitic.com,d.1qianbao.com,daoyu.sdo.com,dapis.mting.info,dl.app.gtja.com,dongfeng.alicdn.com,dsp-impr2.youdao.com,dspsdk.abreader.com,e.dangdang.com,erebor.douban.com,fdfs.xmcdn.com,fm.fenqile.com,frodo.douban.com,fuss10.elemecdn.com,g1.163.com,gateway.shouqiev.com,gorgon.youdao.com,gw.alicdn.com,gw-passenger.01zhuanche.com,hm.xiaomi.com,hui.sohu.com,huichuan.sm.cn,i.weread.qq.com,i.ys7.com,i1.hoopchina.com.cn,iapi.bishijie.com,iface.iqiyi.com,iface2.iqiyi.com,img*.doubanio.com,img.jiemian.com,img.zuoyebang.cc,img01.10101111cdn.com,img1.126.net,img1.doubanio.com,img3.doubanio.com,impservice.dictapp.youdao.com,impservice.youdao.com,interface.music.163.com,ios.prod.ftl.netflix.com,ios.wps.cn,kano.guahao.cn,lives.l.qq.com,m*.amap.com,m.aty.sohu.com,m.client.10010.com,m.creditcard.ecitic.com,m.ibuscloud.com,m.yap.yahoo.com,m5.amap.com,ma.ofo.com,mage.if.qidian.com,mapi.appvipshop.com,mapi.mafengwo.cn,mapi.weibo.com,mbl.56.com,media.qyer.com,mi.gdt.qq.com,mimg.127.net,mlife.cmbchina.com,mmg.aty.sohu.com,mmgr.gtimg.com,mob.mddcloud.com.cn,mobile-api2011.elong.com,mp.weixin.qq.com,mrobot.pcauto.com.cn,mrobot.pconline.com.cn,ms.jr.jd.com,msspjh.emarbox.com,newsso.map.qq.com,nex.163.com,nnapp.cloudbae.cn,open.qyer.com,p.kuaidi100.com,p1.music.126.net,pic.k.sohu.com,pic1.chelaile.net.cn,pic1cdn.cmbchina.com,pic2.zhimg.com,portal-xunyou.qingcdn.com,pss.txffp.com,r.inews.qq.com,render.alipay.com,resource.cmbchina.com,res-release.wuta-cam.com,ress.dxpmedia.com,richmanapi.jxedt.com,rm.aarki.net,rtbapi.douyucdn.cn,service.4gtv.tv,slapi.oray.net,smkmp.96225.com,snailsleep.net,sp.kaola.com,ssl.kohsocialapp.qq.com,sso.ifanr.com,static.api.m.panda.tv,static.vuevideo.net,static1.keepcdn.com,staticlive.douyucdn.cn,storage.wax.weibo.com,support.you.163.com,supportda.ofo.com,thor.weidian.com,trade-acs.m.taobao.com,ups.youku.com,wapwenku.baidu.com,wenku.baidu.com,www.dandanzan.com,www.facebook.com,www.flyertea.com,www.ft.com,www.oschina.net,www.zhihu.com,youtubei.googleapis.com,zhidao.baidu.com

enable = true
ca-passphrase = UNCLEWANG
ca-p12 = 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
```