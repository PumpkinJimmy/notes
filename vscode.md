## VS Code 使用
基本理念：这是一个现代的Vim，通过插件可以无限拓展其功能。
相较于Vim，VSCode有GUI更好上手，方便调试。
VSCode的设置分为全局的设置和工作区的设置。工作区的设置放置在工作区的.vscode文件夹里。所有设置都写成JSON。
- settings.json 保存VSCode设置
- launch.json 保存启动程序的设置
- tasks.json 保存任务设置（*任务*就是在VSCode内执行成套命令的通用接口，可以设计来调用系统命令）

### Markdown
- Markdown Preview 插件可以预览效果，右键标签打开preview，或者快捷键Ctrl-K V 打开分屏视图