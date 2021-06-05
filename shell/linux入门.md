

内部命令: 集成在shell中的命令
外部命令: shell外提供的命令
区分命令类型: type cmd
同名的命令执行优先级: shell命令 > app命令; type -a cmd

### linux登陆和基本操作

#### linux基础

##### 用户类型
- root用户
- 普通用户

##### ternimal
 在Linux上查看地址，cmd: hostname -I

##### 终端类型
- 控制台终端: /dev/console
- 串行终端: /dev/ttyS#
- 虚拟终端: tty, teletypewriters; /dev/tty#, tty可有n个，ctrl+alt+F# 切换
- 图形终端: startx, xwindows
    ```
    centos6: ctrl+alt+F7
    centos7: 在哪个终端启动，即位于哪个虚拟终端
    ```
- 伪终端: pty, pseudo-tty; /dev/pts/# 如 ssh远程连接

#### 查看当前终端
    ```
    # type
    /dev/pst/0
    ```
4