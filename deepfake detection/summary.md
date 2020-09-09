## CNN-RNN检测
思路简单粗暴：
input frames--(CNN)-->feature map sequence--(LSTM,全连接)-->T/F

## CNN+注意力机制
1. 所谓attention map就是一个与feature map同大小的mask，mask的每个pixel是一个介于[0,1]的数，可以理解为这一像素被篡改过的概率。通过对feature map和attention map做逐元素乘法，只有fake概率高的像素的具有高intensity
2. 计算attention map的实现有两种
   - 通过预先计算一些参数，构造线性回归，然后训练
   - 直接训练一个CNN去做这个工作
3. 注意力机制的好处是：模块化程度高，该部件可以作为挂件加入到经典的CNN预训练分类网络当中（如VGG,Xception）
4. （存疑）论文声称这种方法相比于先做face alignment定位人脸的算法，这个方法自然地完成了这一需求，无需额外处理
5. 本方法可以用于多种AI伪造人脸的情景（包括全脸生成，人脸身份替换，表情篡改等）（通过学习attention map实现）

## Warping Artifact
这一思路来自于一个观察：由于目前的Deepfake只能生成有限分辨率的图像，被篡改的视频中必然会有篡改的痕迹。具体地，为了适配原图，Deepfake必然需要做仿射变换，在假脸的边缘留下扭曲、模糊等痕迹。

这一方法的显著优点在于：无需使用Deepfakes类软件生成负样本，**只需要对真人脸执行模糊、仿射变换就可以构造负样本**。

论文声称由于此类artifacts广泛存在于deepfake视频，此法更具有鲁棒性