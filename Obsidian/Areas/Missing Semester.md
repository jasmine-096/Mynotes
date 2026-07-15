---
created: 2026-03-29T16:24
updated: 2026-05-24T17:07
---
## 第一周： Introduction to the Shell
shebang（#!) 是Linux脚本的第一行声明，目的是告诉系统此文件用哪个解释器执行
若没有shebang则需要手动指定解释器:
`python3 test.py OR bash test.sh`

`[`命令([test命令](https://www.linuxcool.com/test)的别名) 是专用于判断并返回`0(true) | 1(false)` 
eg:`[$a -eq 1]` 用来判断变量a是否等于1

[grep命令](https://www.linuxcool.com/grep)  - 按行查找文件内容 

[sort](https://www.linuxcool.com/sort) 排序命令

[sed](https://www.linuxcool.com/sed)替换命令,`sed 's/old name/new name/g'` +文件名  命令会输出数据到终端而不是文件，加`-i` 来保存到文件

[awk](https://www.linuxcool.com/sed)命令 – 对文本和数据进行处理的编程语言

[find](https://www.linuxcool.com/find) 查找文件名

正则表达式:
![[1710705827903_1.png]]

CSV(逗号分隔值)文件,纯文本表格文件

转义字符:
![[1773461416255_2.jpg]]

## 第二周

[touch](https://www.linuxcool.com/touch) 命令用来创建文件，对已存在文件使用则更新修改时间

[通配符](https://www.linuxcool.com/linux%e9%80%9a%e9%85%8d%e7%ac%a6%e6%80%8e%e4%b9%88%e7%94%a8%ef%bc%9f%e6%98%9f%e5%8f%b7%e9%97%ae%e5%8f%b7%e4%b8%80%e6%8b%9b%e6%89%b9%e9%87%8f%e6%93%8d%e4%bd%9c%e6%96%87%e4%bb%b6) `*`号可以用来匹配任意字符，`?`用来匹配单字符
eg:`ls **/*.py` 会列出当前层及下层py文件

[重定向](https://www.linuxcool.com/linux%e9%87%8d%e5%ae%9a%e5%90%91%e5%88%b0%e6%96%87%e4%bb%b6%ef%bc%9a%e5%bc%ba%e5%a4%a7%e5%8a%9f%e8%83%bd%e5%8f%8a%e6%a0%87%e5%87%86%e8%be%93%e5%87%ba%e9%87%8d%e5%ae%9a%e5%90%91%e7%a4%ba%e4%be%8b) 用>可以覆写文件，>>为追加

[export](https://www.linuxcool.com/export)命令 – 将变量提升成环境变量  


退出的快捷键:

| 快捷键        | 发送信号           | 作用说明                       | 使用场景                                 |
| ---------- | -------------- | -------------------------- | ------------------------------------ |
| `Ctrl + C` | `SIGINT` (2)   | **中断信号**，请求程序终止（程序可捕获并忽略）  | 停止卡住 / 运行中的程序（比如 `ping`、`top`）       |
| `Ctrl + Z` | `SIGTSTP` (20) | **暂停信号**，把程序放到后台暂停运行       | 临时暂停当前任务，回到终端干别的事                    |
| `Ctrl + \` | `SIGQUIT` (3)  | **退出信号**，强制终止程序并生成核心转储文件   | 程序完全卡住、`Ctrl+C` 无效时，强制退出             |
| `Ctrl + D` | 无信号            | 发送 `EOF`（文件结束符），表示输入流结束    | 退出交互式程序（如 `python`、`cat`）或退出当前 Shell |
| `Ctrl + S` | -              | **暂停终端输出**，冻结屏幕显示          | 输出刷屏太快时，临时暂停看内容                      |
| `Ctrl + Q` | -              | **恢复终端输出**，解除 `Ctrl+S` 的暂停 | 恢复被冻结的终端输出                           |
