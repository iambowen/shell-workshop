
### 什么是Shell

* 脚本编程语言: 基本的流程控制、替换、循环、条件、算数运算、函数以及shell和执行命令间的双向交互。
* 命令行接口(CLI) - 与 *nix 内核交互
    * Bash
    * Sh
    * Ksh
    * Zsh

```bash
iambowen@ubuntu:~$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/dash
/bin/bash
/bin/rbash
```

![architecture](http://www.ituring.com.cn/download/01ROBenJKxIH)
<h3><center>Bash 组件、解析过程</center>


### 准备

在windows上安装下列工具：

1. [cygwin](https://www.cygwin.com/) 或者 [git bash](https://git-scm.com/downloads)
2. 确保安装`curl`, `telnet`, `jq` 等工具
3. 选择安装[vscode](https://code.visualstudio.com/)

### Hello World

```bash
echo "hello world"
```

### 原语(Primitive)

Bash中包含三种基本记号(token)：
    * 保留关键字: `if`, `while`
    * 操作符: `||`, `&` 等
    * 单词: 剩余的部分

#### literal string  vs meta string

```bash
name="bowen"
echo "my name is $name"
echo 'my name is $name'
```
|        |          |    
|--------|-------- |    
|meta字符 |    	meta字符作用|
|=	    |   设定变量|
|$	    |   作变量或运算替换(请不要与shell prompt混淆)|
|>	    |   输出重定向(重定向stdout)|
|<	    |   输入重定向(重定向stdin)|
|\	    |   	命令管道|
|&	    |   重定向file descriptor或将命令至于后台(bg)运行|
|()	    |   将其内部的命令置于nested subshell执行，或用于运算或变量替换|
|{}	    |   将期内的命令置于non-named function中执行，或用在变量替换的界定范围|
|;	    |   在前一个命令执行结束时，而忽略其返回值，继续执行下一个命令|
|&&	    |   在前一个命令执行结束时，若返回值为true，继续执行下一个命令|
| \|\| |   在前一个命令执行结束时，若返回值为false，继续执行下一个命令|
|!	|   执行histroy列表中的命令|
|...|   	...|

#### quoting

在bash中，常用的quoting有以下三种方法：

* hard quote：''(单引号)，凡在hard quote中的所有meta均被关闭；
* soft quote：""(双引号)，凡在soft quote中大部分meta都会被关闭，但某些会保留(如$);
* escape: \ (反斜杠)，只有在紧接在escape(跳脱字符)之后的单一meta才被关闭；

可用于折行，以防止一行的内容太多
```bash
 ~> echo a \ b \c
a  b c
```

```bash
awk {print $0} 1.txt   # wrong
awk '{print $0}' 1.txt   # right
```

###  参数和变量展开

* () 将command group置于sub-shell(子shell)中去执行，也称 nested sub-shell。
* {} 则是在同一个shell内完成，也称non-named command group。

__括号展开__

```bash
 ~> echo pre{one,two,three}post
preonepost pretwopost prethreepost

 ~> touch 1.txt
 ~> mv 1.{txt,md}
 ~> ls -al 1.md
-rw-r--r--  1 praise  staff  0 Jun  5 00:01 1.md
```

__算术展开__

```bash
 ~> echo $((1 + 1))
2
```

__波浪展开__

```bash
 ~> ls ~
Apps            Documents       Dropbox         Library         Music           Public          audio           github          kubectl         projects
```


### 替换
__$()__ : 命令替换

```bash
 ~> a=$(echo "hello world")
 ~> echo $a
hello world

 ~> b=`echo "hello world"`
 ~> echo $b
hello world
```

__${}__: 变量替换

```bash
 ~> a="b"
 ~> echo $a
b
 ~> echo "${a}"
b
```


#### 字符串展开

```bash
 ~> str=${str:-"hello world"}    #default value
 ~> echo $str
hello world

 ~> echo ${#str}          #length of string
11

 ~> echo ${VAR?"This variable VAR is not set"} 
-bash: VAR: This variable VAR is not set

 ~> echo ${str:0:5}  # get sub string
hello

iambowen@ubuntu:~$ echo ${var^^}  #uppercase
HELLO WORLD

iambowen@ubuntu:~$ a="HELLO"       #lowercase
iambowen@ubuntu:~$ echo ${a,,}
hello

iambowen@ubuntu:~$ echo ${var^^[aeiou]} #uppercase some of the characters
hEllO wOrld
```

### IFS(Internal Field Seperator)

shell的分隔符
* 空白键(White Space)
* 表格键(Tab)
* 回车键(Enter)

shell会依据将命令行所输入的文字给拆解为字段(word). 然后在针对特殊的字符(meta)先做处理，最后在重组整行命令。

```bash
 ~> IFS=:; for var in "a:b:c"; do echo $var ; done
a b c
```

### prompt
* `$`: 一般用户账号
* `#`: root账号

### 别名

```bash
alias gs="git status"
alias gl="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)%Creset' --abbrev-commit --date=relative"

alias ..='cd ../'
alias ...='cd ../../'
```

### 函数

```bash
hello(){                        #推荐写法
    local output="hello world"
    echo "$output"
}

function world {
    echo "hello world"
}

hello
world
```

### build in函数

```bash
 ~> type echo
echo is a shell builtin
```

### 控制流


### 循环

### 测试

### 配置 example

```bash
export PS1=
```

### history


### Style Guide Example


### 有用的几个命令

#### curl

#### telnet

#### jq

#### ssh

#### sed/awk


### 其它材料

1. [udacity shell workshop](https://classroom.udacity.com/courses/ud206)
2. [Bash Programming Guide](https://github.com/bahamas10/bash-style-guide/blob/master/README-cn.md)
3. [Architecture of Shell](http://www.aosabook.org/en/bash.html)
4. [Essential Curl](http://www.jianshu.com/p/8ede8052e555)
5. [Essential SSH](http://www.jianshu.com/p/a360279e6577)
6. [更加安全和简单的方式通过堡垒机ssh](http://www.jianshu.com/p/8a087e806b1b)
7. [Shell 13问](https://www.gitbook.com/book/wizardforcel/shell-13-questions)
8. [String manipulation](http://tldp.org/LDP/abs/html/string-manipulation.html)