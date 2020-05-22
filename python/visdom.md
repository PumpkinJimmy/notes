## visdom可视化工具
### 为什么需要visdom
- visdom的功能其实跟matplotlib类似。如果在交互模式下写PyTorch，那不需要visdom，jupyter notebook的可视化功能足够强大，可以内嵌图像、图表等；
- 但当程序运行在一般Python中时，若想要实时更新图表来呈现动态信息时就需要visdom
- visdom轻量、易上手、灵活，多功能面板的形式使得其得以成为AI训练进程的全能监控面板
- visdom可以持久化，程序运行结束了面板内容还留来页面上
### Quick Start
visdom是一个web服务，使用时要先启动服务器：
```bash
python -m visdom.server
```
然后编写代码
```python
vis = visdom.Visdom(env="test1") # 指定环境名，相当于一个独立的上下文
vis.line(X=x, Y=y, win='plot') # 相当于plt.plot
vis.images(imgs, win='images') # 常用，批量显示图片
```