# gnu 嵌入汇编

## stos
  串存储指令，英文缩写 store string ， 将 eax 中的数据传送到目的地址(目的地址默认为 es:[edi])，以下两条指令相当于一条 stos 指令
```
mov [edi], eax
add edi, 4          // 或者 sub edi, 4 
```
至于到底是加 4 还是减 4 ，是由方向标志 DF 来决定，可以由指令 cld 和 std 指令设置
cld： 从低地址往高地址传送
std： 从高地址往低地址传送

## rep
重复前缀指令，英文缩写 repeat，每执行一次， ecx 减 1，直到 ecx 减至0，重复执行结束，以下代码模拟 rep
```
AGAIN:  stos        // mov [edi], eax
                    // add edi, 4
        dec ecx
        jnz AGAIN
```

```c
if(AddressSize == 16) Counter = &CX; //use CX
else Counter = &ECX; //AddressSize == 32, use ECX

while(Counter != 0) {
    ServiceInterrupts(); //service pending interrupts (if any)
    ExecuteStringInstruction(); //execute associated string instruction
    Counter = Counter - 1;
    if(((Instruction == REPZ || Instruction == REPE) && ZF == 0) || ((Instruction == REPNZ || Instruction == REPNE) && ZF == 1) break;
}
```
## repne scasb

扫描字符串

```asm
cld
repne scasb
```
等同于：
```asm
scans:
    scasb       // cmp byte [edi],al
                // inc edi
    dec ecx
    jnz scans
    je loopdone 
loopdone:
```
可见，指令结束后，edi 指向最后一个不相等的位置的下一个位置。

示例：
```c
extern inline int strlen(const char * s)
{
register int __res __asm__('cx');   // __res 关联寄存器 cx
__asm__("cld\n\t"                   // DF=0，所以scasb执行递增操作，从前到后扫描
    "repne\n\t"
    "scasb\n\t"
    "notl %0\n\t"                   // 取反得到总长度     
    "decl %0"                       // 去掉末尾 0 
    :"=c"(__res)                    // %0 寄存器 cx
    :"D"(s),"a"(0),"0"(0xffffffff):"di");      
    // __res 作为输出
    // edi 关联 s
    // eax 初始化为 0 （字符串结束符）
    // "0"(0xffffffff) 表示寄存器0，此时为 cx（__res）初始化为0xffffffff 
return __res;
```

```asm
 .file "hello.c"
 .text                                      

.globl strlen                           
 .type strlen, @function
strlen:                                  
 pushl %ebp                        
 movl %esp, %ebp            

 pushl %edi                      
 movl 8(%ebp), %edi       

 movl $0, %eax
 movl $-1, %ecx
#APP
 cld
 repne
 scasb
 notl %ecx
 decl %ecx
#NO_APP
 movl %ecx, %eax
 popl %edi
 popl %ebp
 ret
 .size strlen, .-strlen
 .ident "GCC: (GNU) 4.1.3 20070929 (prerelease) (Ubuntu 4.1.2-16ubuntu2)"
 .section .note.GNU-stack,"",@progbits
```

## 通用约束 0-9

  指令中的输出操作数对应的C语言变量应该具有左值类型，当然对于输出操作数没有这种左值限制。输出操作数必须是只写的，也就是说，asm对取出某个操作数，执行一定计算以后再将结果存回该操作数这种类型的汇编指令的支持不是直接的，而必须通过特定的格式的说明。  

  如果汇编指令中包含了一个输入-输出类型的操作数，那么在模板中必须用两个占位符对该操作数的不同功能进行引用：一个负责输入，另一个负责输出。例如：  

  `asm ("addl %2,%0":"=r"(foo):"0"(foo),"g"(bar));`

  在上面这条指令中，
"%0”是一个输入-输出类型的操作数，
"=r"(foo)用于限定其输出功能，该指令的输出结果会存放到C语言变量foo中；
  指令中没有显式的出现“%1”操作数，但是针对它有一个限定字符串"0"(foo)，事实上指令中隐式的“%1”操作数用于描述“%0”操作数的输入功能，它的限定字符串中的"0"限定了“%1”操作数与“%0” 
  具有相同的地址。可以这样理解上述指令中的模板：该指令将“%1”和“%2”中的值相加，计算结果存放回“%0”中，指令中的“%1”与“%0”具有相同的地址。  
  
  *注意，用于描述“%1”的"0"限定字符足以保证“%1”与“%0”具有相同的地址。*

  `foo += bar;`
  
