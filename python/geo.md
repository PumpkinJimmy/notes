## Python地理工具链

### osmnx
```python
import osmnx as ox
ox.geocode_to_gdf('Guangzhou, China') # 搜索获取地点信息
```

### geopandas
```python
gdf.read_file('my.geojson') # 载入地图数据
gdf.plot() # 画地图
gdf.plot(column='data') # 画地图，根据数据上色
```