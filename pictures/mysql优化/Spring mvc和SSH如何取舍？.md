
问:

> **最近好多项目都在用Spring MVC，而SSH大多是几个老项目在用，Spring MVC要比SSH优秀在什么地方，是否要远离SSH**


## **答一:**

他们都是表现层层面的东西，我从几个方面来比较这两个框架：

**1.易用性**

Spring MVC上手简单，并且可以与Spring无缝结合，毕竟都是一个公司的产品，学习起来也比较简单，比如从前端给对象填充值，他的处理就比Struts2简单多了，再比如对Restful风格的URL的支持，这些Spring MVC都比Struts2做的好N倍。

**2.安全性**

说到安全性，我也不想多提Struts2了，我在一家游戏公司工作，之前公司的老项目是用的Struts2，他今年出了不少漏洞，并且是致命性漏洞，每出一次漏洞，我需要加班一次，好吧，我想说我加了4次班了，最可恶的是Struts2有漏洞后，还把攻击方法放到网上。Spring MVC到目前为止还没有发现比较严重的漏洞。

**3.可扩展性**

Spring MVC依靠Spring这颗大树，Spring的实力我想大家不用怀疑吧，包括版本的更新、迭代这些都是经过历史见证的。

## **答二**

首先，纠正一个概念上的错误。

ssh一般意义上是指 struts，spring framework以及hibernate。

这三个框架作用是不一样的。hibernate主要是用于持久层，struts主要是用于mvc，而spring主要用于aop和ioc。再来看 spring mvc。从名字来看，就知道这是一个mvc框架，所以，spring mvc和ssh根本就没有可比性，他们不是一个东西。spring mvc和struts都是mvc框架，他俩才有比较的意义。回到正题，spring mvc和struts都用过，感觉spring mvc更加的灵活，更不容易出错，开发成本也比较低。刚毕业一直用struts，后期转到了spring mvc上。从此不能收手。


## **答三**

现在的趋势是框架除了做最关键的东西，不要封装的太强。因为大家更愿意把学习的精力放在底层如servlet,sql上，而不是struts,hql上。springmvc,mybatis都是这样被人接受的。