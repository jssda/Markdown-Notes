[toc]

# 1. Shell含义

首先，Shell 是一个程序，提供一个与用户对话的环境。这个环境只有一个命令提示符，让用户从键盘输入命令，所以又称为命令行环境（commandline，简写为 CLI）。Shell 接收到用户输入的命令，将命令送入操作系统执行，并将结果返回给用户。Shell 指的就是命令行环境。

其次，Shell 是一个命令解释器，解释用户输入的命令。它支持变量、条件判断、循环操作等语法，所以用户可以用 Shell 命令写出各种小程序，又称为脚本（script）。这些脚本都通过 Shell 的解释执行，而不通过编译。

最后，Shell 是一个工具箱，提供了各种小工具，供用户方便地使用操作系统的功能。

# 2. Shell种类

- Bourne Shell（sh）
- Bourne Again shell（bash）
- C Shell（csh）
- TENEX C Shell（tcsh）
- Korn shell（ksh）
- Z Shell（zsh）
- Friendly Interactive Shell（fish）

下面的命令可以查看当前运行的 Shell

```
$ echo $SHELL
/bin/bash
```

下面的命令可以查看当前的 Linux 系统安装的所有 Shell。

```
$ cat /etc/shells
```

上面两个命令中，`$`是命令行环境的提示符，用户只需要输入提示符后面的内容。

Linux 允许每个用户使用不同的 Shell，用户的默认 Shell 一般都是 Bash，或者与 Bash 兼容。

# 3. echo命令

`echo`命令的作用是在屏幕输出一行文本，可以将该命令的参数原样输出。如果想要输出的是多行文本，即包括换行符。这时需要把多行文本放在引号里面。

```
$ echo "<HTML>
    <HEAD>
          <TITLE>Page Title</TITLE>
    </HEAD>
    <BODY>
          Page body.
    </BODY>
</HTML>"
```

## 3.1 `-n`参数

默认情况下，`echo`输出的文本末尾会有一个回车符。`-n`参数可以取消末尾的回车符，使得下一个提示符紧跟在输出内容的后面。

```
$ echo -n hello world
hello world$
```

上面例子中，`world`后面直接就是下一行的提示符`$`。

```
$ echo a;echo b
a
b

$ echo -n a;echo b
ab
```