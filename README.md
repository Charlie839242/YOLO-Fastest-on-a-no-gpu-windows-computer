# YOLO-Fastest-on-a-no-gpu-computer
describes the process to train a YOLO-Fastest model on a no-gpu computer

该项目以仅有三张训练集和一个类别的数据集来进行演示，仅为记录一下流程。
部分参考自：
https://blog.csdn.net/weixin_41868104/article/details/115795348?ops_request_misc=&request_id=&biz_id=102&utm_term=yolo-fastest%E8%A1%8C%E4%BA%BA&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-.first_rank_v2_pc_rank_v29&spm=1018.2226.3001.4187

## 1.下载源码
  从https://github.com/dog-qiuqiu/Yolo-Fastest上下载源码

## 2.编译源码
  这一步是为了得到darknet.dll和darknet.exe
### 2.1.安装相关软件
安装Cmake：https://cmake.org/download/
安装vs2015：https://my.visualstudio.com/Downloads?q=visual%20studio%202015&wt.mc_id=o~msft~vscom~older-downloads
安装opencv：https://opencv.org/releases/
注意：cmake的安装路径不能有中文，且opencv要添加到环境变量里
### 2.2.Cmake编译
修改源码目录下的Makefile中的前四行参数：
```
GPU=0
CUDNN=0
CUDNN_HALF=0
OPENCV=1
```  
接着打开Cmake，按照下图配置：
![image](https://github.com/Charlie839242/YOLO-Fastest-on-a-no-gpu-windows-computer/tree/main/pictures/0.png)
点击配置configure：
![image](https://github.com/Charlie839242/YOLO-Fastest-on-a-no-gpu-windows-computer/tree/main/pictures/1.png)    
注意一定选择vs14 2015，否则会出现AMD64相关报错。  
然后点击generate即可
  
  
### 2.3.vs2015
用vs2015打开ALL_BUILD.vcxproj
![image](https://github.com/Charlie839242/YOLO-Fastest-on-a-no-gpu-windows-computer/tree/main/pictures/2.png)  
配置成release输出，点击生成解决方案：  
![image](https://github.com/Charlie839242/YOLO-Fastest-on-a-no-gpu-windows-computer/tree/main/pictures/3.png)  

### 2.4.
此时已在目录下生成Release文件夹，将其中的darknet.dll和darknet.exe复制到/build/darknet/x64下，将ModelZoo\yolo-fastest-1.1_coco中的yolo-fastest-1.1.cfg, yolo-fastest-1.1-xl.cfg, yolo-fastest-1.1.weights, yolo-fastest-1.1-xl.weights复制到/build/darknet/x64/cfg下,并修改yolo-fastest-1.1.cfg：  
```
First Line：  

[net]
batch=16
subdivisions=8
width=320
height=320  

858 Line and 926 Line：  

filters=18  

865 and 933 line：  

classes=1  
  
  #可将17行的max_batches调成10来快速训练走下流程

```
  


































