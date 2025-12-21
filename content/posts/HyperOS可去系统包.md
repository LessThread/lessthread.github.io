---
date : '2025-12-21T12:20:44+08:00'
title : 'HyperOS可去系统包'
description : 'HyperOS可去系统包'  
categories : '折腾'
tags: ["HyperOS"]
---
# 操作方法
保留应用数据及缓存卸载
```
adb shell cmd package uninstall -k <包名>
```

恢复出厂预设应用的adb命令
```
adb shell cmd package install-existing <包名>
```
# 可去安装包列表
推荐
```
adb shell pm uninstall --user 0 com.miui.analytics #Analytics——后台上传数据耗电

adb shell pm uninstall --user 0 com.miui.systemAdSolution #小米系统广告

adb shell pm uninstall --user 0 com.miui.hybrid #快应用服务框架

adb shell pm uninstall --user 0 com.miui.hybrid.accessory # 智慧生活
```

可选
```
1.小爱全家桶（后台耗电大户，但某些情况下又很有用）

adb shell pm uninstall --user 0 com.miui.voiceassist #小爱同学/超级小爱

adb shell pm uninstall --user 0 com.xiaomi.aicr #小米澎湃AI引擎
设置景深壁纸需要用到，缺失时换壁纸会一直卡分屏计算

adb shell pm uninstall --user 0 com.miui.voicetrigger #语音唤醒

adb shell pm uninstall --user 0 com.mediatek.voicecommand #语音控制-联发科处理器才有

adb shell pm uninstall --user 0 com.xiaomi.aireco #小爱建议

adb shell pm uninstall --user 0 com.xiaomi.aiasst.service #AI通话/小爱通话

adb shell pm uninstall --user 0 com.xiaomi.aiasst.vision #翻译

2.小米互联全家桶（后台耗电，但某些情况下又很有用）

adb shell pm uninstall --user 0 com.xiaomi.mirror #跨屏协同服务/MIUI+ Beta版

adb shell pm uninstall --user 0 com.xiaomi.mi_connect_service #小米互联通信服务

adb shell pm uninstall --user 0 com.milink.service #互联互通服务/小米妙享中心

adb shell pm uninstall --user 0 com.miui.mishare.connectivity #小米互传

adb shell pm uninstall --user 0 com.miui.phrase # 剪贴板与常用语/常用语-小米云剪贴板



3.传送门（超级小爱的识屏已经超越了它。。。）

adb shell pm uninstall --user 0 com.miui.contentextension #传送门

4.游戏加速服务（核心组件在手机管家里，所以就算卸了还是会给你弹加速窗口）

adb shell pm uninstall --user 0 com.xiaomi.gamecenter #游戏中心

adb shell pm uninstall --user 0 com.xiaomi.gamecenter.sdk.service #游戏服务

adb shell pm uninstall --user 0 com.xiaomi.migameservice #游戏高能时刻

adb shell pm uninstall --user 0 com.miui.vpnsdkmanager #MiuiVpnSdkManager
小米游戏加速的VPN服务，不影响安卓系统的VPN使用

adb shell pm uninstall --user 0 com.xiaomi.macro #自动连招

adb shell pm uninstall --user 0 com.xiaomi.barrage #弹幕通知

5.下载管理组件-小米手机内置的精简版迅雷下载

adb shell pm uninstall --user 0 com.android.providers.downloads.ui #下载管理

adb shell pm uninstall --user 0 com.android.providers.downloads #下载管理程序

6.小米支付全家桶（小米钱包和小米智能卡这3个app涉及NFC复制门禁卡功能）

adb shell pm uninstall --user 0 com.mipay.wallet # 小米钱包

adb shell pm uninstall --user 0 com.miui.nextpay #小米支付

adb shell pm uninstall --user 0 org.mipay.android.manager #Mipayservice

adb shell pm uninstall --user 0 com.xiaomi.payment #米币支付

adb shell pm uninstall --user 0 com.miui.tsmclient #小米智能卡

adb shell pm uninstall --user 0 com.miui.nextpay #小米智能卡网页组件

7.小米人车家体系

adb shell pm uninstall --user 0 com.xiaomi.mis #小米汽车互联服务（某些系统预装）

adb shell pm uninstall --user 0 com.miui.carlink #CarWith

adb shell pm uninstall --user 0 com.xiaomi.digitalkey #小米数字钥匙框架
```
# Ref
https://www.bilibili.com/opus/1073374956499238916