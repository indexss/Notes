1. jmp x:y    xs->x, ip->y

2. jmp ax     ip->[ax]

3. Debug

   ![image-20221114102654691](/Users/shilinli/Library/Application Support/typora-user-images/image-20221114102654691.png)

编译运行

MASM FUCK.ASM	

回车 x 3

LINK FUCK.OBJ

回车 x 3

FUCK.EXE



挂载

MOUNT C ~/ASSEMBLY

```assembly
mov bx,1000
mov dx,bx
mov al,[0];move DS:[0]'s content to AL register
```

8086 don't support you to move a DATA into a SEGMENT REGISTER

of course you can reverse this process, change the step 3 like this:

```assembly
mov bx,1000
mov ds,bx
mov [0],al
```

the meaning of the process is: to move the content of AL to the memory 1000:0

 ![image-20221115182949975](/Users/shilinli/Library/Application Support/typora-user-images/image-20221115182949975.png)

radix 转化进制

assume cs:code;是把cs寄存器和代码段code联系起来，等要执行代码段的时候cs指向其首地址