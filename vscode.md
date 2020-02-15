## VS Code 使用
基本理念：这是一个现代的Vim，通过插件可以无限拓展其功能。
相较于Vim，VSCode有GUI更好上手，方便调试。
VSCode的设置分为全局的设置和工作区的设置。工作区的设置放置在工作区的.vscode文件夹里。所有设置都写成JSON。
- settings.json 保存VSCode设置
- launch.json 保存启动程序的设置
- tasks.json 保存任务设置（*任务*就是在VSCode内执行成套命令的通用接口，可以设计来调用系统命令）

### Markdown
- Markdown Preview 插件可以预览效果，右键标签打开preview，或者快捷键Ctrl-K V 打开分屏视图

### Tasks
动机：在VS Code内调用外部命令行工具

理念：在.vscode/tasks.json中以JSON格式指明运行的命令等参数

使用：
- Ctrl+Shift+P打开命令面板，输入tasks找到Run Tasks运行命令
- 常用：
  - `label`指明任务名称
  - `command`指明命令
  - `args`指明命令行参数
  - `options`指明环境变量、工作目录等
  - `problemMatcher`指明问题匹配器，这个要指定现成的，否则要自定义
  - `dependOn` 指明前置要调用的Task，用这个可以组合多个Tasks用于构建工具链，详见文档
- 可以使用`${}`使用预定义的变量，常用的有`${file}`，详见文档
- Tasks虽然功能是有限的，但适用面非常广泛（调用外部命令），可以用于build, test, runserver, clear, 调用make等