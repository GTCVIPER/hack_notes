# DosBox

## 常用指令

```cmd 
mount c d:masm  # 将 D 盘的 指定目录挂载到 C 盘

dir # 查看当前目录
```

![image-20230816151736278](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230816151736278.png)

<font size=5 color=gree>里面含有 调试 汇编程序的常用工具</font>

# Debug

是 8086 程序的调试工具，使用可以查看CPU寄存器中的内容、内存情况以及机器码级跟踪程序的运行

```cmd
debug

# 此时进入 Debug 的交互界面 
-r  # 查看当前CPU各个 寄存器的值
-r ax  # 更改 ax 寄存器里的值，执行会提示输入 ax 的值
```

![image-20230816152843738](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230816152843738.png)

```cmd
debug

# 此时进入 Debug 的交互界面 
-d  # 查看当前内存中的内容，连续的运行会输出连续的内存偏移及其对应的内容
-d 0000:0000 # [-d 段地址:偏移地址]，从指定的内存地址开始输出里面的内容
-d 0000:9 # [-d 段地址:偏移数(十六进制)]，从段地址 0000 第 9 个内容开始进行输出
-d 0000:0000 f # [-d 段地址:偏移地址 (出字符个数+1 十六进制数)], 从地址 0000:0000 开始，输出 16 个字符的内容
```

![image-20230816154303428](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230816154303428.png)

![image-20230816154343973](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230816154343973.png)

![image-20230816154438896](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230816154438896.png)

![image-20230816154859152](D:\hack_notes\汇编\assets\image-20230816154859152.png)

```cmd
debug

# 此时进入 Debug 的交互界面 
-e 0000:0000 12 23 ab 89  # -e [段地址:偏移地址] 值，值间用空格分割；给地址 0000:0000 覆盖写入以下的值 12 23 ab 89
-e 0000:0000 # 修改 0000:0000 地址的值，回车会依次定位到指定位置的值
```

![image-20230816164759361](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230816164759361.png)

![image-20230816164929186](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230816164929186.png)

```cmd
debug

# 此时进入 Debug 的交互界面 
-a CS值:IP值  # 向 执行指令的内存地址 输入汇编指令，指令地址的段地址存储在 CS 寄存器中，偏移地址存储在 IP 寄存器中，可以通过 R 命令进行查看，回车之后，可以逐个输入 汇编语句，进行后续的调试
-a # 直接在 当前 CS:IP 地址输入对应的汇编指令

-t  # t 命令，逐条语句执行 a 命令输入的  汇编语句，结合 R 命令可以查看各个寄存器的执行结果
```

```cmd
debug

# 此时进入 Debug 的交互界面 
-d CS值:IP值 # 查看 执行指令的内存地址开始存储的内容，该内容正是，之前 a指令写入汇编指令对应的机器码

-u CS值:IP值 # 将内存中的机器指令翻译成汇编指令
```

![image-20230816171250784](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230816171250784.png)

CS:IP --> `073F:0100` 

`a 073f:0100` 向内存写入 汇编语句

![image-20230818135457959](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230818135457959.png)

T 命令逐个运行 之前的汇编语句，并显示寄存器情况

![image-20230818135651242](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230818135651242.png)

`d 073f:0100` 查看内存里汇编语言对应的机器码

![image-20230818135822655](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230818135822655.png)

`u 073f:0100` 将 内存里汇编语言对应的机器码 翻译成 对应的汇编语句

![image-20230818140113525](https://raw.githubusercontent.com/GTCVIPER/pic/master/img/image-20230818140113525.png)

<font size=5 color=green>总之，Debug 工具中的 常用命令为</font> <font size=4 color=red>T，R，U，A，D</font>

