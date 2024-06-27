## 软测+Python自动化测试交接

Date: 24/06/26

Author: Fenn 肖凯峰

***



## 前言



## 快速上手

### 工具与资料

#### 平台

1. **禅道**：测试用例、缺陷（bug）管理
2. **Confluence**：知识库，应当优先查询它（查不到还可以企业微信-微盘全局搜索）。包含Figma产品需求文档、研发文档、测试文档教程、Q&A等



#### 测试所需的环境

1. **UAT（用户验收测试环境）管理后台**：test.akuvox.com，内网访问。

   分为对讲云(蓝)和家居云(橙)，双云联动但也有部分没有打通。

   含账号管理、社区/单住户-家庭管理、设备管理、机型管理（固件、MAC、升级等）

   **账号体系**：Supermanage 超管->DIS 代理商账号->INS 安装商账号->enduser 终端用户

2. **家居云HA家庭后台**：my.uat.akubela.com，需要当前有家庭中心在网，功能同上云设备端后台

3. **词条库系统**：[词条库系统](http://192.168.10.118/newWords/#/layout/wordsNoProduct)，内网访问，测试时遇到词条机翻的问题可访问验证，但不要修改！



#### 工具与软件环境

1. **无线路由器**：WiFi最好配置在192.168.88.XXX网段，电脑固定IP在88.100，路由器网关固定在88.254，管理机门口机可固定在88.200，便于PNP和升级等功能；路由器DHCP的DNS需要有一条是“192.168.10.8”，否则无法访问Confluence

2. **自动化相关**：Confluence里关于搭建自动化环境有一个文档很详细，有打包好的环境包

   - **TortoiseSVN**：测试代码云同步与版本控制，建议每天早上同步一次代码，晚上提交更改
   - AndroidSDK, Appium, Python3.7, NodeJS, Java等在环境包中，执行环境包的bat
   - Python三方库：执行get-pip.py或者批量安装.bat，可提前学习uiautomator2, easyocr, weditor的用法
   - **UiAutomator2+WEditor**：前者在设备上模拟操作、定位元素完成自动化测试，后者在PC端定位Android界面上的元素，支持id, name, xpath等，但不能点击
   - **tidevice+Python facebook-wda+weditor**：Windows平台对iOS设备进行自动化测试的框架，用于测试BelaHome iOS App，前两个启动自动化，weditor定位元素同上
   - **雷电模拟器**：自动化测试可以用的Android模拟器
   - **scrcpy**：自动化测试可用，在电脑上投屏并点击Android设备屏幕的工具
   - **向日葵**：远程连接到测试间电脑
     - 测试间有两个，环境都搭好了，建议本地自测用例3遍通过后再向日葵到测试间验证；
     - 当然也可在家远程到工位电脑改用例、执行并查看报告
   - **Postman**：调试接口请求与响应

3. **软测相关**：

   - **MobaXTerm或SecureCRT**：看个人喜好二选一，有Macro捷径，PC的IP配置到88.100后可以有SSH/Telnet连接的预设，所有IP都有，预设不用再手动输IP和端口号；
     - Macro里的logcat捷径用于Android设备流式打印日志，tail捷径用于Linux设备流式打印日志，adbd开ADB端口
   - **PNP/SDMC**：升级工具，在升级用例中会用到，配置后设备重启后自动拉起更新PC发的固件
     - 前者支持除了KS41外的所有Android家居面板设备；
     - 后者为KS41等Linux家居面板使用
   - **固件NAS**：192.168.13.53
     - Android设备的固件存放在tsHome/AndroidSpace/<机型>/Rom-<版本分支>/rom-<机型>_<版本分支>_akubela_release
     - Linux设备存放在smartHome/NormalSpace/...
   - **CFGD**：贴近硬件层面的烧写MAC工具，在“对讲家居联动项目创建培训”文档最后有使用方法
   - **USB Burning Tool**：给 X933/C319 的 USB 固件烧录工具
     - 拆下设备底盒和后盖露出主板USB口，连接USB到电脑，参照文档使用，可有线刷机。
     - 需要先安装文件夹内的WorldCup_Device驱动
   - **tftpd32**：FTP工具，在Android或者Linux设备到电脑上，通过ftp互传文件
   - **HFS**：http文件服务器，一般用于挂载升级计划里的固件包
   - **Akubela 网关模拟器**：由Jason负责，可模拟添加一个子网关设备，在这个子网关上添加一系列模拟设备、场景等功能并触发。
     - 当测试环境没有需要的实体设备时，可以用网关模拟器，但是网关模拟器和实体还是有区别，有实体优先实体
   - **IP Scanner**：快捷扫描局域网内所有家居/对讲设备IP、MAC和固件版本

   - **KNX**：一般用不到
   - （用于测试的手机）**Belahome App、小睿 App**：公司家居业务的App不多说了
   - （用于测试的手机）**Sonos、Yale**等三方生态APP：如果测到Sonos音箱会用到，三方生态文档有写

4. **个人推荐安装的小工具**：

   - **LANDrop**：iOS/Android/PC/macOS之间互传文件，速度取决于路由器局域网上限。手机拍完复现步骤的视频，不用企业微信压缩再上传到电脑了
   - **Listary**：Windows全局文件搜索，Everything其实也很好用，但是Listary更像macOS的Spotlight一样无感。当开发过来找你看导出的日志的时候，直接Ctrl两下搜log马上就出来
   - **Typora**：Markdown编辑器，可视化实时编辑，谁用谁知道
   - **Windows Terminal**：Microsoft商店搜索，比CMD和Powershell好用
   - **IIS**：用来在电脑开FTP，这样只用把文件丢到FTP文件夹，设备端执行ftpget的捷径就可以拉去电脑上传过去的linux so库了
   - 梯子



### 工作主要内容

#### Daily

##### 软测阶段

1. 早9点，到岗位后检查邮件、企微群、禅道系统和固件SVN是否有新版本待测，跟进复现bug
1. 早10点确定当日任务，执行测试单或跟进复现bug
1. 



##### 自动化阶段

1. 早9点，到岗位后同步SVN，查看改动的代码
2. 



#### Android智能家居面板 软件测试

1. ##### 桌面启动器	 Akubela Panel Launcher

   - **包名**：com.akubela.panel; com.akuvox.voiceassistant; com.akuvox.upgradeui

   - **功能**：(1) 面板UI。包含家居设备控制（继电器、各类智能家居能力、监控、能耗等）、联系人对讲、家居云功能（安防告警、手动自动场景等）、机身功能（闹钟计时器、音乐、屏保壁纸、设置与升级等）——*可参照禅道机型版本测试单的“**关键功能点**”用例，在测试中快速了解面板功能。此文档稍后部分会提到各功能点的使用方法和注意事项*

     (2) 语音助手。没有接触过

     (3) 升级界面。设备接收到家居云下发版本更新（强制或用户确认更新）后，拉起的一个升级界面。

   - **常见测试用例**：

2. ##### 工厂测试	 Akubela FactoryTest

   - **包名：**com.akuvox.factorytest
   - **入口：**面板设置-系统设置-关于，连点页面顶部产品名称卡片10下
   - **功能：**整机测试、主板测试等。在工作中一般大版本会有厂测用例需要执行，主板测试需要拆下主板、上对应的主板工装、烧录固件后再执行主板测试；整机测试则在机器上正常测试。一般包含WIFI、蓝牙、Zigbee等10余项测试点，有些会自动测试，有些需要手动或Python自动化压测，全部通过为正常。*具体的厂测使用见Confluence文档*

3. 



## 软测





## 自动化测试



