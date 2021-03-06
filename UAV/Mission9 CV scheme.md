# Mission 9 CV Scheme
## 模块定位拆卸视觉方案
### 1. 搜寻目标桅杆

深度图Otsu分割前景 + HSV/RGB色彩定位

- 采用HSV是因为HSV空间具有一定的光照鲁棒性；采用RGB是因为模块主体是亮蓝色；
- 采用深度图OTSU分割是因为我们假设**目标桅杆是一片空旷场地上的唯一物体**，故对深度图做Otsu分割会将背景与桅杆以及桅杆前方的地面分割开来。使用Otsu分割的另一个原因是可以自适应选取合适的全局分割阈值
- 此外，假设**目标场景中主要的蓝色干扰来自天空**。再深度图分割中我们肯定能将天空去除
- 此处假设光照的影响并不严重，否则需要*光照恢复*或者采用*高动态相机*

## 模块安装方案