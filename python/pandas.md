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
DataFrame(df, columns=, index=) # 重排行/列索引
df.fillna(0) # 填充NaN项
df.index = df.new_index # 修改索引
df.columns = df.new_col # 修改列标签
df.reset_index() # 将索引重置为数组索引
df.replace = 

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
- `to_sql(table, engine)` 写入SQL数据库 
- `to_pickle`

- 写入SQL数据库：
  
  pandas不能使用SQL数据库的连接直接写入，需要sqlalchemy库包装的engine来操作，如写入MySQL数据库：
  `engine = create_engine('mysql+pymysql://username:passwd@host/db')`

  （其中的`username, passwd, host, db`需要自己填写）

  然后`df.to_sql(table, engine)`即可写入SQL数据库


### 多级索引
很懂常见情况需要形如`性别->年龄`这样的多级索引的表格
- 创建多级索引：
  `pd.MultiIndex.from_product(['male', 'female'], [18, 19, 20])`
- 二维表转多级索引：
  ```python
  df.set_index(['sex','age']) # 法1：推荐
  DataFrame(df, index=mi) # 法2：采用自己生成的多级索引
  ```

- 多级索引转回一般二维表：
  `df.reset_index()`

- 展开多级索引到列：`df.stack()`；展开索引逆运算：`df.unstack()`

### 关于索引
- 将某一类作为索引：`df.index = df.my_new_index_col`
- 重置索引，将原本乱序的index设为数组位置下标：`df.reset_index`
- 重新索引：`df.reindex`
  
  这个方法的威力在于它通过与原索引对齐，甚至可以设置填充方法来对齐不同的表的index

  注意区别`reset_index`

  这个方法也可用于columns


### 1. 基本操作
```python
# 访问
df.loc['i1':'i5','B':'E'] # 按标签访问
df.iloc[1:2,3:4] # 按数组下标访问
df['col'] # 访问列
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
DataFrame(df, columns=, index=) # 重排行/列索引
df.index = df.new_index # 修改索引
df.columns = df.new_col # 修改列标签
df.reset_index() # 将索引重置为数组索引
df.replace(pat, repl) # 替换

# 缺失值处理
df.fillna(0) # 填充NaN项
df.isna() # 返回布尔数组，np.nan的项为True
df.isnull() # 返回布尔数组，None得项为True
df.notna() # 返回布尔数组，非np.nan的项为True
df.notnull() # 返回布尔数组，非None得项为True
```
### 2. 索引操作与数组重塑
- 本质上行/列标签都是`Index`系列的对象，接口是一致的
- 将列设置为索引/列标签：
  ```python
  df.set_index(idx_col/idx_col_list) # 设置索引
  df.set_axis(idx_row/idx_row_list,axis=1) # 设置列标签
  ```

  此法通过传入列表来设置多层索引

- 直接修改行列标签：
  ```python
  # 直接修改行/列标签
  df.index/df.columns = ...
  ```
  此法也可用于**设置多层索引**

  注意：此法不可用于索引重排，发生的是位置对应

- 索引重排：
  
  ```python
  df.reindex(idx,method=None/'ffill')
  ```

### 坑
- `df.loc`与`df.iloc`的异同
  
  同：这两个都可以通过数字下标来访问df的数据
  异：`loc`依据的是*行索引*，iloc依据的是*数组索引*。`DataFrame`的行是绑定了一个行索引（Index），它通常跟数组索引是一样的，但在时间序列分析种它也可以是日期而不是整数，在筛选出部分行view里面它不是连续的整数索引。而`iloc`则将df当作一般二维数组来处理根据位置来使用索引

- 视图、副本、切片赋值、inplace
  
  概括来说，关于pandas修改两点原则：
  1. 在试图使用赋值来修改DataFrame时，**不要使用形如`df['a']['b']`的链式索引**
  2. **不要对视图使用inplace选项**，这不会报错，但**不会正常工作**

  解释如下：

  首先，我们使用不同类型的索引取值时（包括数字索引/分片、行/列标签索引/分片、布尔索引、花式索引）时，**返回的类型有可能是视图，也有可能是拷贝**。这是因为Pandas基于Numpy的ndarray，而ndarray返回视图还是拷贝取决于其*内存布局*，因此Numpy对所有索引/切片行为没有承诺。

  但我们使用索引/切片赋值时触发的是`__setitem__`方法，可以保证其赋值行为是正确的。

  这解释了第一条，为什么不能用链式索引。
  
  考虑索引赋值语句：
  
  `df['a']['b'] = 5`

  链式赋值实际上等价于如下调用：

  `df.__getitem__('a').__setitem__('b',v)`

  若运气好，第一层的`__getitem__`返回视图，链式索引赋值将正常工作；但运气不好就会由于修改拷贝而失败；

  使用单层索引：`df['a','b'] = 5`
  
  等价于如下调用：

  `df.__setitem__(('a','b'),5)`

  保证正常工作

  同理，第二条也是由于对拷贝使用inplace无法修改原数组

  一个小总结是：
  
  结合*复杂索引*的数组修改的**唯一**正确操作是：

  非链式索引语法+索引赋值操作

- `np.nan != np.nan`，请使用notna()
- 不要使用形如`dict(sr)`的转换方式，使用`sr.to_dict`，因为`dict(sr)`得到的数据成员的类型是**Numpy自己的类型而不是标准类型**