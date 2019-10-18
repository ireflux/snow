---
title: "栈和队列的相互转换"
date: 2019-08-18
lastmod: 2019-08-18
draft: false
categories: ["Clavicula Salomonis"]
tags: ["algorithm"]
author: "sherry"
---
关于栈和队列，有时会在面试题中看到诸如以下的问题：

1. 如何使用栈实现队列？
2. 如何使用队列实现栈？

栈的特点是 last in, first out (LIFO)，而队列的特点是First-In-First-Out (FIFO) 

接下来根据和队列的特点开始分析一下，首先看第一个问题，要想用栈实现队列，则至少需要两个栈，假如用 stack1 和 stack2 分别表示这两个栈。思路如下：

进入此 “队列” 时，需要先 push 进 stack1; pop 时如若 stack2 是非空的，则直接从 stack2 pop，如果是空的则需要将 stack1 中的元素全部 push 到 stack2 中，再进行出 pop 操作。

<!--more-->

以下是实现及测试代码：

```java
public class StackToQueue {

    private Stack<Integer> stack1 = new Stack<>();
    private Stack<Integer> stack2 = new Stack<>();

    private void pushQueue(int num){
        stack1.push(num);
    }

    private Integer popQueue(){

        if(!stack2.empty() || !stack1.empty()){
            if(stack2.empty()){
                while (!stack1.empty()){
                    stack2.push(stack1.pop());
                }
            }
            return stack2.pop();
        } else {
            return -1;
        }
    }

    public static void main(String[] args){
        StackToQueue stackToQueue = new StackToQueue();
        stackToQueue.pushQueue(1);
        stackToQueue.pushQueue(2);
        stackToQueue.pushQueue(3);

        System.out.println(stackToQueue.popQueue());
        System.out.println(stackToQueue.popQueue());

        stackToQueue.pushQueue(4);
        System.out.println(stackToQueue.popQueue());
        System.out.println(stackToQueue.popQueue());
    }
```

第二题使用队列实现栈，同样使用 queue1 和 queue2 来分别代表两个队列， 这里偏个题，忽然间发现，里面还有一个 ArrayDeque 的实现，以前没有见到过，查了下资料发现是 jdk6 加的，具有双向的特性，队列和栈的特性集一身，还看到了篇 stack over flow 的提问，详情不再赘述，见文末参考资料。言归正传，队列实现栈的思路如下：

入 ”栈” 时，假设 1,2,3 依次进入了 queue1，出 “栈” 时，需要先出 3，因此需要先将 1,2 从队列中出来放到 queue2, 然后再次需要出 “栈” 时，需要出 2,因此此时需要先将 1 从 queue2 中出来放到 queue1 中，再从 queue2 中出 2。即出队列时，若队列不为空，需要将 n-1 元素转移到另一个空的队列中。

以下是实现及测试代码：

```java
public class QueueToStack {

    private Queue<Integer> queue1 = new ArrayDeque<>();
    private Queue<Integer> queue2 = new ArrayDeque<>();

    private void pushStack(Integer num){
        if(!queue1.isEmpty()){
            queue1.offer(num);
        }
        queue2.offer(num);
    }

    private Integer popStack(){
        if(queue1.isEmpty() && queue2.isEmpty()){
            return -1;
        }

        if(!queue1.isEmpty()){
            while (queue1.size() > 1){
                queue2.offer(queue1.poll());
            }
            return queue1.poll();
        } else{
            while (queue2.size() > 1){
                queue1.offer(queue2.poll());
            }
            return queue2.poll();
        }
    }

    public static void main(String[] args){
        QueueToStack queueToStack = new QueueToStack();
        queueToStack.pushStack(1);
        queueToStack.pushStack(2);
        queueToStack.pushStack(3);
        System.out.println(queueToStack.popStack());
        System.out.println(queueToStack.popStack());
        queueToStack.pushStack(4);
        System.out.println(queueToStack.popStack());
        System.out.println(queueToStack.popStack());
    }
}
```

## 参考资料

> [Why is ArrayDeque better than LinkedList](https://stackoverflow.com/questions/6163166/why-is-arraydeque-better-than-linkedlist)  
[java api queue](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)  
[java api ArrayDeque](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayDeque.html)