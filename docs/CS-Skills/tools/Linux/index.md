# Linux


## 基本命令

- `ctrl c`: 取消命令，并且换行
- `ctrl u`: 清空本行命令
- `cp XXX YYY`: 将XXX文件复制成YYY，XXX和YYY可以是一个路径
- `mkdir XXX`: 创建目录XXX
- `rm XXX`: 删除普通文件
- `rm XXX -r`: 删除文件夹
- `mv XXX YYY`: 将XXX文件移动到YYY，和cp命令一样，XXX和YYY可以是一个路径；**重命名**也是用这个命令
- `touch`、`cat`


## tmux

tmux中复制/粘贴文本的通用方式：
1. 按下Ctrl + a后松开手指，然后按[
2. 用鼠标选中文本，被选中的文本会被自动复制到tmux的剪贴板
3. 按下Ctrl + a后松开手指，然后按]，会将剪贴板中的内容粘贴到光标处



## vim

- `n<Space>`：n表示数字，按下数字后再按空格，光标会向右移动这一行的n个字符
- `0 或 功能键[Home]`：光标移动到本行开头
- `$ 或 功能键[End]`：光标移动到本行末尾
- `G`：光标移动到最后一行
- `:n 或 nG`：n为数字，光标移动到第n行
- `gg`：光标移动到第一行，相当于1G
- `n<Enter>`：n为数字，光标向下移动n行
- `/word`：向光标之下寻找第一个值为word的字符串。
- `?word`：向光标之上寻找第一个值为word的字符串。
- `n`：重复前一个查找操作
- `N`：反向重复前一个查找操作
- `:n1,n2s/word1/word2/g`：n1与n2为数字，在第n1行与n2行之间寻找word1这个字符串，并将该字符串替换为word2
- `:1,$s/word1/word2/g`：将全文的word1替换为word2
- `:1,$s/word1/word2/gc`：将全文的word1替换为word2，且在替换前要求用户确认。
- `v`：选中文本
- `d`：删除选中的文本
- `dd`: 删除当前行
- `y`：复制选中的文本
- `yy`: 复制当前行
- `p`: 将复制的数据在光标的下一行/下一个位置粘贴
- `u`：撤销
- `Ctrl + r`：取消撤销
- `大于号 >`：将选中的文本整体向右缩进一次
- `小于号 <`：将选中的文本整体向左缩进一次
- `:set paste` 设置成粘贴模式，取消代码自动缩进
- `:set nopaste` 取消粘贴模式，开启代码自动缩进
- `:set nu` 显示行号
- `:set nonu` 隐藏行号
- `gg=G`：将全文代码格式化
- `:noh` 关闭查找关键词高亮
- `Ctrl + q`：当vim卡死时，可以取消当前正在执行的命令


## ssh


### 基本用法

远程登录服务器：

- `ssh user@hostname`
user: 用户名
hostname: IP地址或域名
- 默认登录端口号为22。如果想登录某一特定端口：
```  
ssh user@hostname -p 端口号
```

### 配置文件
创建文件 `~/.ssh/config`

然后在文件中输入：
```
Host myserver1
    HostName IP地址或域名
    User 用户名

Host myserver2
    HostName IP地址或域名
    User 用户名
```
之后再次使用服务器时，可以直接使用别名myserver1、myserver2。



### 密钥登录

创建密钥：
```
ssh-keygen
```
执行结束后，`~/.ssh/`目录下会多两个文件：
id_rsa：私钥
id_rsa.pub：公钥
之后想免密码登录哪个服务器，就将公钥传给哪个服务器即可。

例如，想免密登录myserver服务器。则将公钥中的内容，复制到myserver中的
`~/.ssh/authorized_keys`文件里即可。

也可以使用如下命令一键添加公钥：
```
ssh-copy-id myserver
```

## 常用命令

### 系统状况
- `top`：查看所有进程的信息（Linux的任务管理器）
    - 打开后，输入M：按使用内存排序
    - 打开后，输入P：按使用CPU排序
    - 打开后，输入q：退出
- `df -h`：查看硬盘使用情况
- `free -h`：查看内存使用情况
- `du -sh`：查看当前目录占用的硬盘空间
- `ps aux`：查看所有进程
- `kill -9 pid`：杀死编号为pid的进程
    - 传递某个具体的信号：kill -s SIGTERM pid
- `netstat -nt`：查看所有网络连接
- `w`：列出当前登陆的用户
- `ping www.baidu.com`：检查是否连网


### 文件权限
- `chmod`：修改文件权限
  - `chmod +x xxx`：给xxx添加可执行权限
  - `chmod -x xxx`：去掉xxx的可执行权限`
  - `chmod 777 xxx`：将xxx的权限改成777`
  - `chmod 777 xxx -R`：递归修改整个文件夹的权限


### 文件检索
- `find /path/to/directory/ -name '*.py'`：搜索某个文件路径下的所有*.py文件
- `grep xxx`：从stdin中读入若干行数据，如果某行中包含xxx，则输出该行；否则忽略该行。
- `wc`：统计行数、单词数、字节数；既可以从stdin中直接读入内容；也可以在命令行参数中传入文件名列表；
  - `wc -l`：统计行数
  - `wc -w`：统计单词数
  - `wc -c`：统计字节数
- `tree`：展示当前目录的文件结构
  - `tree /path/to/directory/`：展示某个目录的文件结构
  - `tree -a`：展示隐藏文件
- `ag xxx`：搜索当前目录下的所有文件，检索xxx字符串
- `cut`：分割一行内容
    - 从`stdin`中读入多行数据
    - `echo $PATH | cut -d ':' -f 3,5`：输出PATH用:分割后第3、5列数据
    - `echo $PATH | cut -d ':' -f 3-5`：输出PATH用:分割后第3-5列数据
    - `echo $PATH | cut -c 3,5`：输出PATH的第3、5个字符
    - `echo $PATH | cut -c 3-5`：输出PATH的第3-5个字符
- `sort`：将每行内容按字典序排序
    - 可以从stdin中读取多行数据
    - 可以从命令行参数中读取文件名列表
- `xargs`：将stdin中的数据用空格或回车分割成命令行参数
- `find . -name '*.py' | xargs cat | wc -l`：统计当前目录下所有python文件的总行数

### 查看文件内容
- `more`：浏览文件内容
    - 回车：下一行
    - 空格：下一页
    - `b`：上一页
    - `q`：退出
- `less`：与`more`类似，功能更全
    - 回车：下一行
    - `y`：上一行
    - `Page Down`：下一页
    - `Page Up`：上一页
    - `q`：退出
- `head -3 xxx`：展示xxx的前3行内容
    - 同时支持从stdin读入内容
- `tail -3 xxx`：展示xxx末尾3行内容
    - 同时支持从stdin读入内容

### 用户相关
- `history`：展示当前用户的历史操作。内容存放在`~/.bash_history`中

### 工具
- `md5sum`：计算md5哈希值
    - 可以从`stdin`读入内容
    - 也可以在命令行参数中传入文件名列表；
- `time command`：统计command命令的执行时间
- `ipython3`：交互式python3环境。可以当做计算器，或者批量管理文件。
    - `! echo "Hello World"`：!表示执行shell脚本
- `watch -n 0.1 command`：每0.1秒执行一次command命令
- `tar`：压缩文件
    - `tar -zcvf xxx.tar.gz /path/to/file/*`：压缩
    - `tar -zxvf xxx.tar.gz`：解压缩
- `diff xxx yyy`：查找文件xxx与yyy的不同点

### 安装软件
- `sudo command`：以root身份执行`command`命令
- `apt-get install xxx`：安装软件
- `pip install xxx --user --upgrade`：安装python包


## Tips
**复制文本**
    windows/Linux下：`Ctrl + insert`，Mac下：`command + c`
**粘贴文本**
    windows/Linux下：`Shift + insert`，Mac下：`command + v`