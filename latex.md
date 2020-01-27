## Latex笔记
### 概念
- Tex
  一个排版系统，相当于内核，只提供原始的命令，但可以通过组装命令来提供拓展功能
- Tex格式
  理解为一种书写文档的语法规则吧，原始Tex配套一个PlainTex的Tex格式
- Latex
  世界上最流行的Tex格式
- Tex套装
  包含核心部分和一些宏包
- TexLive
  流行的跨平台Tex套装
### 安装
直接装TexLive，Unbuntu下直接apt install texlive
中文支持：apt install texlive-lang-chinese
### 基本使用
- latex src.tex 使用latex，输出div
- xelatex src.tex 使用latex，输出pdf
### 注意事项
- latex里面不要随便加空行，因为空行用于分割段落！
