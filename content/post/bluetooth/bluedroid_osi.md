+++
title = 'bluedroid_osi'
date = 2023-11-06T14:39:57+08:00
draft = false
+++

### thread.cc
线程创建函数：  
```cpp
thread_t* thread_new(const char* name) {
  return thread_new_sized(name, DEFAULT_WORK_QUEUE_CAPACITY);
}
```

#### bluedroid中主要线程  
**BTU线程**  
```cpp
static const char* BT_WORKQUEUE_NAME = "bt_workqueue";
  bt_workqueue_thread = thread_new(BT_WORKQUEUE_NAME);
  thread_set_rt_priority(bt_workqueue_thread, BTU_TASK_RT_PRIORITY);
  // Continue startup on bt workqueue thread.
  thread_post(bt_workqueue_thread, btu_task_start_up, NULL);
  return;
```
bt_workqueue线程执行btu_task_start_up,创建btu线程：  
```cpp
  message_loop_thread_ = thread_new("btu message loop");

  thread_set_rt_priority(message_loop_thread_, THREAD_RT_PRIORITY);
  thread_post(message_loop_thread_, btu_message_loop_run, nullptr);
```
bta_sys_main在btu线程中执行：  
```cpp
void bta_sys_sendmsg(void* p_msg) {
  base::MessageLoop* bta_message_loop = get_message_loop();

  bta_message_loop->task_runner()->PostTask(
      FROM_HERE, base::Bind(&bta_sys_event, static_cast<BT_HDR*>(p_msg)));
}
```

```cpp
void do_in_bta_thread(const base::Location& from_here,
                      const base::Closure& task) {
  base::MessageLoop* bta_message_loop = get_message_loop();

  bta_message_loop->task_runner()->PostTask(from_here, task);
}
```

**JNI线程**  
```cpp
static const char* BT_JNI_WORKQUEUE_NAME = "bt_jni_workqueue";
  bt_jni_workqueue_thread = thread_new_sized(BT_JNI_WORKQUEUE_NAME, MAX_JNI_WORKQUEUE_COUNT);
  thread_post(bt_jni_workqueue_thread, run_message_loop, nullptr);
```
使用ps -T -p查看线程,JNI线程的名称为BT Service Call,这是因为这个线程会附加到JVM  
```cpp
static void btif_jni_associate() {
  BTIF_TRACE_DEBUG("%s Associating thread to JVM", __func__);
  HAL_CBACK(bt_hal_cbacks, thread_evt_cb, ASSOCIATE_JVM);
}
```

```cpp
static void callback_thread_event(bt_cb_thread_evt event) {
  JavaVM* vm = AndroidRuntime::getJavaVM();
  if (event == ASSOCIATE_JVM) {
    JavaVMAttachArgs args;
    char name[] = "BT Service Callback Thread";
    args.version = JNI_VERSION_1_6;
    args.name = name;
    args.group = NULL;
    vm->AttachCurrentThread(&callbackEnv, &args);
    ALOGV("Callback thread attached: %p", callbackEnv);
  }
}
```
后续btif回调JNI函数都要用到这个callbackEnv  
```cpp
static JNIEnv *callbackEnv = NULL;
JNIEnv* getCallbackEnv() { return callbackEnv; }

class CallbackEnv {
public:
    CallbackEnv(const char *methodName) : mName(methodName) {
        mCallbackEnv = getCallbackEnv();
    }
......
```

**a2dp media线程**  
```cpp
  /* Start A2DP Source media task */
  btif_a2dp_source_cb.worker_thread =
      thread_new_sized("media_worker", MAX_MEDIA_WORKQUEUE_SEM_COUNT);
```




### fixed_queue.cc


### reactor.cc
