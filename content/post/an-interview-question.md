---
title: "一道笔试题"
date: 2018-04-16
lastmod: 2018-04-16
draft: false
categories: ["数据结构与算法"]
tags: ["算法"]
author: "sherry"
---
## 前言

前段时间参加了某头条的笔试，出的五道编程题都跟算法有关。其中有道题大致意思是这样的：__输入一个表达式，输出由‘6’这个字符组成的运算结果的图形。也就是说，假设输入5*6+6,就需要输出由‘6’组成的结果为“36”的图形。__

事实上，这道题考得是“表达式求值”。关键点就在于需要将这个表达式拆开并按照加减乘除和括号之间的优先级算法，首先需要算出具体的答案，然后取余跟事先画好的0-9的图形进行匹配即可。可惜我当时一直在想怎么表达出这些毫无规律的“数字图形”，却没意识到考点其实是<数据结构>上曾经学过的表达式求值:(

<!--more-->

一般来说，表达式求值有“中缀表示法”和“后缀表示法”(又称“逆波兰表示法”)两种。适合人类阅读的便是“中缀表示法”，例如：5+6\*7 但“后缀表示法”更适合计算机来计算，因为无需判断操作符号的优先级，例如：567\*+

虽然“逆波兰表示法”更容易编程，然而事实上，一般都会输入“中缀表达式”，因此还要转化为“逆波兰表示法”，所以两种算法个人感觉没有哪个更为简单。

要判断运算符优先级，首先要将数字和运算符分两边存储，然后看第一个运算符和第二个运算符之间的大小，如果第一个比第二个大或者相等就可以先算第一个，反之则要先计算第二个，很符合先进先出的规则。这样就很自然的想到用到栈来表示。

### 中缀表达式

以下是“中缀表达式”在栈中执行过程，假设表达式为：__5+6*7/8__

首先，遇到 5 ，把 5 送到操作数栈，遇到 + ，送到操作符栈：

操作数栈 | 操作符栈
:---: | :---:
5 | +

遇到 6，压入操作数栈中

操作数栈 | 操作符栈
:---: | :---:
6 |
5 | +

遇到乘，由于 * 的优先级高于 + ，所以，现在就什么也不做，只把乘号进栈：

操作数栈 | 操作符栈
:---: | :---:
6 | *
5 | +

遇到 7 ，压入操作数栈：

操作数栈 | 操作符栈
:---: | :---:
7 |
6 | *
5 | +

遇到 / ，即可计算 6*7 ，从操作数栈中取出两个数字，从操作符中取出一个操作符，并将结果压入操作数栈：

操作数栈 | 操作符栈
:---: | :---:
6*7 |
5 | +

将 / 入操作符栈：

操作数栈 | 操作符栈
:---: | :---:
6*7 | /
5 | +

将 8 入操作数栈：

操作数栈 | 操作符栈
:---: | :---:
8 |
6*7 |/
5 | +

全部压入栈中后，每次只需从操作数中取出两个数字，从操作符中取出一个运算符，并将其结果压入操作数栈中即可。

### 调度场算法

以下来源于Wikipedia:

> 调度场算法（Shunting Yard Algorithm）是一个用于将中缀表达式转换为后缀表达式的经典算法，由艾兹格·迪杰斯特拉引入，因其操作类似于火车编组场而得名。

Input: 3 + 4 × 2 ÷ ( 1 − 5 ) ^ 2 ^ 3

Token | Action | Output(in RPN) | Operator stack | Notes
:---: | :----- | :------------- | -------------: | :---:
3 | Add token to output | 3
\+ | Push token to stack | 3 | +
4 | Add token to output | 3 4 | +
× | Push token to stack | 3 4 | × + | × has higher precedence than +
2 | Add token to output | 3 4 2 | × +
÷ | Pop stack to output | 3 4 2 × | + | ÷ and × have same precedence
(same up) | Push token to stack | 3 4 2 × | ÷ + | ÷ has higher precedence than +
( | Push token to stack | 3 4 2 × | ( ÷ +
1 | Add token to output | 3 4 2 × 1 | ( ÷ +
− | Push token to stack | 3 4 2 × 1 | − ( ÷ +
5 | Add token to output | 3 4 2 × 1 5 | − ( ÷ +
) | Pop stack to output | 3 4 2 × 1 5 − | ( ÷ + | Repeated until "(" found
(same up) | Pop stack | 3 4 2 × 1 5 − | ÷ + | Discard matching parenthesis
^ | Push token to stack | 3 4 2 × 1 5 − | ^ ÷ + | ^ has higher precedence than ÷
2 | Add token to output | 3 4 2 × 1 5 − 2 | ^ ÷ +
^ | Push token to stack | 3 4 2 × 1 5 − 2 | ^ ^ ÷ + | ^ is evaluated right-to-left
3 | Add token to output | 3 4 2 × 1 5 − 2 3 | ^ ^ ÷ +
end | Pop entire stack to output | 3 4 2 × 1 5 − 2 3 ^ ^ ÷ +

可以看出，基本上是和“中缀表达式”原理差不多的，也是需要比较运算符的优先级，按照计算的先后顺序排序。

### 逆波兰表达式

以下例子来源于Wikipedia:

例如：中缀表达式__5+((1+2)\*4)−3__写作__512+4\*+3−__

输入 | 操作 | 堆栈 | 注释
:---: | :---: | :---: | :---:
5 | 入栈 | 5 |
1 | 入栈 | 5, 1 |
2 |入栈 | 5, 1, 2 |
\+ | 加法运算 | 5, 3 | (1, 2)出栈；将结果（3）入栈
4 | 入栈 | 5, 3, 4 |
\* | 乘法运算 | 5, 12 | (3, 4)出栈；将结果（12）入栈
\+ | 加法运算 | 17 | (5, 12)出栈；将结果 (17)入栈
3 | 入栈 | 17, 3 |
− | 减法运算 | 14 | (17, 3)出栈；将结果（14）入栈

计算完成时，栈内只有一个操作数，这就是表达式的结果：14

### 程序

再次回到这个笔试题目，我这里做了些改动，毕竟手动画出用‘6’组成的0-9的图形很累，也不美观，因此我在[AsciiArt](https://www.asciiart.eu/)的常用问题页面找了个自动[生成AsciiArt](http://www.network-science.de/ascii/)的网页，经过对比，感觉“larry3d”这个字体比较美观，因此我选择了这个来生成图案。

```java
package interview;

import java.util.Scanner;
import java.util.Stack;

public class num_graphic {
    private String getGraphic(char rel){
        String zero= "\n" +
                "   __     \n" +
                " /'__`\\   \n" +
                "/\\ \\/\\ \\  \n" +
                "\\ \\ \\ \\ \\ \n" +
                " \\ \\ \\_\\ \\\n" +
                "  \\ \\____/\n" +
                "   \\/___/ \n" +
                "          ";
        String one="\n" +
                "   _     \n" +
                " /' \\    \n" +
                "/\\_, \\   \n" +
                "\\/_/\\ \\  \n" +
                "   \\ \\ \\ \n" +
                "    \\ \\_\\\n" +
                "     \\/_/\n" +
                "         ";
        String two="   ___     \n" +
                " /'___`\\   \n" +
                "/\\_\\ /\\ \\  \n" +
                "\\/_/// /__ \n" +
                "   // /_\\ \\\n" +
                "  /\\______/\n" +
                "  \\/_____/ \n" +
                "           ";
        String three="   __     \n" +
                " /'__`\\   \n" +
                "/\\_\\L\\ \\  \n" +
                "\\/_/_\\_<_ \n" +
                "  /\\ \\L\\ \\\n" +
                "  \\ \\____/\n" +
                "   \\/___/ \n" +
                "          ";
        String four="\n" +
                " __ __      \n" +
                "/\\ \\\\ \\     \n" +
                "\\ \\ \\\\ \\    \n" +
                " \\ \\ \\\\ \\_  \n" +
                "  \\ \\__ ,__\\\n" +
                "   \\/_/\\_\\_/\n" +
                "      \\/_/  \n" +
                "            ";
        String five="\n" +
                " ______    \n" +
                "/\\  ___\\   \n" +
                "\\ \\ \\__/   \n" +
                " \\ \\___``\\ \n" +
                "  \\/\\ \\L\\ \\\n" +
                "   \\ \\____/\n" +
                "    \\/___/ \n" +
                "           ";
        String six="  ____    \n" +
                " /'___\\   \n" +
                "/\\ \\__/   \n" +
                "\\ \\  _``\\ \n" +
                " \\ \\ \\L\\ \\\n" +
                "  \\ \\____/\n" +
                "   \\/___/ \n" +
                "          ";
        String seven="\n" +
                " ________ \n" +
                "/\\_____  \\\n" +
                "\\/___//'/'\n" +
                "    /' /' \n" +
                "  /' /'   \n" +
                " /\\_/     \n" +
                " \\//      \n" +
                "          ";
        String eight="\n" +
                "   __     \n" +
                " /'_ `\\   \n" +
                "/\\ \\L\\ \\  \n" +
                "\\/_> _ <_ \n" +
                "  /\\ \\L\\ \\\n" +
                "  \\ \\____/\n" +
                "   \\/___/ \n" +
                "          ";
        String nine="\n" +
                "   __      \n" +
                " /'_ `\\    \n" +
                "/\\ \\L\\ \\   \n" +
                "\\ \\___, \\  \n" +
                " \\/__,/\\ \\ \n" +
                "      \\ \\_\\\n" +
                "       \\/_/\n" +
                "           ";
        switch (rel){
            case '0':
                return zero;
            case '1':
                return one;
            case '2':
                return two;
            case '3':
                return three;
            case '4':
                return four;
            case '5':
                return five;
            case '6':
                return six;
            case '7':
                return seven;
            case '8':
                return eight;
            case '9':
                return nine;
            default:
                System.out.println("Unknown error...");
        }
        return "";
    }

    private float evaluate(String expression) {
        char[] tokens = expression.toCharArray();

        Stack<Float> stackOfNum = new Stack<>();
        Stack<Character> stackOfOps = new Stack<>();

        for (int i = 0; i < tokens.length; i++) {

            if (tokens[i] == ' ')
                continue;

            if (tokens[i] >= '0' && tokens[i] <= '9') {
                StringBuilder str = new StringBuilder();

                while (i < tokens.length && tokens[i] >= '0' && tokens[i] <= '9') {
                    str.append(tokens[i++]);
                }
                i--; // 回退一位
                stackOfNum.push(Float.parseFloat(str.toString()));
            } else if (tokens[i] == '(')
                stackOfOps.push(tokens[i]);
            else if (tokens[i] == ')') {
                while (stackOfOps.peek() != '(')
                    stackOfNum.push(calculate(stackOfOps.pop(), stackOfNum.pop(), stackOfNum.pop()));
                stackOfOps.pop();
            } else if (tokens[i] == '+' || tokens[i] == '-' || tokens[i] == '*' || tokens[i] == '/') {

                while (!stackOfOps.empty() && hasPrecedence(tokens[i], stackOfOps.peek()))
                    stackOfNum.push(calculate(stackOfOps.pop(), stackOfNum.pop(), stackOfNum.pop()));

                stackOfOps.push(tokens[i]);
            }
        }

        while (!stackOfOps.empty())
            stackOfNum.push(calculate(stackOfOps.pop(), stackOfNum.pop(), stackOfNum.pop()));

        return stackOfNum.pop();
    }

    private boolean hasPrecedence(char op1, char op2) {
        if (op2 == '(' || op2 == ')')
            return false;
        return (op1 != '*' && op1 != '/') || (op2 != '+' && op2 != '-');
    }

    private float calculate(char op, float b, float a) {
        switch (op) {
            case '+':
                return a + b;
            case '-':
                return a - b;
            case '*':
                return a * b;
            case '/':
                if (b == 0) {
                    throw new UnsupportedOperationException("Cannot divide by zero");
                }
                return a / b;
        }
        return 0;
    }

    public static void main(String[] args){
        Scanner in=new Scanner(System.in);
        System.out.print("Enter expression:");
        String input=in.next();
//        String[] parts=input.split("(?=[+-/*])|(?<=[+-/*])");
        num_graphic ng=new num_graphic();
        int result=(int)ng.evaluate(input);
        String str = result+"";
        for (int i=0;i<str.length();i++){
            System.out.println(ng.getGraphic(str.charAt(i)));
        }
    }
}
```

## 参考

> 逆波兰表达式：https://en.wikipedia.org/wiki/Reverse_Polish_notation  
> 调度场算法：https://en.wikipedia.org/wiki/Shunting-yard_algorithm  
> http://www.network-science.de/ascii/  
> https://www.asciiart.eu/faq