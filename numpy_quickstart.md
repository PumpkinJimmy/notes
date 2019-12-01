## Numpy速查手册
1. ndarray基本属性
   - ndarray.ndim 维数
   - ndarray.shape 形状
   - ndarray.dtype 数据类型
   - ndarray.data 原始数据
2. 新建数组（都可以声明dtype）
   - array([])
   - zeros((row, col))
   - ones((row, col))
   - empty((row, col))
   - eye(n)
   - arange(from, [to, [step]])
   - random.random((row, col))
   - linspace(from, to, cnt)
   - fromfunction(f, (row, col))
3. 基本统计（所有操作可以加轴）
   - a.sum()
   - a.min() / a.max()
   - a.cumsum() / a.cumprod()
   - a.argmin() / a.argmax()
   - a.mean()
   - a.std() / a.var()
   - a.cov()
4. 分片与分片赋值
5. 迭代：直接当多维数组迭代或*扁平化*a.flat
6. 数组形状相关
   - a.ravel() 扁平化
   - a.reshape(row, col)
   - a.T 转置
   - a.resize((row, col)) inplace的reshape
7. 数组堆叠 （高维数组慎用）
   - stack/vstack/hstack
   - r_[]/c_[] 快捷堆叠表达 
8. 数组分割：hsplit/vsplit(a, cnt)
9. 数组复制与视图
   - 数组赋值操作不复制数据
   - a.view() 一个浅复制的视图，只复制了数组的元信息（如shape、dtype），数据是共享的，也是可以修改的。
   - a.copy() 深复制
10. 数组重复：a.repeat 可指定轴
11. 索引
    - 布尔索引 只取True的位置，配合数组比较
    - 花式索引 。。。
    - 广播 。。。
12. 线性代数
    - lingla.inv 逆
    - lingla.eig 特征值与特征向量
    - a1 @ a2 矩阵乘法
    - a1.dot(a2) 矩阵乘法
    - mat(a) 得到矩阵（就可以用*表示矩阵乘法）
    - m.A 矩阵得数组
13. 储存
    - save(f, array)
    - savez(f, a=array1, b=array2)
    - chunk = load(f)
