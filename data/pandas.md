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