# [150. Evaluate Reverse Polish Notation](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

### **Note:**

- Division between two integers should truncate toward zero.
- The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

### **Example 1:**

```
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

### **Example 2:**

```
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

### **Example 3:**

```
Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

## 应用Polya四步法解决问题

1. 理解问题(Understand the problem)

    > ​		我们需要编写函数`evalRPN(vector<string>& tokens)`来计算此后缀表达式的值，`tokens`中的值为数字或运算符。

2. 设计方案(Devise a plan)

    > 后缀表达式的计算需要用栈实现，具体步骤如下:
    >
    > > 1. 遍历`tokens`
    > > 2. 遇到数字则入栈
    > > 3. 遇到运算符号则从栈中取出俩个数进行运算
    > > 4. 遍历结束后取栈中唯一值为运算结果
    > >

3. 实施计划(Carry out the plan)

    > 上述方案代码如下
    >
    > ```C
    > class Solution {
    > public:
    >     int evalRPN(vector<string>& tokens) {
    >         stack<int> mystack;
    >         int a,b;
    >         for(int i=0;i<tokens.size();i++){
    >             if((tokens[i][0]>='0'&&tokens[i][0]<='9')||tokens[i].size()>1){
    >                 mystack.push(atoi(tokens[i].c_str()));
    >             }else{
    >                 a = mystack.top();
    >                 mystack.pop();
    >                 b = mystack.top();
    >                 mystack.pop();
    >                 if(tokens[i]=="+")
    >                     mystack.push(a+b);
    >                 else if(tokens[i]=="-")
    >                     mystack.push(b-a);
    >                 else if(tokens[i]=="*")
    >                     mystack.push(a*b);
    >                 else if(tokens[i]=="/")
    >                     mystack.push(b/a);
    >             }
    >         }
    >         return mystack.top();
    >     }
    > };
    > ```
    
4. 反思回顾(Look back)

    > ​		这道题比较容易，只要清楚栈的特性和后缀表达式的特性解答并不难。

