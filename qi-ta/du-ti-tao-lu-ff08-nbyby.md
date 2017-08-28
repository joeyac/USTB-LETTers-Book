# 读题套路\_yby

* 读题目标
  * 确定题目类型：图论、数学、DP、数据结构等
  * 确定题目要求做的事
  * 确定数据范围
    * 很多情况下数据范围能帮助解题
    * 包括确定题目类型和做法等
    * 有些时候数据范围藏在题面里，不在input里，应当注意
  * 判断是否可做？
  * input output怎么搞？
  * 为想题写题搞清障碍，如果可以的话，可以把重点搞出来
* 我的一般流程（仅供参考）
  * 题目短的话，其实可以随意。。。。
  * 确定读哪个题？
    * 看你的英文水平而定，一半会分工。我一向是挑短的读，以及跟着榜读（重要）。
    * 题目长度以及题目的名称其实很关键
    * 气球有时候也是玄学
    * 知己知彼，方能百战不殆。比赛前摸清和自己实力相似地队伍，或者比自己弱地队伍。那么他们出的题，一般都不会难=。=
    * 题面长短很重要。一般长的题有可能难题，有可能模拟，或是逗你玩。其实有些并不是值得放弃的。
  * 然后先读input output
    * 这两个里面会给你很多信息。因为其实题面的话会有很多冗余信息。而这两者里面基本都是有用的。
    * 读完这两个，一般能确定大概类型
      * 比如给了你n个点，m条边，显然是个图
      * 给了查询次数，每次查询是个区间，数据结构?
      * 字符串的题也很好认？
    * 明确了数据规模
      * 数据规模是解题的很大助力
      * 比如看数据规模，就大概知道解法
    * 为读很长的题面来确定其他想要寻找的信息。
  * 之后读样例
    * 有了input output之后，明白了读进来的是啥
    * 很多情况下，能大致猜出要我们干啥了。。。
    * 而且读样例不花时间吧。。
    * 当然，很多的坑样例里面并不会涉及，需要自己之后仔细想清楚细节。
  * 读题面
    * 题面可以倒着读。（从最后一段往前）因为有的题目喜欢讲故事。但越后面的内容往往越是题目的关键信息。
    * 可以跳着读。通过input output可以给我们一些提示，然后我们去找关键信息可以更容易。
    * 实在读不懂的话，在正着读一遍，或者找队友。
    * 注意题面里是否隐藏一些限制，有时候有数据范围。
    * 把题面中的重点圈划出来
      * 有向无环图
      * 图有向无向、有权无权、有没有自环重边、图是否联通
      * 数据结构的话几种操作、查询
      * 数值是否可重复
      * 计数的话是否考虑怎么考虑同样的情况
      * 是否可以将操作查询等离线？或者只能在线做
  * 判断题目类型
    * 这个接下来专门分开来讲
  * 判断是否可做
    * 这个需要我们自己的知识水平？以及对队友的了解？
      * 所以需要多和队友沟通交流，以及加强水平？
    * 哪怕可做的话，开始写代码之前，建议理清楚算法思路，然后理清楚怎么写代码再上机
* 具体题目类型的判断？复杂度？
  * 输入格式输出格式？
    * 给了n个点，m条边显然是张图
      * 图的话，其实一般来说是图论的概率很大，当然看数据规模。
      * n m 很小，几十，考虑状压
    * n个点，n-1条边，连通图，树
      * 树的话，不是树形DP，一般就是数据结构题
    * Q个查询也很关键，一般数据结构、图论、DP
    * 几种操作。。几种查询。。。显然数据结构
    * 计数题？
      * DP、组合数学、polya等
    * 数学题的话一般也比较容易看出来
    * 字符串题的话，都是对连在一起的一些字符操作，而不是字符序列
    * 计算几何？图片一般就有提示？数据规模有精度？这个很好认
  * 数据规模？
    * 首先需要我们知道各种算法复杂度
    * 一般刚好1e9左右，bitset可以考虑
    * 1e18 数位DP、矩阵快速幂、纯数学
    * 1e5 一般nlog nloglog sqrt做法比较少
      * 一般来说数据结构题
    * 1e4做法很多
    * 几十 状压相关 搜搜搜
  * 其他？
    * 题面很长。。。模拟可能性很高
    * 样例很长。。。模拟可能性很高
    * 万事不决。。。然后数据规模在10000以下，一般可以考虑网络流、DP


