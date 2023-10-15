+++
title = 'Notebook'
date = 2023-12-31T14:49:50+08:00
draft = false
+++

Android官方文档：  
https://source.android.google.cn/docs/core?hl=zh-cn  
VIM使用：  
https://github.com/iggredible/Learn-Vim  
Rust学习：  
https://www.rust-lang.org/zh-CN/learn  
web教程：  
https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps/Getting_started  
git学习:  
https://learngitbranching.js.org/  

/***************************************************************************  
dlclose卸载libbt_vendor库后出现crash，dlclose调用无异常，调用栈显示是  

libbt_vendor内部调用了QMI接口qmi_client_init用于获取nv mac地址  
QMI接口最终会调用xport_open分别创建ctrl xport的线程和data xport的线程  
libbt_vendor使用完QMI功能后调用qmi_client_release接口释放client资源  
qmi_client_release内部实现仅关闭了data xport线程  
因此当blue_host调用dlclose卸载libbt_vendor时，qmi ctrl xport的线程还未结束造成crash  
解决方案：当QMI结束data xport的线程后调用qmi_cci_xport_qrtr_deinit结束线程  
修改后不再出现crash  
****************************************************************************/ 

### 计划：  
1 android avrcp分析  
2 android 启动流程，init进程，分区加载分析    
Android启动流程： https://blog.csdn.net/yangwen123/article/details/9029959  
uevent原理分析：  https://blog.csdn.net/W1107101310/article/details/80211885  
3 bt osi完善  
4 ble audio分析  