---
title: "Einstein's five-houses riddle" 
date: 2019-07-24
lastmod: 2019-07-24
draft: false
categories: ["随笔"]
author: "sherry"
---
最近看到一篇发布在 v2ex [文章](https://www.v2ex.com/t/585712)，po 主抛出了一个问题：“随着年龄增长越来越难以集中注意力，你们有吗？”，他在里面描述道：“解题时的注意力越来越发散，难以在头脑里形成完整的逻辑链”，“反而清楚的记得我在小学时就可以仅凭思考就算出爱因斯坦的那个著名推理题甚至不需要借助纸笔”。

看到他描述的现象感觉和我之前的一篇[文章](https://wanmei.ml/snow/post/about-imagination/)有一点相似之处，只不过他称之为“注意力”，我在文章中描述的是“想象力的强度”，需要“注意力”，但又不完全是”注意力“。他的经历也从某种程度上佐证了我那个观点——幼年时期的大脑皮层可能更为活跃

<!--more-->

之后我也来尝试不借助纸笔来推理一下那道[谜题](https://udel.edu/~os/riddle.html)。结果完全失败了，不借助纸笔只能做到把房子排列的颜色列出来，还有一些附带的零碎信息。之后只能借助纸笔，将它完全画了出来，我来说一下我的思路：

原题摘抄如下：

### The situation

1. There are 5 houses in five different colors.
2. In each house lives a person with a different nationality.
3. These five owners drink a certain type of beverage, smoke a certain brand of cigar and keep a certain pet.
4. No owners have the same pet, smoke the same brand of cigar or drink the same beverage. 

The question is: Who owns the fish? 

### Hints

- the Brit lives in the red house
- the Swede keeps dogs as pets
- the Dane drinks tea
- the green house is on the left of the white house
- the green house's owner drinks coffee
- the person who smokes Pall Mall rears birds
- the owner of the yellow house smokes Dunhill
- the man living in the center house drinks milk
- the Norwegian lives in the first house
- the man who smokes blends lives next to the one who keeps cats
- the man who keeps horses lives next to the man who smokes Dunhill
- the owner who smokes BlueMaster drinks beer
- the German smokes Prince
- the Norwegian lives next to the blue house
- the man who smokes blend has a neighbor who drinks water 

Einstein wrote this riddle this century. He said that 98% of the world could not solve it. 

### The solution

由上文可知，房子一共有 red，green，white，yellow，blue 五种颜色。

> he Norwegian lives in the first house  
> the Norwegian lives next to the blue house

蓝色一定是排在第二位

> the green house is on the left of the white house  
> the green house's owner drinks coffee  
> the man living in the center house drinks milk

以上三句可以推理出绿色一定是在白色左边的，而且绿色不在中间（第三位），所以绿色在第四位，白色在第五位。

> the Brit lives in the red house

英国人住的红房子，而住在第一个房子里的挪威人只能是黄色，红色排在第三位。

因此根据已知的信息我们可以得到一个关系表格：

| House | 1 | 2 | 3 | 4 | 5 |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Color | Yellow | Blue | Red | Green | White |
| Natl | Norwegian |  | Brit |  |  |
| Bevg | | | | | |
| Smokes | | | | | |
| Pet | | | | | |

再来就是他们的行为：

> the owner of the yellow house smokes Dunhill  
> the man living in the center house drinks milk  
> the green house's owner drinks coffee  
> the man who keeps horses lives next to the man who smokes Dunhill

这四句，显而易见，Norwegian 抽的是 Dunhill，Brit 喝得是牛奶，绿房子房主喝得是咖啡，Norwegian 的邻居（即蓝房子房主）家里养的是马。

此时，关系图表如下所示：

| House | 1 | 2 | 3 | 4 | 5 |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Color | Yellow | Blue | Red | Green | White |
| Natl | Norwegian |  | Brit |  |  |
| Bevg | | | Milk | Coffee | |
| Smokes | Dunhill | | | | |
| Pet | | Horse | | | |

> the owner who smokes BlueMaster drinks beer  
> the Dane drinks tea

结合已知信息，饮料一共有牛奶，咖啡，啤酒，茶和水，其中 3,4 分别是牛奶和咖啡，丹麦人喝茶（排除1，3,4,原因：挪威人，牛奶，咖啡），而抽 BlueMaster 的喝得是啤酒（排除1,3，4,原因：抽 Dunhill，牛奶，咖啡），所以只剩一个水选项，只能给挪威人。因此5号房间只能是抽 BlueMaster，喝啤酒的人。因为丹麦人喝茶，所以只能在2号房间。

> the man who smokes blend has a neighbor who drinks water 

喝水的是一号房间，只能是二号抽的 blend。

> the German smokes Prince

根据 1,2,5 已有 smoke，4 是英国人，推理出 4 号房间是德国人，抽 prince

> the Swede keeps dogs as pets

因此，瑞典人只能是 5 号房（1：Norweg，2：Dane，3：Brit，4：German）

> the person who smokes Pall Mall rears birds

只有 3 号房间同时缺少 smoke 和 pet，因此 3 号房间抽 Pall Mall，养鸟。

> the man who smokes blends lives next to the one who keeps cats

所以只能是 1 号房间养猫（3 号已经养鸟了）

最后只剩下一个空位，只能是 German 养鱼。

最后关系图表如下：

| House | 1 | 2 | 3 | 4 | 5 |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Color | Yellow | Blue | Red | Green | White |
| Natl | Norwegian | Dane | Brit | German | Swede |
| beverages | Water | Tea | Milk | Coffee | Beer |
| Smokes | Dunhill | Blends | Pall Mall | Prince | Blue Master |
| Pet | Cat | Horse | Birds | Birds | Dogs |