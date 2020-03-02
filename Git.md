## Git笔记

### Quick Start
```bash
git init # 初始化仓库
git config # 配置

git add # 追踪文件/添加更改
git reset HEAD -- file # 撤回修改
git commit # 提交
git checkout # 回退历史版本

git checkout # 切换分支
git checkout -b # 新建分支
git branch # 查看分支
git merge # 合并分支

git clone # 克隆远程仓库
git remote # 列出远程仓库
git remote add # 添加远程仓库
git push # 推送
git fetch # 从远程仓库拉取
git pull # 相当于git fetch + git merge
```
### 子模块（Submodule）功能
子模块功能旨在在一个Git仓库里面引用另一个Git仓库，且便于Git版本控制。指令主要是`git submodule`系列。
- 添加的Git仓库一定要是远程仓库，并拥有一个URL
- 添加后的配置信息存放在.gitmodules文件里面。由于之后访问子模块都是通过第一次添加配置的Url，所以必须保证Url长期有效
- 进入了submodule所在文件夹即可将其当作一般仓库来操作，包括修改、提交、Push等。在操作子模块的代码上，submodule仓库跟原始的仓库是平等的，都可以向远程库推送修改。

### Github
优秀的Git远程仓库，除了网速慢其他都好

#### Pull Request
Github优秀的软件开发协作机制。
> Pull Request 是一种通知机制。你修改了他人的代码，将你的修改通知原来的作者，希望他合并你的修改，这就是 Pull Request。
对于贡献者众多的大型开源项目可以有如下工作流：
- 用户把项目fork到自己的名下，fork出来的仓库（分支）独立于原仓库，相当于复制了一份原来的仓库。
- 在自己的fork下面提交了修改代码后，可以提交Pull Request
相比于将修改直接merge到分支，Pull Request让代码的维护者可以进行Reiew，然后决定是否合并这一修改