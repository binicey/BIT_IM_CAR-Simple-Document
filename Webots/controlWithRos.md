# ROS联合仿真

**Author**：Iccccy     **Data**：2022-02-08

> 参考资料：
>
> [Webots documentation: Tutorial 9: Using ROS (60 Minutes) (cyberbotics.com)](https://www.cyberbotics.com/doc/guide/tutorial-9-using-ros)
>
> [Webots documentation: Using ROS (cyberbotics.com)](https://www.cyberbotics.com/doc/guide/using-ros)
>
> [ ROS联合webots实战案例(一)安装配置webots_锡城小凯的博客-CSDN博客](https://blog.csdn.net/xiaokai1999/article/details/112545103)
>
> [ ROS联合webots实战案例(五)导航功能包入门1_锡城小凯的博客-CSDN博客](https://blog.csdn.net/xiaokai1999/article/details/112596613)

## 重编译Webots ROS API版本（可选）

现阶段Webtos当中对应的ROS API对应Noetic版本，建议使用其他版本的ROS的同学，使用如下命令进行重新编译，以免使用时出现功能问题。

```shell
export ROS_DISTRO=noetic  # or ROS_DISTRO=melodic, etc.
cd ${WEBOTS_HOME}/projects/default/controllers/ros
make
```

## 安装webots_ros功能包

使用如下命令进行安装：

```shell
sudo apt-get install ros-<ros版本>-webots-ros
```

## 测试运行

使用如下命令运行测试程序，检测webots_ros功能包的安装情况。

```shell
source /opt/ros/<ros version>/setup.bash
roslaunch webots_ros e_puck_line.launch
```

## webots_ros原码

可使用[cyberbotics/webots_ros: Webots ROS package (github.com)](https://github.com/cyberbotics/webots_ros)，构建自己的Webots控制功能包。

❗原码精读：

+ src/keyboard_teleop.cpp : 键盘控制程序；
+ scripts/webots_launcher.py ：webots启动程序；
+ 等等

## 常用webots仿真结点框架

```c++
#include <signal.h>
#include <std_msgs/String.h>
#include "ros/ros.h"
 
#include <webots_ros/set_float.h>
#include <webots_ros/set_int.h>
#include <webots_ros/Int32Stamped.h>
 
using namespace std;
#define TIME_STEP 32    //时钟
#define NMOTORS 2       //电机数量
#define MAX_SPEED 2.0   //电机最大速度
 
ros::NodeHandle *n;

static int controllerCount;
static vector<string> controllerList; 

ros::ServiceClient timeStepClient;          //时钟通讯客户端
webots_ros::set_int timeStepSrv;            //时钟服务数据

/*******************************************************
* Function name ：controllerNameCallback
* Description   ：控制器名回调函数，获取当前ROS存在的机器人控制器
* Parameter     ：
        @name   控制器名
* Return        ：无
**********************************************************/
void controllerNameCallback(const std_msgs::String::ConstPtr &name) { 
    controllerCount++; 
    controllerList.push_back(name->data);//将控制器名加入到列表中
    ROS_INFO("Controller #%d: %s.", controllerCount, controllerList.back().c_str());
}
/*******************************************************
* Function name ：quit
* Description   ：退出函数
* Parameter     ：
        @sig   信号
* Return        ：无
**********************************************************/
void quit(int sig) {
    ROS_INFO("User stopped the '/robot' node.");
    timeStepSrv.request.value = 0; 
    timeStepClient.call(timeStepSrv); 
    ros::shutdown();
    exit(0);
}

int main(int argc, char **argv) {
    setlocale(LC_ALL, ""); // 用于显示中文字符
    string controllerName;
    // 在ROS网络中创建一个名为robot_init的节点
    ros::init(argc, argv, "robot_init", ros::init_options::AnonymousName);
    n = new ros::NodeHandle;
    // 截取退出信号
    signal(SIGINT, quit);

    // 订阅webots中所有可用的model_name
    ros::Subscriber nameSub = n->subscribe("model_name", 100, controllerNameCallback);
    while (controllerCount == 0 || controllerCount < nameSub.getNumPublishers()) {
        ros::spinOnce();
    }
    ros::spinOnce();
    // 服务订阅time_step和webots保持同步
    timeStepClient = n->serviceClient<webots_ros::set_int>("robot/robot/time_step");
    timeStepSrv.request.value = TIME_STEP;

    // 如果在webots中有多个控制器的话，需要让用户选择一个控制器
    if (controllerCount == 1)
        controllerName = controllerList[0];
    else {
        int wantedController = 0;
        cout << "Choose the # of the controller you want to use:\n";
        cin >> wantedController;
        if (1 <= wantedController && wantedController <= controllerCount)
        controllerName = controllerList[wantedController - 1];
        else {
        ROS_ERROR("Invalid number for controller choice.");
        return 1;
        }
    }
    ROS_INFO("Using controller: '%s'", controllerName.c_str());
    // 退出主题，因为已经不重要了
    nameSub.shutdown();

    // main loop
    while (ros::ok()) {
        if (!timeStepClient.call(timeStepSrv) || !timeStepSrv.response.success) {
        ROS_ERROR("Failed to call service time_step for next step.");
        break;
        }
        ros::spinOnce();
    }
    timeStepSrv.request.value = 0;
    timeStepClient.call(timeStepSrv);
    ros::shutdown(); 
    return 0;
}
```

