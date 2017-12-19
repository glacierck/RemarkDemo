# RemarkDemo

### 1.0 【2017.12.18】

微信自动修改好友备注，初步实现所有基本功能。

### 步骤

1、动态获取手机IMEI值

2、获取当前登录微信号的uin值

3、拼接加密取得微信数据库密码

4、将微信数据库EnMicroMsg.db复制到本地wx_data.db（直接操作微信数据库会使微信退出登录）

5、利用密码连接到本地的wx_data.db ，查询rconversation 和rcontact两个表，获取备注名称、wxid和最近一次聊天的时间

6、将上面三个字段储存自己创建的数据库localInfo.db

7、遍历数据库的数据，利用辅助功能类来自动修改好友备注

辅助功能类的使用，要具体每个界面的跳转都要有判断到，这次的项目封装一个方法不断的找想要的info节点，也有一个方法不断地点击找到的节点，找不到就会抛出异常

另外有两个需要注意的是，每个界面的返回键的id不一定都是一样的，我目前发现的就有三个了。然后想要利用坐标，又有一个bug，在具体聊天界面时，可能好友的头像就在返回键的下面，然后拿到的就是那个好友的头像，每次就点到头像而不是返回键，这样就陷入了两个界面不断的来回重复。最后的解决方案是不点返回了，然后每次启动搜索界面时，adb的命令是利用clear top 的方式来做，这样就不会产生一大堆界面。
然后就是修改备注时，为点击到备注框时，是一个TextView。点击了之后就变成了EditText，而且id也变化了，所以两次获取的时候要相应的修改id。

对了，上面的所有步骤放在一个大的while 循环不断的做任务，直到任务完成，模拟器崩溃或者直接关机都不会影响的，因为都是自动的。

然后所有步骤的异常都抛到外面统一处理，然后在最外面catch到，显示在界面中


### 1.2 【2017.12.19】  修改几处bug

1、时间为空时给个默认时间，并将其看成是符合要求的好友。

2、数据库密码错误，因为应用变量的hook失败，之前遗留的历史问题，这种手动修改正确的IMEI值即可

3、修改每次获取的好友的标准，之前是第四个为下划线，现在改为所有，记得过滤掉微信团队和公众号

4、对特殊昵称进行处理，否则插入数据库会出错

5、判断搜索的好友，如果没有的话会跳过ta，并将其数据库的status状态进行修改
