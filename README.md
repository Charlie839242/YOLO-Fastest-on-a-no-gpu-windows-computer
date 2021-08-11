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































