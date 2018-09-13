此为项目网上资源，在此引用，仅学习使用。
使用说明：
需要安装的软件：Python2、Redis、MongoDB； 需要安装的Python模块：scrapy、requests、lxml。
进入cookies.py，填入你的微博账号（已有两个账号示例）。
进入settings.py，如果你填入的账号足够多，可以将`DOWNLOAD_DELAY = 10` 和 `CONCURRENT_REQUESTS = 1`注释掉。另外可以修改存放种子和去重队列的机器，可以存放在两台不同的机器上面。
运行launch.py启动爬虫，也可在命令行执行`scrapy crawl SinaSpider`（Linux只能采用后者）。
分布式扩展：将代码拷贝到一台新机器上，运行即可。注意各子爬虫要共用一个去重队列，即将settings.py里面的`FILTER_HOST`设成同一台机的IP。


Sina_Spider1为单机版本。<br>
Sina_Spider2在Sina_Spider1的基础上基于scrapy_redis模块实现分布式。<br>
Sina_Spider3增加了Cookie池的维护，优化了种子队列和去重队列。<br>
<br>
 --------------------------------------------------------------------------
<br>
20161215更新：
<br>
有人反映说爬虫一直显示爬了0页，没有抓到数据。
<br>
1、把settings.py里面的LOG_LEVEL = 'INFO'一行注释掉，使用默认的"DEBUG"日志模式，运行程序可查看是否正常请求网页。
<br>
2、注意程序是有去重功能的，所以要清空数据重新跑的话一定要把redis的去重队列删掉，否则起始ID被记录为已爬的话也会出现抓取为空的现象。清空redis数据 运行cleanRedis.py即可。
<br>
3、另外，微博开始对IP有限制了，如果爬的快 可能会出现403，大规模抓取的话需要加上代理池。
<br><br><br><br>
 ---------------------------------------------------------------------------
<br>
20170323更新：
<br>微博从昨天下午三点多开始做了一些改动，原本免验证码获取Cookie的途径已经不能用了。以前为了免验证码登录，到处找途径，可能最近爬的人多了，给封了。
<br>那么就直面验证码吧，走正常流程登录，才没那么容易被封。此次更新主要在于Cookie的获取途径，其他地方和往常一样（修改了cookies.py，新增了yumdama.py）。
<br>加了验证码，难度和复杂程度都提高了一点，对于没有编程经验的同学可能会有一些难度。
<br>验证码处理主要有两种：手动输入和打码平台自动填写（手动输入配置简单，打码平台输入适合大规模抓取）。
<br><br>手动方式流程：
<br>
1、下载PhantomJS.exe，放在python的安装路径（适合Windows系统，Linux请找百度）。
<br>
2、运行launch.py启动爬虫，中途会要求输入验证码，查看项目路径下新生成的aa.png，输入验证码 回车，即可。
<br>
<br>打码方式流程：
<br>
1、下载PhantomJS.exe，放在python的安装路径。
<br>
2、安装Python模块PIL（请自行百度，可能道路比较坎坷）
<br>
3、验证码打码：我使用的是 http://www.yundama.com/ （真的不是打广告..），将username、password、appkey填入yumdama.py（正确率挺高，weibo.cn正常的验证码是4位字符，1元可以识别200个）。
<br>（如果一直出现302，调试发现yumdama.py一直返回空字符串，可将yumdama.py中的apiurl改成 'http://api.yundama.net:5678/api.php' 试试，在第38行前后，原值是 'http://api.yundama.com/api.php' 。）
<br>
4、cookies.py中设置IDENTIFY=2，运行launch.py启动爬虫即可。
<br><br><br><br>
 ---------------------------------------------------------------------------
<br>
20170405更新：
<br>微博从4月1日开始对IP限制更严了，很容易就403 Forbidden了，解决的办法是加代理。从16年12月更新代码后爬微博的人多了许多，可能对weibo.cn造成了挺多无效访问。所以此次代码就不更新了，过滤一些爬虫新手，如果仍需大量抓取的，在middleware.py中加几行代码，带上代理就行了，难度也不大。没加代理的同学将爬虫速度再降低一点，还是能跑的。<br>
可能有挺多同学需要微博数据写论文，在群里找一下已有数据的同学吧，购买代理也不便宜。<br>
（我也没怎么跑微博，手上也没什么数据）
<br><br><br><br>
 ---------------------------------------------------------------------------
<br>
20170407更新：
<br>有些同学还用着SinaSpider1，现将SinaSpider1中获取Cookie的代码也作了更新，使用方法和SinaSpider3的一样，见上面的更新说明。
<br><br><br><br>
 ---------------------------------------------------------------------------
<br>
20170410更新：
<br>许多同学问微博帐号哪里买，淘宝上禁的有一点严，所以直接搜可能没搜到。需要的同学可以搜店铺名称：账号素材生产基地 或 互联网账号营销中心，看店铺里的商品，有老客户链接。偶尔会断货，购买多少自行斟酌。非广告，不需要的请忽略。
<br><br><br><br>
 ---------------------------------------------------------------------------
<br>
20170426更新：
<br>从昨天下午开始，weibo.cn的登录方式又变了，关闭了原来的登录页面，采用m.weibo.com的登录途径，登录过程中可能会出现图形解锁的验证码。隐约感觉有几个微博官方反爬虫的人正在暗处默默地盯着我，说不定什么时候就要请我去喝茶了。。
唉，图形解锁应该也是可以破解的，但是最近事多，要过两个星期才有空研究，有需要的可以等等，或者大伙自己可以研究一下，按像素识别。
<br><br><br><br>
 ---------------------------------------------------------------------------
<br>
20170509更新：
<br>1、http://weibo.cn改成了https://weibo.cn。
<br>2、图形解锁验证码的破解见博客 [《图形解锁破解（附Python代码）》](http://blog.csdn.net/bone_ace/article/details/71056741) 。微博爬虫的Cookie获取模块请自行更新。
<br><br>
<br>

