+++
title = 'Notebook'
date = 2023-12-31T14:49:50+08:00
draft = true
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

/***************************************************************  
C++静态函数调用非静态成员编译报错，使用GetInstance规避  
linux信号捕获：  
```cpp
int VendorInterface::data_service_setup_sighandler(void)
{
    struct sigaction sig_act;
    HDF_LOGI("%s: Entry", __func__);
    memset(&sig_act, 0, sizeof(sig_act));
    sig_act.sa_handler = data_service_sighandler;
    sigemptyset(&sig_act.sa_mask);
    sigaction(SIGTERM, &sig_act, NULL);
    return 0;
}
```
***************************************************************/  
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
/***************************************************************************  
HFP调试问题  
1 sdp搜索与a2dp的sdp搜索冲突导致搜索出错。  
2 搜索到服务记录后，配置了both支持hfp ag和headset的情况下，跳开hfp 的rfcomm chann，只连接了headset的rfcomm chann，导致hfp协议连接失败，没有触发slc连接。  
3 上层没有suspend a2dp，导致建立esco指令返回disallowed  
4   
***************************************************************************/  
/*****************************************************************************
android/linux平台 rtk芯片移植， xradio芯片移植  
rtk移植cts引脚特殊配置，波特率修改。  
bluedroid 从模式开发  
bt mesh移植开发，btmesh pts  
jr510 bt开发调试  
挂测出现概率性crash，由于bt hal收到controller的非法数据导致。  
从每次ipc log看，每次问题都出在切换波特率的时候，从bt hal代码看，每次切换波特率之前会关闭流控，切换完成后再开启流控  
查看uart驱动代码，设置流控的地方，在设置值改变的情况下没有更新到对应寄存器mcr中，实际上驱动层没有真正关闭流控。  
*****************************************************************************/

### 计划：  
1 android avrcp分析  
2 android 启动流程，init进程，分区加载分析    
Android启动流程： https://blog.csdn.net/yangwen123/article/details/9029959  
uevent原理分析：  https://blog.csdn.net/W1107101310/article/details/80211885  
3 bt osi完善  
4 ble audio分析  
