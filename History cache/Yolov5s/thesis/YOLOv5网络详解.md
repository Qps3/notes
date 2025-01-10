# YOLOv5网络详解



本文链接：[https://blog.csdn.net/qq\_37541097/article/details/123594351](https://blog.csdn.net/qq_37541097/article/details/123594351)



官方源码仓库：[https://github.com/ultralytics/yolov5](https://github.com/ultralytics/yolov5)  


视频讲解：[https://www.bilibili.com/video/BV1T3411p7zR](https://www.bilibili.com/video/BV1T3411p7zR)

---

#### 文章目录

- - [0 前言](https://blog.csdn.net/qq_37541097/article/details/123594351#0__10)
    - [1 网络结构](https://blog.csdn.net/qq_37541097/article/details/123594351#1__36)
    - [2 数据增强](https://blog.csdn.net/qq_37541097/article/details/123594351#2__128)
    - [3 训练策略](https://blog.csdn.net/qq_37541097/article/details/123594351#3__154)
    - [4 其他](https://blog.csdn.net/qq_37541097/article/details/123594351#4__165)
    - - [4.1 损失计算](https://blog.csdn.net/qq_37541097/article/details/123594351#41__166)
        - [4.2 平衡不同尺度的损失](https://blog.csdn.net/qq_37541097/article/details/123594351#42__177)
        - [4.3 消除Grid敏感度](https://blog.csdn.net/qq_37541097/article/details/123594351#43_Grid_183)
        - [4.4 匹配正样本(Build Targets)](https://blog.csdn.net/qq_37541097/article/details/123594351#44_Build_Targets_219)

---

### 0 前言

在前面我们已经介绍过了YOLOv1~v4的网络的结构，今天接着上次的YOLOv4再来聊聊[YOLOv5](https://so.csdn.net/so/search?q=YOLOv5&spm=1001.2101.3001.7020)，如果还不了解YOLOv4的可以参考之前的[博文](https://blog.csdn.net/qq_37541097/article/details/123229946)。YOLOv5项目的作者是`Glenn Jocher`并不是原`Darknet`项目的作者`Joseph Redmon`。并且这个项目至今都没有发表过正式的论文。之前翻阅该项目的`issue`时，发现有很多人问过这个问题，有兴趣的可以翻翻这个issue [#1333](https://github.com/ultralytics/yolov5/issues/1333#issuecomment-936818045)。作者当时也有说准备在2021年的12月1号之前发表，并承诺如果到时候没有发表就吃掉自己的帽子。  
![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700194.png)

(⊙o⊙)…，emmm，但这都2022年了，也不知道他的帽子是啥味儿。过了他承诺的发表期限后，很多人还去该`issue`下表示"关怀"，问啥时候吃帽子，下面这位大哥给我整笑了。

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700195.png)  
本来`Glenn Jocher`是准备要发表论文的，但至于为啥没发成作者并没有给出原因。我个人的猜测是自从YOLOv4发表后，有很多人想发这方面的文章，然后在YOLOv4上进行改动，改动过程中肯定有人把YOLOv5仓库里的一些技术拿去用了（YOLOv4论文4月出的，YOLOv5仓库5月就有了）。大家改完后发了一堆文章，那么YOLOv5的技术就被零零散散的发表到各个文章中去了。`Glenn Jocher`一看，这也太卷了吧，你们都把我技术写了，那我还写个锤子，直接撂挑子不干了。

当然以上都是我个人yy哈，回归正题，YOLOv5仓库是在`2020-05-18`创建的，到今天已经迭代了很多个大版本了，现在（`2022-3-19`）已经迭代到`v6.1`了。所以本篇博文讲的内容是针对`v6.1`的，大家阅读的时候注意看下版本号，不同的版本内容会略有不同。前几天我在YOLOv5项目中向作者提了一个issue [#6998](https://github.com/ultralytics/yolov5/issues/6998)，主要是根据当前的源码做了一个简单的总结，然后想让作者帮忙看看总结的内容是否有误，根据作者的反馈应该是没啥问题的，所以今天就来谈谈我个人的见解。下表是当前(`v6.1`)官网贴出的关于不同大小模型以及输入尺度对应的`mAP`、推理速度、参数数量以及理论计算量`FLOPs`。

![image-20240103170332700](./assets/image-20240103170332700.png)

---

### 1 网络结构

关于YOLOv5的网络结构其实网上相关的讲解已经有很多了。网络结构主要由以下几部分组成：

- **Backbone**: `New CSP-Darknet53`
- **Neck**: `SPPF`, `New CSP-PAN`
- **Head**: `YOLOv3 Head`

下面是我根据`yolov5l.yaml`绘制的网络整体结构，YOLOv5针对不同大小（`n`, `s`, `m`, `l`, `x`）的网络整体架构都是一样的，只不过会在每个子模块中采用不同的深度和宽度，分别应对`yaml`文件中的`depth_multiple`和`width_multiple`参数。还需要注意一点，官方除了`n`, `s`, `m`, `l`, `x`版本外还有`n6`, `s6`, `m6`, `l6`, `x6`，区别在于后者是针对更大分辨率的图片比如`1280x1280`，当然结构上也有些差异，后者会下采样64倍，采用4个预测特征层，而前者只会下采样到32倍且采用3个预测特征层。本博文只讨论前者。下面这幅图（`yolov5l`）有点大，大家可以下载下来仔细看一下。

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700196.png)

通过和上篇博文讲的YOLOv4对比，其实YOLOv5在**Backbone**部分没太大变化。但是YOLOv5在`v6.0`版本后相比之前版本有一个很小的改动，把网络的第一层（原来是`Focus`模块）换成了一个`6x6`大小的卷积层。**两者在理论上其实等价的**，但是对于现有的一些GPU设备（以及相应的优化算法）使用`6x6`大小的卷积层比使用`Focus`模块更加高效。详情可以参考这个issue [#4825](https://github.com/ultralytics/yolov5/issues/4825)。下图是原来的`Focus`模块(和之前`Swin Transformer`中的`Patch Merging`类似)，将每个`2x2`的相邻像素划分为一个`patch`，然后将每个`patch`中相同位置（同一颜色）像素给拼在一起就得到了4个`feature map`，然后在接上一个`3x3`大小的卷积层。这和直接使用一个`6x6`大小的卷积层等效。

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700197.png)

在**Neck**部分的变化还是相对较大的，首先是将`SPP`换成成了`SPPF`（`Glenn Jocher`自己设计的），这个改动我个人觉得还是很有意思的，两者的作用是一样的，但后者效率更高。`SPP`结构如下图所示，是将输入并行通过多个不同大小的`MaxPool`，然后做进一步融合，能在一定程度上解决目标多尺度问题。

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700198.png)  
而`SPPF`结构是将输入串行通过多个`5x5`大小的`MaxPool`层，这里需要注意的是串行两个`5x5`大小的`MaxPool`层是和一个`9x9`大小的`MaxPool`层计算结果是一样的，串行三个`5x5`大小的`MaxPool`层是和一个`13x13`大小的`MaxPool`层计算结果是一样的。

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700199.png)

下面做个简单的小实验，对比下`SPP`和`SPPF`的计算结果以及速度，代码如下（注意这里将`SPPF`中最开始和结尾处的`1x1`卷积层给去掉了，只对比含有`MaxPool`的部分）：

```python
import time
import torch
import torch.nn as nn


class SPP(nn.Module):
    def __init__(self):
        super().__init__()
        self.maxpool1 = nn.MaxPool2d(5, 1, padding=2)
        self.maxpool2 = nn.MaxPool2d(9, 1, padding=4)
        self.maxpool3 = nn.MaxPool2d(13, 1, padding=6)

    def forward(self, x):
        o1 = self.maxpool1(x)
        o2 = self.maxpool2(x)
        o3 = self.maxpool3(x)
        return torch.cat([x, o1, o2, o3], dim=1)


class SPPF(nn.Module):
    def __init__(self):
        super().__init__()
        self.maxpool = nn.MaxPool2d(5, 1, padding=2)

    def forward(self, x):
        o1 = self.maxpool(x)
        o2 = self.maxpool(o1)
        o3 = self.maxpool(o2)
        return torch.cat([x, o1, o2, o3], dim=1)


def main():
    input_tensor = torch.rand(8, 32, 16, 16)
    spp = SPP()
    sppf = SPPF()
    output1 = spp(input_tensor)
    output2 = sppf(input_tensor)

    print(torch.equal(output1, output2))

    t_start = time.time()
    for _ in range(100):
        spp(input_tensor)
    print(f"spp time: {time.time() - t_start}")

    t_start = time.time()
    for _ in range(100):
        sppf(input_tensor)
    print(f"sppf time: {time.time() - t_start}")


if __name__ == '__main__':
    main()
```

终端输出：

```
True
spp time: 0.5373051166534424
sppf time: 0.20780706405639648
```

通过对比可以发现，两者的计算结果是一模一样的，但`SPPF`比`SPP`计算速度快了不止两倍，快乐翻倍。

在**Neck**部分另外一个不同点就是`New CSP-PAN`了，在YOLOv4中，**Neck**的`PAN`结构是没有引入`CSP`结构的，但在YOLOv5中作者在`PAN`结构中加入了`CSP`。详情见上面的网络结构图，每个`C3`模块里都含有`CSP`结构。在**Head**部分，YOLOv3, v4, v5都是一样的，这里就不讲了。

---

### 2 [数据增强](https://so.csdn.net/so/search?q=%E6%95%B0%E6%8D%AE%E5%A2%9E%E5%BC%BA&spm=1001.2101.3001.7020)

在YOLOv5代码里，关于数据增强策略还是挺多的，这里简单罗列部分方法：

- **Mosaic**，将四张图片拼成一张图片，讲过很多次了  
    ![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700200.png)
    
- **Copy paste**，将部分目标随机的粘贴到图片中，前提是数据要有`segments`数据才行，即每个目标的实例分割信息。下面是`Copy paste`原论文中的示意图。  
    ![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700201.png)
    
- **Random affine(Rotation, Scale, Translation and Shear)**，随机进行仿射变换，但根据配置文件里的超参数发现只使用了`Scale`和`Translation`即缩放和平移。  
    ![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700202.png)
    
- **MixUp**，就是将两张图片按照一定的透明度融合在一起，具体有没有用不太清楚，毕竟没有论文，也没有消融实验。代码中只有较大的模型才使用到了`MixUp`，而且每次只有10%的概率会使用到。  
    ![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700203.png)
    
- **Albumentations**，主要是做些滤波、直方图均衡化以及改变图片质量等等，我看代码里写的只有安装了`albumentations`包才会启用，但在项目的`requirements.txt`文件中`albumentations`包是被注释掉了的，所以默认不启用。
  
- **Augment HSV(Hue, Saturation, Value)**，随机调整色度，饱和度以及明度。  
    ![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700204.png)
    
- **Random horizontal flip**，随机水平翻转  
    ![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700205.png)
    

---

### 3 训练策略

在YOLOv5源码中使用到了很多训练的策略，这里简单总结几个我注意到的点，还有些没注意到的请大家自己看下源码：

- **Multi-scale training(0.5~1.5x)**，多尺度训练，假设设置输入图片的大小为 640 × 640 640 \\times 640 640×640，训练时采用尺寸是在 0.5 × 640 ∼ 1.5 × 640 0.5 \\times 640 \\sim 1.5 \\times 640 0.5×640∼1.5×640之间随机取值，注意取值时取得都是32的整数倍（因为网络会最大下采样32倍）。
- **AutoAnchor(For training custom data)**，训练自己数据集时可以根据自己数据集里的目标进行重新聚类生成Anchors模板。
- **Warmup and Cosine LR scheduler**，训练前先进行`Warmup`热身，然后在采用`Cosine`学习率下降策略。
- **EMA(Exponential Moving Average)**，可以理解为给训练的参数加了一个动量，让它更新过程更加平滑。
- **Mixed precision**，混合精度训练，能够减少显存的占用并且加快训练速度，前提是GPU硬件支持。
- **Evolve hyper-parameters**，超参数优化，没有炼丹经验的人勿碰，保持默认就好。

---

### 4 其他

#### 4.1 损失计算

YOLOv5的损失主要由三个部分组成：

- **Classes loss**，分类损失，采用的是`BCE loss`，注意只计算正样本的分类损失。
- **Objectness loss**，`obj`损失，采用的依然是`BCE loss`，注意这里的`obj`指的是网络预测的目标边界框与GT Box的`CIoU`。这里计算的是所有样本的`obj`损失。
- **Location loss**，定位损失，采用的是`CIoU loss`，注意只计算正样本的定位损失。

![image-20240103171039077](./assets/image-20240103171039077.png)

#### 4.2 平衡不同尺度的损失

这里是指针对三个预测特征层（`P3, P4, P5`）上的`obj`损失采用不同的权重。在源码中，针对预测小目标的预测特征层（`P3`）采用的权重是`4.0`，针对预测中等目标的预测特征层（`P4`）采用的权重是`1.0`，针对预测大目标的预测特征层（`P5`）采用的权重是`0.4`，作者说这是针对`COCO`数据集设置的超参数。  
![image-20240103171056593](./assets/image-20240103171056593.png)

#### 4.3 消除Grid敏感度

在上篇文章YOLOv4中有提到过，主要是调整预测目标中心点相对Grid网格的左上角偏移量。下图是YOLOv2，v3的计算公式。

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700206.png)

其中：

![image-20240103171141134](./assets/image-20240103171141134.png)

![在这里插入图片描述](./assets/202401031700207.png)  

![image-20240103171218105](./assets/image-20240103171218105.png)

![image-20240103171243862](./assets/image-20240103171243862.png)

作者`Glenn Jocher`的原话如下，也可以参考issue [#471](https://github.com/ultralytics/yolov5/issues/471)：

> The original yolo/darknet box equations have a serious flaw. Width and Height are completely unbounded as they are simply out=exp(in), which is dangerous, as it can lead to runaway gradients, instabilities, NaN losses and ultimately a complete loss of training.

![image-20240103171302573](./assets/image-20240103171302573.png)


![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700208.png)

#### 4.4 匹配正样本(Build Targets)

![image-20240103171350301](./assets/image-20240103171350301.png)

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700209.png)  
剩下的步骤和YOLOv4中一致：

- 将`GT`投影到对应预测特征层上，根据`GT`的中心点定位到对应`Cell`，注意图中有三个对应的`Cell`。因为网络预测中心点的偏移范围已经调整到了 ( − 0.5 , 1.5 ) (-0.5, 1.5) (−0.5,1.5)，所以按理说只要`Grid Cell`左上角点距离`GT`中心点在 ( − 0.5 , 1.5 ) (−0.5,1.5) (−0.5,1.5)范围内它们对应的`Anchor`都能回归到`GT`的位置处。这样会让正样本的数量得到大量的扩充。
- 则这三个`Cell`对应的`AT2`和`AT3`都为正样本。

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700210.png)  
![image-20240103171430979](./assets/image-20240103171430979.png)

![在这里插入图片描述](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202401031700211.png)  
到此，YOLOv5相关的内容基本上都分析完了。当然由于个人原因，肯定还有一些细节被我忽略掉了，也建议大家自己看看源码，收获肯定会更多。