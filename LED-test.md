建立LED
==========

1、确定目录结构。
------------------------

  ./applications/BearPi/BearPi-HM_Nano/sample路径下新建一个目录   myled
  
  用于存放业务源码文件。
  
  ./applications/BearPi/BearPi-HM_Nano/sampl/myled
  
  在myled下新增业务my_led，其中
  
  新建，hello_world.c为业务代码，为业务源码文件，
  
  新建，BUILD.gn为编译脚本，为业务源码编译脚本。
  
    .
    └── applications  
        └── BearPi 
            └── BearPi-HM_Nano
               └── sample
                    │── my_led
                    │  │── hello_world.c
                    │  └── BUILD.gn
                    └── BUILD.gn

2、编写业务代码。
------------------
在led_example.c中新建业务入口函数led_example，并实现业务逻辑         
     
    添加头文件：
    #include "ohos_init.h"
    #include "wifiiot_gpio.h"
    #include "wifiiot_gpio_ex.h"
    #include "unistd.h"

    void led_example(void)
    {
        GpioInit();
        IoSetFunc(WIFI_IOT_IO_NAME_GPIO_2,WIFI_IOT_IO_FUNC_GPIO_2_GPIO); //引脚号（2），功能（普通gpio）
        GpioSetDir(WIFI_IOT_IO_NAME_GPIO_2,WIFI_IOT_GPIO_DIR_OUT);//输出模式

        for(int i = 0;i < 10; i++)
        {
            GpioSetOutputVal(WIFI_IOT_IO_NAME_GPIO_2,0);
            usleep(1000000);
            GpioSetOutputVal(WIFI_IOT_IO_NAME_GPIO_2,1);
            usleep(1000000);
        }
        GpioSetOutputVal(WIFI_IOT_IO_NAME_GPIO_2,1);//IO口的高低电平
    }
    APP_FEATURE_INIT(led_example);#定义在"ohos_init.h"中
   
               
3、编写用于将业务构建成静态库的BUILD.gn文件。
------------------

./applications/BearPi/BearPi-HM_Nano/sampl/myled/BUILD.gn

BUILD.gn文件由三部分内容（目标、源文件、头文件路径）构成
    
    static_library("myled") {
    sources = [
        "led_example.c"
        ]
    include_dirs = [
        "//utils/native/lite/include",    #
        "//base/iot_hardware/interfaces/kits/wifiiot_lite"    # wifiiot_gpio.h路径
        ]
    }
    
    
====================================================================

 static_library中指定业务模块的编译结果，为静态库文件libmyapp.a，开发者根据实际情况完成填写。
 
 sources中指定静态库.a所依赖的.c文件及其路径，若路径中包含"//"则表示绝对路径（此处为代码根路径），若不包含"//"则表示相对路径。
 
include_dirs中指定source所需要依赖的.h文件路径。

BUILD.gn 必须大写

====================================================================


4、编写模块BUILD.gn文件，指定需参与构建的特性模块。
------------------
配置./applications/BearPi/BearPi-HM_Nano/sample/BUILD.gn文件

在features字段中增加索引"my_first_app:myapp"，使目标模块参与编译。


        #"D1_iot_wifi_ap:wifi_ap",
        #"D2_iot_wifi_sta_connect:wifi_sta_connect",        
        #"D3_iot_udp_client:udp_client",
        #"D4_iot_tcp_server:tcp_server",
        #"D5_iot_mqtt:iot_mqtt",        
        #"D6_iot_cloud_oc:oc_mqtt",
        #"D7_iot_cloud_onenet:onenet_mqtt",

        #"my_app:myapp"，
        "myled:myled",
        文件夹：目标(##(static_library("myled")##)
         
   my_appshi是相对路径；指向./applications/BearPi/BearPi-HM_Nano/sampl/my_app/BUILD.gn
   
   myapp是目标；指向./applications/BearPi/BearPi-HM_Nano/sampl/my_app/BUILD.gn中的tatic_library("myapp") 
       
5、在服务器端Linux编译、烧录。
------------------
  
在编译文件夹中运行“hpm dist”

    cd code_2/dome
    hpm dist

编译成功：

      -------------output/bin/Hi3861_wifiiot_app_ota.bin image info print end--------------

      < ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ >
                              BUILD SUCCESS                              
      < ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ >

      See build log from: /home/q123/code/code_2/demo/vendor/hisi/hi3861/hi3861/build/build_tmp/logs/build_kernel.log
      [214/214] STAMP obj/vendor/hisi/hi3861/hi3861/run_wifiiot_scons.stamp
      ohos BearPi-HM_Nano build success!
      @bearpi/bearpi_hm_nano: distribution building completed.


6、在win打开HiBurn。
------------------

1、选择COM口——

2、Setting中设置

     com setting
     
     波特率
     buad 为 921600
     选择烧录文件
     select fifle
     //code_2/demo/out/BearPi-HM_Nano/Hi3861_wifiiot_app_allinone.bin
     勾选Auto burn
  
3、单击 Connect

      Connecting...

4、单击开发板复位按键

5、开始下载
    
    Connecting...
    Ready to load at 0x10A00
    CCStartBurn
    total size:0x3C00
    loady succ
    Entry loader
    ============================================

6、点击Disconnect

7、下载完成


打开串口工具查看
==========

1、Serial
  选择COM口
  波特率为115200
  
2、单击开发板复位按键
      
    ready to OS start
    sdk ver:Hi3861V100R001C00SPC025 2020-09-03 18:10:00
    formatting spiffs...
    FileSystem mount ok.
    wifi init success!

    [DEMO] Hello world.
    00 00:00:00 0 196 D 0/HIVIEW: hilog init success.
    00 00:00:00 0 196 D 0/HIVIEW: log limit init success.

 3、成功显示


*************************************************
https://www.bilibili.com/video/BV1tv411b7SA?p=6
