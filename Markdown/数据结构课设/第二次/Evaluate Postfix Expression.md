# 6-2 Evaluate Postfix Expression (25分)

Write a program to evaluate a postfix expression. You only have to handle four kinds of operators: +, -, x, and /.

## Format of functions:

```c++
ElementType EvalPostfix( char *expr );
```

where `expr` points to a string that stores the postfix expression. It is guaranteed that there is exactly one space between any two operators or operands. The function `EvalPostfix` is supposed to return the value of the expression. If it is not a legal postfix expression, `EvalPostfix` must return a special value `Infinity` which is defined by the judge program.

## Sample program of judge:

```c++
#include <stdio.h>
#include <stdlib.h>

typedef double ElementType;
#define Infinity 1e8
#define Max_Expr 30   /* max size of expression */

ElementType EvalPostfix( char *expr );

int main()
{
    ElementType v;
    char expr[Max_Expr];
    gets(expr);
    v = EvalPostfix( expr );
    if ( v < Infinity )
        printf("%f\n", v);
    else
        printf("ERROR\n");
    return 0;
}

/* Your function will be put here */  
```

## Sample Input 1:

```in
11 -2 5.5 * + 23 7 / - 
```

## Sample Output 1:

```out
-3.285714    
```

## Sample Input 2:

```
11 -2 5.5 * + 23 0 / -
```

## Sample Output 2:

```
ERROR
```

## Sample Input 3:

```
11 -2 5.5 * + 23 7 / - *
```

## Sample Output 3:

```
ERROR
```

## 应用Polya四步法解决问题

1. 理解问题(Understand the problem)

    > ​		我们需要编写函数`EvalPostfix( char *expr )`来处理此后缀表达式`expr`，若后缀表达式格式正确，则返回此表达式的答案，若后缀表达式格式错误，则返回`Infinity`。

2. 设计方案(Devise a plan)

    > 后缀表达式的计算需要用栈实现，具体步骤如下:
    >
    > > 1. 遇到数字则入栈
    > > 2. 遇到运算符号则从栈中取出俩个数进行运算
    > > 3. 若表达式正确，计算结束后栈中应只有一个数字为结果。
    > >
    > > - 在计算过程中需要注意以下几种错误:
    > >
    > >     > 1. 遇到运算符时栈中数字不足俩个
    > >     > 2. 遇到除法运算符时栈顶元素为`0`
    > >     > 3. 运算结束后栈中数字数量不为`1`

3. 实施计划(Carry out the plan)

    > 上述方案的代码如下
    >
    > ```c
    > ElementType EvalPostfix( char *expr ){
    >     double numStack[30];
    >     double num;
    >     char temp[30];
    >     int numStackTop = 0, numStackBase = 0;
    >     int SignCount = 0;
    >     int exprHead = 0, exprTail = 0;
    >     int cnt = 0;
    >     for(int i=0;expr[i]!='\0';i++,exprTail++){
    >         if((expr[i]=='+'||expr[i]=='-'||expr[i]=='*'||expr[i]=='/')&&(expr[i+1]<'0'||expr[i+1]>'9')){
    >             SignCount++;
    >             if(numStackTop-numStackBase<2)
    >                 return Infinity;
    >             if(expr[i]=='+'){
    >                 num = numStack[numStackTop-2]+numStack[numStackTop-1];
    >                 numStackTop-=2;
    >                 numStack[numStackTop++]=num;
    >             }else if(expr[i]=='-'){
    >                 num = numStack[numStackTop-2]-numStack[numStackTop-1];
    >                 numStackTop-=2;
    >                 numStack[numStackTop++]=num;
    >             }else if(expr[i]=='*'){
    >                 num = numStack[numStackTop-2]*numStack[numStackTop-1];
    >                 numStackTop-=2;
    >                 numStack[numStackTop++]=num;
    >             }else if(expr[i]=='/'){
    >                 if(numStack[numStackTop-1]==0)
    >                     return Infinity;
    >                 num = numStack[numStackTop-2]/numStack[numStackTop-1];
    >                 numStackTop-=2;
    >                 numStack[numStackTop++]=num;
    >             }
    >             if(expr[i+1]=='\0')
    >                 break;
    >             i+=1;
    >             exprTail=i;
    >             exprHead=i+1;
    >         }
    >         else if(expr[i]==' '){
    >             cnt = 0;
    >             for(cnt = 0;exprHead<exprTail;cnt++,exprHead++){
    >                 temp[cnt] = expr[exprHead];
    >             }
    >             temp[cnt] = '\0';
    >             exprHead=exprTail+1;
    >             numStack[numStackTop++]=atof(temp);
    >         }else if(expr[i+1]=='\0'){
    >             cnt = 0;
    >             for(cnt = 0;exprHead<=exprTail;cnt++,exprHead++){
    >                 temp[cnt] = expr[exprHead];
    >             }
    >             temp[cnt] = '\0';
    >             exprHead=exprTail+1;
    >             numStack[numStackTop++]=atof(temp);
    >         }
    >     }
    >     if(SignCount==0)
    >         return atof(expr);
    >     if(numStackTop!=1)
    >         return Infinity;
    >     else return numStack[numStackTop-1];
    > }
    > ```

4. 反思回顾(Look back)

    > 