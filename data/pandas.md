## Pandas
### 常用操作
```python
# 访问
df.loc['i1':'i5','B':'E'] # 按标签访问
df.iloc[1:2,3:4] # 按数组下标访问
df['col'] # 访问列
df.isna() # 返回布尔数组，np.nan的项为True
df.isnull() # 返回布尔数组，None得项为True
df.notna() # 返回布尔数组，非np.nan的项为True
df.notnull() # 返回布尔数组，非None得项为True
df.str.???() # （字符串类型得数组适用）执行逐项字符串算法
sr.apply() # 逐元素应用给定函数
df.idxmax() # 类似argmax，但返回索引标签
'label' in df.index # 检查标签是否存在

# 增
df['new_col'] = my_col # 添加新列
df.join(df2) # 根据索引合并
pd.merge(df1, df2) # 合并
pd.concat(df1, df2) # 直接堆叠，相当于np.stack

# 删
df.drop(['i1', 'i2']) # 按索引删除行
del df['col'] # 删除列
df.drop_duplicates(subset=['id','name']) # 根据给定列去重

# 改
df.fillna(0) # 填充NaN项
df.index = df.new_index # 修改索引
df.columns = df.new_col # 修改列标签
df.reset_index() # 将索引重置为数组索引

```
### 数据导入导出
pandas数据分析当然牛逼，但只论其DataFrame类那不比numpy或者Excel强很多

pandas具有丰富的导入导出能力

导入：
- `read_csv`
- `read_excel`
- `read_clipboard`
- `read_html` 读取html里面的表格
- `read_table` 读取比较混乱的文本文件中的表格，效果可能一般
- `read_sql(sql_statement, conn)` 读取SQL数据库
- 还有hdf文件，等等

导出：
- `to_excel`
- `to_csv`
- `to_sql(table, conn)` 写入SQL数据库 
- `to_pickle`

### 关于索引
- 将某一类作为索引：`df.index = df.my_new_index_col`
- 重置索引，将原本乱序的index设为数组位置下标：`df.reset_index`

### 坑
- `df.loc`与`df.iloc`的异同
  
  同：这两个都可以通过数字下标来访问df的数据
  异：`loc`依据的是*行索引*，iloc依据的是*数组索引*。`DataFrame`的行是绑定了一个行索引（Index），它通常跟数组索引是一样的，但在时间序列分析种它也可以是日期而不是整数，在筛选出部分行view里面它不是连续的整数索引。而`iloc`则将df当作一般二维数组来处理根据位置来使用索引

- 视图、副本和切片赋值
  
  Python允许切片赋值，但这在Pandas中不一定奏效。

  切片赋值本质上是返回原数组的*视图*，然后对视图进行赋值操作来改变原数组的值

  - 当采用索引引用*单个元素*或者*切片*时，`DataFrame`**总是返回视图**，也即**可以赋值修改原数组**；
  - 当采用*花式索引*时，`DataFrame`**总是返回副本**，也即**不可以赋值修改原数组**
    
    常见的花式索引除了常规的索引数组访问之外，还有*布尔索引访问*

  本质上，这种行为的差异来自于*内存布局*。采用单索引和切片时，内存布局没有改变，通过记录起点、终点和步长可以表达一个数组的切片视图；但采用花式索引之后原数组的储存连续性不再保留，故只能返回副本。这一切取决于Numpy的工作，Pandas对其行为没有保证

- `np.nan != np.nan`，请使用notna()
- 在试图修改数据时，**不要使用形如`df['a']['b']`的链式索引**