
!>  **更新时间**：{docsify-updated}  

## 设备升级操作手册



**设备升级流程**  

![设备升级流程图][pic1]

## 1.注册账号

1)	注册海极网个人账号，地址: 
	https://www.haigeek.com/developercenter/static/develop.html#/system/reg/

2)	开通海极网权限，登录BPM系统： 
	https://ssosci.haier.net:444/authority/home.jsp，搜索“海极网”，填写申请信息，直线经理审批


## 2.加入企业

1)	联系产线管理员，加入企业

2)	设置权限
由管理员根据操作权限分配角色

![流程][pic2]

权限详情请参考如下表格：

![权限详情][pic3]


## 3.创建产品

在第1步创建功能集，并在第3步进行型号适配


## 4.开通服务

在第5步“拓展功能”中开通设备升级服务，并启用具体型号

![开通服务1][pic4]
![开通服务2][pic5]


## 5.固件管理

创建固件，输入固件相关信息，上传固件包

<font color='red'> 注：只有在“拓展功能”中开通了设备升级服务的型号，才能创建固件 </font>

![固件管理][pic6]

## 6.创建升级任务

创建任务，填写任务相关信息

<font color='red'>
注：
1.	只有创建了固件的型号，才能创建升级任务
2.	仅升级适用版本：仅升级和当前版本不相同的版本；仅升级适用版本的设备ID：仅升级适用版本下指定的设备ID 
3.	通知接收人邮箱：升级结果将通过邮件通知给接收人
 </font>

![创建升级任务1][pic7]
![创建升级任务2][pic8]

## 7.灰度发布

升级任务创建完成后，到第2步“灰度发布”，输入发布范围，进行小范围灰度发布。
点击“灰度发布”，显示灰度发布结果。根据结果选择“返回修改”或“发布完成”。
点击“发布完成”，上传见证性资料，提交审核

<font color='red'>
注：灰度发布的发布范围仅支持按适用版本的设备ID升级
 </font>

![灰度发布1][pic9]
![灰度发布2][pic10]


## 8.升级策略

提交审核后，由“运营者”角色录入APP升级文案，APP升级注意事项

![升级策略][pic11]


## 9.任务发布
由管理员进行任务审核，确认信息无误后，选择“通过”，点击“确认并启动升级”
如果信息有误，选择“不通过”，点击“确认”，返回任务列表

![任务发布1][pic12]
![任务发布2][pic13]




[^-^]:常用图片注释
[pic1]:../_media/_scheduler/pic1.png
[pic2]:../_media/_scheduler/pic2.png
[pic3]:../_media/_scheduler/pic3.png
[pic4]:../_media/_scheduler/pic4.png
[pic5]:../_media/_scheduler/pic5.png
[pic6]:../_media/_scheduler/pic6.png
[pic7]:../_media/_scheduler/pic7.png
[pic8]:../_media/_scheduler/pic8.png
[pic9]:../_media/_scheduler/pic9.png

[pic10]:../_media/_scheduler/pic10.png
[pic11]:../_media/_scheduler/pic11.png
[pic12]:../_media/_scheduler/pic12.png
[pic13]:../_media/_scheduler/pic13.png


