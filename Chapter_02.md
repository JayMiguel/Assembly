### 检测点2.1
**（1）写出以下每条汇编指令执行后相关寄存器中的值。**
>答：
>```bash
>mov ax,62627    AX=F4A3H
>mov ah,31H      AX=31A3H
>mov al,23H      AX=3123H
>add ax,ax       AX=6246H
>mov bx,826CH    BX=826CH
>mov cx,ax       CX=6246H
>mov ax,bx       AX=826CH
>add ax,bx       AX=04D8H
>mov al,bh       AX=0482H
>mov ah,bl       AX=6C82H
>add ah,ah       AX=D882H
>add al,6        AX=D888H
>add al,al       AX=D810H
>mov ax,cx       AX=6246H
>```

**（2）只能使用目前学过的汇编指令，最多使用4条指令，编程计算2的4次方。**
>答：
>```bash
>mov ax,2H       AX=2H
>add ax,ax       AX=4H
>add ax,ax       AX=8H
>add ax,ax       AX=10H
>```

### 检测点2.2
**（1）给定段地址为0001H，仅通过变化偏移地址寻址，CPU的寻址范围为<u>0010H</u>到<u>1000FH</u>。**

**（2）有一数据存放在内存20000H单元中，现给定段地址为SA，若想用偏移地址寻到此单元，则SA应满足的条件是：最小为<u>1001H</u>，最大为：<u>2000H</u>**

### 检测点2.3
**问：下面的3条指令执行后，CPU几次修改IP？都是在什么时候？最后IP中的值是多少？**
```bash
mov ax,bx
sub ax,ax
jmp ax
```
>答：<br/>
>CPU四次修改IP；<br/>
>第一次修改，输入输出控制电路将mov ax,bx对应的机器指令送入指令缓冲器之后，IP中的值自动增加；<br/>
>第二次修改，输入输出控制电路将sub ax,bx对应的机器指令送入指令缓冲器之后，IP中的值自动增加；<br/>
>第三次修改，输入输出控制电路将jmp ax对应的机器指令送入指令缓冲器之后，IP中的值自动增加；<br/>
>第四次修改，执行控制器执行第3条汇编指令对应的机器指令后，指令指针寄存器IP中的值改为寄存器ax中的值；<br/>
>最后IP中的值是0000H。<br/>

### 实验任务
**（1）使用Debug将下面的程序段写入内存，逐条执行，观察每条指令执行后，CPU中相关寄存器中内容的变化。**
```bash
机器码          汇编指令
b8 20 4e        mov ax,4E20H
05 16 14        add ax,1416H
bb 00 20        mov bx,2000H
01 d8           add ax,bx
89 c3           mov bx,ax
01 d8           add ax,bx
b8 1a 00        mov ax,001AH
bb 26 00        mov bx,0026H
00 d8           add al,bl
00 dc           add ah,bl
00 c7           add bh,al
b4 00           mov ah,0
00 d8           add al,bl
04 9c           add al,9CH
```
**提示：可用E命令和A命令以两种方式将指令写入内存。注意用T命令执行时，CS:IP的指向。**<br/>
>答：
>```bash
>>debug
>
>-rcs
>CS 073F
>:1000
>
>-rip
>IP 0100
>:0000
>
>-a 1000:0
>1000:0000 mov ax,4E20
>1000:0003 add ax,1416
>1000:0006 mov bx,2000
>1000:0009 add ax,bx
>1000:000B mov bx,ax
>1000:000D add ax,bx
>1000:000F mov ax,001A
>1000:0012 mov bx,0026
>1000:0015 add al,bl
>1000:0017 add ah,bl
>1000:0019 add bh,al
>1000:001B mov ah,0
>1000:001D add al,bl
>1000:001F add al,9C
>1000:0021
>
>-t
>AX=4E20 BX=0000 CX=0000 DX=0000 SP=00FD BP=0000 SI=0000 DI=0000
>DS=073F ES=073F SS=073F CS=1000 IP=0003  NV UP EI PL NZ NA PO NC
>1000:0003 051614      ADD    AX,1416
>......不断重复t指令......
>```

**(2)将下面3条指令写入从2000:0开始的内存单元中，利用这3条指令计算2的8次方。**
```bash
mov ax,1
add ax,ax
jmp 2000:0003
```
>答：
>```bash
>>debug
>
>-a 2000:0
>2000:0000 mov ax,1
>2000:0003 add ax,ax
>2000:0005 jmp 2000:0003
>2000:0007
>
>-rcs
>CS 073F
>:2000
>
>-rip
>IP 0100
>:0
>```

**（3）查看内存中的内容**<br/>
>答：
>```bash
>>debug
>-d
>073F:0100 00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00 ................
>073F:0110 00 00 00 00 00 00 00 00-00 00 00 00 34 00 2E 07 ............4...
>073F:0120 00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00 ................
>073F:0130 00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00 ................
>073F:0140 00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00 ................
>073F:0150 00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00 ................
>073F:0160 00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00 ................
>073F:0170 00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00 ................
>```