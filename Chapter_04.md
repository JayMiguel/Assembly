### 实验3
**（1）将下面的程序保存为t1.asm文件，将其生成可执行文件t1.exe:**
>答：
>```bash
># t1.asm
>assume cs:codesg
>codesg segment
>    mov ax,2000H
>    mov ss,ax
>    mov sp,0
>    add sp,4
>    pop ax
>    pop bx
>    push ax
>    push bx
>    pop ax
>    pop bx
>    mov ax,4c00H
>    int 21h
>codesg ends
>end
>
># 编译、连接
>>masm t1;
>Microsoft (R) Macro Assembler Version 5.10
>Copyright (C) Microsoft Corp 1981, 1988. All rights reserved.
>
>  50168 + 463237 Bytes symbol space free
>      0 Warning Errors
>      0 Servere Errors
>
>>link t1;
>Microsoft (R) Overlay Linker  Version 3.64
>Copyright (C) Microsoft Corp 1983-1988.  All rights reserved.
>
>LINK : warning L4021: no stack segment
>```

**（2）用Debug跟踪t1.exe的执行过程，写出每一步执行后，相关寄存器中的内容和栈顶的内容。**
>答：
>```bash
>>debug t1.exe
>-r
>AX=FFFF BX=0000 CX=0016 DX=0000 SP=0000 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=0769 CS=076A IP=0000 NV UP EI PL NZ NA PO NC
>076A:0000 B80020   MOV AX,2000
>-t
>
>AX=2000 BX=0000 CX=0016 DX=0000 SP=0000 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=0769 CS=076A IP=0003 NV UP EI PL NZ NA PO NC
>076A:0003 8ED0     MOV SS,AX
>-t
>
>AX=2000 BX=0000 CX=0016 DX=0000 SP=0000 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=2000 CS=076A IP=0008 NV UP EI PL NZ NA PO NC
>076A:0008 83C404   ADD SP,+04      # 栈顶内容为0000
>-t
>
>AX=2000 BX=0000 CX=0016 DX=0000 SP=0004 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=2000 CS=076A IP=000B NV UP EI PL NZ NA PO NC
>076A:000B 58       POP AX          # 栈顶内容为0000
>-t
>
>AX=0000 BX=0000 CX=0016 DX=0000 SP=0006 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=2000 CS=076A IP=000C NV UP EI PL NZ NA PO NC
>076A:000C 5B       POP BX          # 栈顶内容为0000
>-t
>
>AX=0000 BX=0000 CX=0016 DX=0000 SP=0008 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=2000 CS=076A IP=000D NV UP EI PL NZ NA PO NC
>076A:000D 50       PUSH AX          # 栈顶内容为0000
>-t
>
>AX=0000 BX=0000 CX=0016 DX=0000 SP=0006 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=2000 CS=076A IP=000E NV UP EI PL NZ NA PO NC
>076A:000E 53       PUSH BX          # 栈顶内容为0000
>-t
>
>AX=0000 BX=0000 CX=0016 DX=0000 SP=0004 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=2000 CS=076A IP=000F NV UP EI PL NZ NA PO NC
>076A:000F 58       POP AX          # 栈顶内容为0000
>-t
>
>AX=0000 BX=0000 CX=0016 DX=0000 SP=0006 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=2000 CS=076A IP=0010 NV UP EI PL NZ NA PO NC
>076A:0010 5B       POP BX          # 栈顶内容为0000
>-t
>
>AX=0000 BX=0000 CX=0016 DX=0000 SP=0008 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=2000 CS=076A IP=0011 NV UP EI PL NZ NA PO NC
>076A:0011 B8004C   PUSH AX,4C00    # 栈顶内容为0000
>-t
>
>AX=4C00 BX=0000 CX=0016 DX=0000 SP=0008 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=2000 CS=076A IP=0014 NV UP EI PL NZ NA PO NC
>076A:0014 CD21     INT 21          # 栈顶内容为0000
>-p
>
>Program terminated normally

**（3）PSP的头两个字节是CD20，用Debug加载t1.exe，查看PSP的内容。**
>答：
>```bash
>>debug t1.exe
>-r
>AX=FFFF BX=0000 CX=0016 DX=0000 SP=0000 BP=0000 SI=0000 DI=0000
>DS=075A ES=075A SS=0769 CS=076A IP=0000 NV UP EI PL NZ NA PO NC
>076A:0000 B80020   MOV AX,2000     # 可知PSP的段地址是075A
>-d 075A:0000   #可以看到PSP的头两个字节确实是CD20
>075A:0000 CD 20 FF 9F 00 EA FF FF-AD DE 4F 03 A3 01 8A 03
>.........以下内容省略.........