## Pandas
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