# YOLO-Fastest-on-a-no-gpu-computer
describes the process to train a YOLO-Fastest model on a no-gpu computer

该项目以仅有三张训练集和一个类别的数据集来进行演示，仅为记录一下流程。
部分参考自：
https://blog.csdn.net/weixin_41868104/article/details/115795348?ops_request_misc=&request_id=&biz_id=102&utm_term=yolo-fastest%E8%A1%8C%E4%BA%BA&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-.first_rank_v2_pc_rank_v29&spm=1018.2226.3001.4187

## 1.下载源码
  从 https://github.com/dog-qiuqiu/Yolo-Fastest 上下载源码

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
![image](https://github.com/Charlie839242/YOLO-Fastest-on-a-no-gpu-windows-computer/blob/main/pictures/0.png)
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
1 line：  

[net]
batch=16
subdivisions=8
width=320
height=320  

858 and 926 line：  

filters=18  

865 and 933 line：  

classes=1  
  
#可将17行的max_batches调成10来快速训练走下流程

```
  
## 3.构建数据集
/build/darknet/x64/data目录下就是数据集相关的文件。  
要用到的照片放在image文件夹中，然后标注后的xml文件放在Annotations文件夹中，然后运行makedata.py和/build/darknet/x64目录下的voc_label.py，最后将labels文件夹里的txt文件全部复制进入images，即可生成yolo格式的数据集。
![image](https://github.com/Charlie839242/YOLO-Fastest-on-a-no-gpu-windows-computer/tree/main/pictures/4.png)   
![image](https://github.com/Charlie839242/YOLO-Fastest-on-a-no-gpu-windows-computer/tree/main/pictures/5.png)   
接着在/build/darknet/x64/data目录下新建eye.data和eye.names：
Eye.data:
```
classes= 1
train  = data/train.txt
valid  = data/test.txt
names = data/eye.names
backup = backup/
```
eye.names:包含类别
```
eye
```

## 4.训练
在/build/darknet/x64新建一个model文件夹用来存放模型  

在/build/darknet/x64目录下新建一个eye.bat文件，作为预训练，预训练模型存放在/build/darknet/x64/model中，其中写入：
```
darknet partial cfg\yolo-fastest-1.1.cfg cfg\yolo-fastest-1.1.weights model\yolo-fastest-1.1.conv.109 109
pause
```
然后双击该文件开始预训练模型  

再新建一个eyefull.bat文件来完整训练，其模型存在其中/build/darknet/x64/backup中，向其中写入：
```
darknet detector train data\eye.data cfg\yolo-fastest-1.1.cfg model\yolo-fastest-1.1.conv.109 backup\
pause
```
双击开始训练模型：
![image](https://github.com/Charlie839242/YOLO-Fastest-on-a-no-gpu-windows-computer/tree/main/pictures/6.png)

## 5.运行模型
在/build/darknet/x64新建一个eyerun.bat，其中写入：
```
darknet.exe detector test ./data/eye.data ./cfg/yolo-fastest-1.1.cfg ./backup/yolo-fastest-1_final.weights ./data/test/0.jpg
pause
```
要检测图片存放在/build/darknet/x64/data/test中  
![image](https://github.com/Charlie839242/YOLO-Fastest-on-a-no-gpu-windows-computer/tree/main/pictures/7.png)





































