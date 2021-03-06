#*FG.video*#
=====================================
##摘要：##
FG.video(Future Gadget Video)模块用于扩展Nginx视频服务方式，让NGINX支持优酷(youku.com), 
爱奇艺(iqiyi.com)，芒果(imgo.tv)等网站的flv，mp4文件拖放请求.

##编译：##
./configure --add-module=/path/to/FG.video
编译时请 **不要** 添加 --with-http_mp4_module --with-http_flv_moudle

##使用：##
1.参考conf.example修改配置文件。

##请求类型：##
###youkuns:###
优酷请求方式根据GET请求ns参数返回指定文件段

	http://127.0.0.1/test.flv?ns=1240_23456
	http://127.0.0.1/test.mp4?ns=1240_23456

###iqiyirange###
爱奇艺请求方式根据GET请求range参数返回指定文件段

	http://127.0.0.1/test.f4v?range=4096-8192

###pptv###
pptv请求方式根据GET请求start、end参数返回指定文件段

	http://127.0.0.1/test.mp4?start=123&end=0

###letv###
letv请求方式根据GET请求rstart, rend参数返回指定文件段
	
	http://127.0.0.1/test.letv?rstart=0&rend=4095
	http://127.0.0.1/test.ts?rstart=0&rend=4095

##指令：##
###flv指令:###
默认情况下，Nginx会按照字节位置拖拽方式处理flv请求中的start、end参数
(芒果，新浪，土豆都按照字节位置进行请求)，对于优酷的flv请求(按照时间，
youkuns)处理需要**添加type=youku**参数。

	curl -oa.flv 'http://127.0.0.1/test.flv?start=1234&end=1345&type=youku'
	curl -oa.flv 'http://127.0.0.1/test.flv?start=123.2&end=134.4&type=youku'
	curl -oa.flv 'http://127.0.0.1/test.flv?type=youku&ns=1230_23456'

###mp4指令：###
默认情况下Nginx会将mp4中的请求参数start, end当做时间处理。
对于youkuns请求需要**添加type=youku**参数。对于pptv请求需要添加**type=pptv**参数

	curl -oa.mp4 'http://127.0.0.1/test.mp4?start=12.3&end=14.5'
	curl -oa.mp4 'http://127.0.0.1/test.mp4?type=youku&ns=1234_23333'
	curl -oa.mp4 'http://127.0.0.1/test.mp4?start=1234&end=0&type=pptv'

###f4v指令：###
用于处理土豆、爱奇艺视频一般以f4v作为后缀名，目前遇到的f4v文件都是按照flv
格式封装的，所以处理上同flv格式。对于iqiyirange需要**添加type=iqiyi**

	curl -oa.f4v 'http://127.0.0.1/test.f4v?start=1234&end=1234'
	curl -oa.f4v 'http://127.0.0.1/test.f4v?range=1024-4096&type=iqiyi'

###letv指令：###
用于处理乐视视频请求

###ts指令：###
用于处理M3U8 Segment File。对于letv的ts请求需要添加**type=letv**参数

	curl -oa.ts 'http://127.0.0.1/test.ts'
	curl -oa.ts 'http://127.0.0.1/test.ts?rstart=0&rend=123&type=letv'

###作者：###
NadiaF0rever	(eeiwant@gmail.com)
