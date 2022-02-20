# Docker官方镜像
https://registry.hub.docker.com/r/yipengfei/movie-robot/

手把手教你安装：https://feather-purple-bdd.notion.site/b6c925bf2a9e44548bd4bdeea7d06946
关于机器人智能选择策略的详细说明和思考：https://feather-purple-bdd.notion.site/12f6d44243194c8c96a7e000b9dde023
# 特别说明
现在只有两种方式可以获得激活码使用本镜像：
* 如果你是开发者，可以扩展适配一个pt站点，可以获得激活码一枚。pt站的适配支持，源码中给出了一个示范；
* 通过打赏作者，可以获得激活码，打赏码在下面，也可以直接访问：https://yee-1254270141.cos.ap-beijing.myqcloud.com/movie_robot/pay.jpg
* 如果你有其他为项目作出重大贡献的行为，也可以获得激活码。

# 启动命令
```
docker run -itd --restart=always --name=movie-robot -v /volume1/docker_stable/movie-robot:/data --env 'LICENSE_KEY=abc'  yipengfei/movie-robot:latest
```
-v 中源路径改成你自己的
--env的激活码也改成你自己的

[申请一周体验激活码](https://docs.qq.com/form/page/DS3FsQktHcGJ0b2xH)
这个填好了我会每天晚上睡前发邀到邮箱里

官方telgram免费大群：[加入智能影音机器人交流群](https://t.me/+shOuvzcee9I4ZDll)
进群有机会获得免费的体验码


# 功能
定时自动从豆瓣电影的想看、在看、看过中获取影音信息，然后去PT站（支持多家站点）自动检索种子，找到最佳资源后按豆瓣电影分类提交到BT下载工具下载。在下载前，会自动检查你的Emby中是否已经存在。
基于此功能机制，还顺带具备了下列功能：
- 将一部刚上映，或者还没上映的电影加入想看，当PT站更新时会第一时间帮你下好，被Emby扫描到后直接观看。
- 对剧集类型的影视资源，如果你正在看一部没更新完的剧，只要pt站更新，也会帮你对比本地影音库缺少的剧集开始自动下载。
- 支持多PT站汇总搜索打分选种

针对新增下载和存量硬盘的影视库，机器人还可以帮你对乱七八糟下载种子名做标准化整理，整理后会按电影名+年份+tmdbid的方式存储，可以使用硬链接或复制模式的整理方式。
# 当前支持的站点
## mteam、hdsky、tjupt、hdchina、ssd、chdbits、keepfrds

# 更新日志
2022.02.20
1. 感谢大佬 miniers 贡献代码，支持了chdbits、keepfrds
2. 优化手动提交种子任务监测的通知友好性，对自由下载的剧集，识别集数信息；
3. 硬链接整部剧集是忽略音频文件

2022.02.19
1. 优化剧集数识别，认识回話，增加几个日漫命名格式。
2. compress策略将remux分数定义翻倍，绝对碾压其他权重。
3. 对pt站匹配不到年份的种子，放行，不做验证。

2022.02.17
1. 重磅更新：任何自己添加到下载器的种子，都可以被机器人监控到，被监控的种子，监控下载完成时，会自动做影视识别和硬链接或复制，同时发送通知；这个功能可以让不喜欢使用豆瓣想看体系的用户，享受机器人的识别整理改名功能；
2. 机器人的搜索方式，从以前的通过豆瓣上电影名称搜索，改为通过imdbid去pt站搜索。这可以有效避免复杂影视名称搜不到结果的问题，如日漫，和很长的电影名字。同时通过imdbid搜索的方式，也不会对年份做强验证，一些种子名称不规范年份不对而被机器人过滤的问题应该不会存在了。当然如果一个电影没有imdbid，还是会采用电影名去搜索；
3. 从豆瓣想看的电影下载完做识别和硬链接时，会将豆瓣的电影信息和年份给到识别器，让识别的准去率更高，未来可能还会直接给imdbid；
4. 升级优化默认的三套选种策略，详细说明：https://feather-purple-bdd.notion.site/12f6d44243194c8c96a7e000b9dde023
5. 回归老版本启动逻辑，docker容器第一次启动时，默认执行一次任务；

2022.02.15
1. 修复PT站存在emoji表情时导致搜索失败的BUG；
2. 修复因剧集智能整理改动，导致追剧选种解析错误的一个BUG，追剧集数选择有问题的朋友需要更新；
3. 修复TR下载器卡死会导致监测任务报错的BUG；

2022.02.14
1. 正式推出电影和电视剧智能管理Beta版！通过机器人下载好的影视，会自动识别影视信息，然后采用硬链接或复制的方式，以标准的影视命名，链接到指定的新目录，完美解决影音服务的刮削识别问题，帮它找好tmdbid！

2022.02.11
1. 支持电影文件管理功能，将下好名称乱七八糟的资源，统一整理为电影名+年份的标准文件夹结构，同时填入tmdbid，帮助影音服务器完成刮削；支持将多版本电影格式，合并到一个文件夹内，让Emby这种影音服务器可以播放时选择不同版本；整理后的文件可以选择采用硬链接或复制的方式，整理到新的目录，不会影响原有文件做种；（电视剧整理很快将会放出，敬请期待）

2022.02.08
1. 修复了使用transmission提示下载失败且无法推送的BUG
2. 增加企业微信推送支持


2022.02.06
1. 新增本地数据库，对下载记录做记录，一些看完从影视库删除或下载工具删除种子的电影，如果还未取消豆瓣想看，不会出现重复下载的BUG了。pt站再下载完成间隔增加新种子可能导致选到新种重复下载的BUG也修复了；
2. 新增通知系统，当前暂时只支持Bark通知；
3. 新增下载种子状态监听功能，当种子下载完成时，可以通过配置的通知方式，发送通知；
4. 新增两套规则配置，名称分别是compress、compact，全压缩选种规则和紧凑型存储空间选种，这两种新增的规则，全压缩版不会再出现emby无法播放的蓝光原盘内容，紧凑型会格外注重视频压缩质量，同时优先匹配1080为主的视频，体积会小很多；
5. 优化规则中name_keywords字符串匹配不区分大小写；
6. 优化pt站检索结果，增加种子id的识别；
7. 优化选种打分逻辑，更客观的打分逻辑；
8. 优化下载保存模式，增加区域（area）细分匹配项，同时cate和area都支持多个并且匹配模式；
9. 修复电视剧名称含有‘话’的分集方式可能识别错误的BUG；

2022.02.02
1. 优化日志输出形式，由以前的stdout调整为可写本地文件以及stdout，日志文件目录在映射的/data目录下


2022.02.01
1. 增加支持新站点ssd
2. 支持Plex媒体服务

2022.01.31
1. 新增支持hdchina；
2. 优化归一化实现及部分打分逻辑，增强选种效果；
3. 优化剧集选择逻辑，增加文件尺寸估算算法，来确认资源是否真的包含全集资源（很多站资源标题规则不一致，比如 权力的游戏第八季，无法通过名字准确识别到底包含了整季资源还是部分资源）；


2022.01.30
1. 重新设计实现了剧集（综艺、电视剧）的选种逻辑，如果你在豆瓣点了一部想看的电视剧，如果这部剧还没更新完，初次下载，会帮你把已更新剧集的种子，都下上。如果你已经有了最新的剧集，只要出新的，就自动帮你下最新一集；如果一部剧已经完整的更新完，则会选择全集资源包优先下载；
2. 加速PT站点第一次访问速度；

2022.01.28
1. 修复tjupt初次下载或很久没在页面点下载需要手动确认导致失败的BUG；
2. 修复Qbit下载工具web api登陆过期无法正常使用的BUG；
3. 修复cookie对多余分号处理错误的BUG；

2022.01.27
1. 增加pt站点北洋园支持
2. 豆瓣支持cookie登陆，解决极其罕见的电影信息需要登陆获取的问题
3. 修复hdsky下载数取成正在下载数的BUG；
4. 修复单个pt站点挂了，导致其他pt站搜索不可用的BUG；

# 赞赏一下
<img src="https://yee-1254270141.cos.ap-beijing.myqcloud.com/movie_robot/pay.jpg" width="310" height="310" alt="赞赏码" style="float: left;"/>

# 作者微信
微信号：yipengfei329
