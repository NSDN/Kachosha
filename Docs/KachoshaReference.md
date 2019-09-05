# Kachosha 标准手册

[toc]

## 1 数据类型

字节序全部使用小端序。
| 类型 | 大小（字节） | 语义 |
| --- | --- | --- |
| i32 | 4 | 4字节有符号整数 |
| u32 | 4 | 4字节无符号整数 |
| f32 | 4 | 32位浮点数  |

## 2 字节码

## 3 汇编码
```bnf
SourceCode ::= 
    [ModuleDeclaration]
    {ImportDeclaration}
    {Function}
    
ModuleDeclaration ::=
   "module" <ModuleName> <NewLine>

ImportDeclaration ::=
    "import" <ModuleName> <NewLine>
    
ModuleName ::= String
FunctionName ::= String

Function ::= 
    "." <FunctionName> ["export"] <NewLine>
    { <InstructionLine> <NewLine> }
    
InstructionLine ::=
	<Instruction> | (<Instruction> " " <Argument> { "," <Argument>})
    
Comment ::= ';' <String> <NewLine>
    
```

例子：
```
module Hello	;这个模块叫Hello
import Print	;导入Print模块

.main
push_string "HelloWorld"	;压入HelloWorld（作为两个u32表示的常量区字符串），这是一个语法糖
calle Print,print	;调用Print模块的print函数
ret	;返回

.some export	;定义一个被到导出的函数
ret

.wmain
push_string L"HelloWorld"
calle Print.wprint
ret
```

## 4 VM标准
### 4.1 栈结构与本地变量区
### 4.2 程序计数器
### 4.3 持久变量区
### 4.4 函数调用约定

## 5 指令集

| 指令 | 汇编码 | 参数 | 语义 |
| --- | --- | --- | --- |
| 00 | nop | | 不操作 |

| 01 | jmp | 目标地址(u32) | 无条件跳转到目标地址 |
| 02 | jt | 目标地址(u32) | 当栈顶一个u8不为0时跳转到目标地址 |
| 03 | call | | 调用栈顶地址指向的函数并弹出栈顶 |
| 04 | callc | C函数ID(u32) | 调用一个C函数 |
| 05 | calle | 外部模块ID(u32),外部模块中的函数ID(u32) | 调用一个外部函数 |
| 06 | yield | | 在此中断 |
| 07 | fail | | 异常终止 |
| 0F | ret | | 返回 |

| 0A | and | | 两个u32类型求布尔“且”|
| 0B | or | | 两个u32类型求布尔“或”|
| 0C | xor | | 两个u32类型求布尔“异或”|
| 0D | not | | 一个u32类型求“非”|

| 10 | push32 | 值(i32/u32/f32) | 压入一个32位值 |
| 11 | pushc | 常量区地址(u32),大小(u32) | 压入一些常量区中的字节 |
| 12 | pushl | 本地变量区地址(u32),大小(u32) | 压入一些本地变量区的字节 |
| 13 | popl | 本地变量区地址(u32),大小(u32) | 弹出一些字节到本地变量区指定区域 |
| 14 | pushs | 持久变量区地址(u32),大小(u32) | 压入一些持久变量区的地址 |
| 15 | pops | 持久变量区地址(u32),大小(u32) | 弹出一些字节到持久变量区 |

| 18 | localAlloc | 大小(u32) | 分配新的本地变量区 |
| 19 | localFree | | 弹出本地变量区 |

| 20 | addi32 | | 两个i32相加 |
| 21 | subi32 | | 两个i32相减 |
| 22 | muli32 | | 两个i32相乘 |
| 23 | divi32 | | 两个i32相除 |
| 24 | modi32 | | 两个i32求余 |
| 25 | negi32 | | 顶部i32取反 |
| 26 | gi32 | | 两个i32求大于比较 |
| 27 | gei32 | | 两个i32求大于等于比较 |
| 28 | si32 | | 两个i32求小于比较 |
| 29 | sei32 | | 两个i32求小于等于比较 |
| 2A | ei32 | | 两个i32进行等于比较 |

| 30 | addu32 | | 两个u32相加 |
| 31 | subu32 | | 两个u32相减 |
| 32 | mulu32 | | 两个iu2相乘 |
| 33 | divu32 | | 两个u32相除 |
| 34 | modu32 | | 两个u32求余 |
| 35 | negu32 | | 顶部u32取反 |
| 36 | gu32 | | 两个u32求大于比较 |
| 37 | geu32 | | 两个u32求大于等于比较 |
| 38 | su32 | | 两个u32求小于比较 |
| 39 | seu32 | | 两个u32求小于等于比较 |
| 3A | eu32 | | 两个u32进行等于比较 |

| 40 | addf32 | | 两个u32相加 |
| 41 | subf32 | | 两个u32相减 |
| 42 | mulf32 | | 两个iu2相乘 |
| 43 | divf32 | | 两个u32相除 |
| 44 | modf32 | | 两个u32求余 |
| 45 | negf32 | | 顶部u32取反 |
| 46 | gf32 | | 两个u32求大于比较 |
| 47 | gef32 | | 两个u32求大于等于比较 |
| 48 | sf32 | | 两个u32求小于比较 |
| 49 | sef32 | | 两个u32求小于等于比较 |
| 4A | ef32 | | 两个u32进行等于比较 |



