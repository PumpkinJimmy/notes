## 机器学习笔记
### 性能指标
- 准确率 = （正确样本数 / 总样本数）
- 非均衡问题
  
    |预测/实际| 1       |       -1   |
    | :---:  | :---:    | :---:       |
    |   1    |TP（真阳）|  FP（假阳） |
    |   -1   |FN（假阴）|  TN（真阴） |
    查准律(Precision)= TP / (TP + FP)
    
    查准率相当于“挑出来的西瓜里有多少好的”
    
    召回率(Recall) = TP / (TP + FN)

    召回率相当于“好西瓜有多少被挑出来”

- ROC曲线与AUC（待完善）

## ImageNet
ImageNet是一个有标注的大型图像数据集。

每年都会有针对这个数据集的图像分类比赛，通常称为ILSVRC（ImageNet Large-Scale Visual Recognition Challenge）

这个比赛涵盖许多方面，如图像分类，图像定位等。

其中，衡量一个网络在ImageNet上的分类性能，我们通常用：
1. Top-5 Error Rate 一张图若其Softmax概率最大的五个类别中包含了正确的结果则认为这张图正确。以此标准计算的错误率就是Top-5
2. Top-1 就是取Softmax概率最大的分类作为结果，这个结果正确才算正确。

该项比赛产生了许多优秀模型如：
- AlexNet
- VGG
- Inception V3
- ResNet

都是实验中常用的baseline模型