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
git tag # 打标签
```
### 配置
`git config`命令用于处理配置
```bash
git config --list # 列出当前的配置
git config [--global] user.name bbpumpkin # 将user.name的值配置为bbpumpkin，--global用于指定全局设置
```
每个仓库都必须要配置`user.name,user.email`，这两个选项用于指明每一个提交的作者及其email
通常只需要在一台新机器上执行一次`git config --global`来配置这两个选项就可以自动应用到所有仓库的配置里面了
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

#### Github SSH支持
Github支持通过SSH来访问远程仓库。
- 预备：你需要一对公钥私钥
- 添加：可以通过在Github的Settings里面添加公钥
- 测试：`ssh -T git@github.com`，若返回结果显示“成功识别，但Github不支持shell服务“的提示信息说明配置成果
- 使用：在clone时直接选择SSH链接，或者修改已有仓库的`remote.origin.url`为SSH URL，仓库就可以用SSH通信了，push不用输入用户和密码

### 远程仓库
- 基于结合远程仓库以及本地库的Git的分布式机制是Git的重要特性之一（区别Subversion一类集中式仓库，必须经常下载远程仓库，且断网=停工）
- 主要使用`git remote`系列命令来执行远程相关的操作
- 实际上Git管理的单位是分支，与远程仓库交互就是与远程分支交互
- 一般从远程库clone下来的库一般命名为`origin`，但可以自定义，没有特殊含义
- 与远程分支交互存在权限问题，只有被允许的用户才可以获得相应的操作分支的权限
  - 主要的权限无非是能否拉取(`fetch`或者说`pull`)以及能否推送(`push`)，也就是读和写
  - 作为开源项目，往往只有开发者拥有推送的权限，而其他人只有拉取的权限
  - Github的私有项目对未经允许的用户即不可`fetch`也不可`push`
- 一个本地库可以有多个对应的远程仓库，每个分支相对独立，可以推送到不同的库（故push时要注意指定目标库和要推送的分支）
- 一个本地分支只能有一个*上游分支*，其意义在与本地分支将与某特定的远程分支关联同步。
  - 具体来说，Git将关注该本地分支是否*领先*（本地分支版本较上游分支新）或者*落后*（本地分支版本较上游分支旧）于对应上游分支
  - 在已经关联了上游分支的本地分支上执行push操作时不需要显式指定要推送到的远程库，缺省参数将默认推送到远程分支，pull操作也类似
  - 可以在首次执行`git push`时使用选项`-u`连指定当前推送目标为上游分支
  - 习惯上为开发中的分支总是指定一个上游分支是个好选择（保持同步）
- 在使用远程仓库协同工作时要注意从远程分支fetch到的修改可能与本地的修改*冲突*（这也会导致`git pull`失败），需要手动解决冲突。正确的工作流应该是经常使用fetch/pull来保持与上游同步