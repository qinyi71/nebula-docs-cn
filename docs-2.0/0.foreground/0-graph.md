# 图

图是计算机科学研究的主要领域之一。图能够高效地解决目前存在的诸多问题。本章将说明图数据库在使用方面的优点以及图数据库在现代应用程序开发中的巨大潜力。并介绍分布式图数据库的区别和几种其他类型的数据库。当前，从计算机行业巨头（例如 Amazon 和 Facebook）到小型研究团队，都投入了大量的资源探索图数据库在解决各种数据关系问题上的潜力。当然你也可以选择像他们这样进行尝试。现在可供选择的数据库有很多，那么图数据库在数据库领域里处于什么位置呢？

## 图、图片与图论

图无处不在。当听到图这个词时，很多人都会想到条形图或折线图，因为有时候我们确实会把它们称作图。从传统意义上来说，图是用来展示两个或多个数据系统之间的联系的。最简单的例子如下图，下图展示了 Nebula Graph GitHub 仓库星星数量随时间的变化。

![image](https://user-images.githubusercontent.com/42762957/91426247-d3861000-e88e-11ea-8e17-e3d7d7069bd1.png "这不是本书所说的图")

这是相对比较简单的一种图，横轴为时间，纵轴为星星数量。可以看到，星星数量是随着时间变化上升的。这种类型的图通常称为拆线图。折线图可以显示随时间（根据常用比例设置）而变化的连续数据。此处我们只给出了折线图的例子。当然图的形式有多种，比如饼图、条形图等。

还有一种“图”在日常口语中会更多的被提及，例如，“图像识别”，“美图秀”，“修图”等。例如下“图”的左边。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/image.png "这也不是本书所说的图")


但是——总会有但是——我们在本书中讨论的图是另外一个概念——“图论”。

在数学的分支“图论”中，图用于表示实体与实体之间的关系，这才是图论的基本研究对象。一张图由一些小圆点（称为顶点或结点, Vertex）和连接这些圆点的直线或曲线（称为边, Edge）组成。“图 Graph”这一名词最早由西尔维斯特在 1878 年提出。

通常，在英文中，为了区分这两种不同的图，前者会称为Image，后者称为Graph。在中文中，前者会强调为"图片"，后者会强调为"拓扑图"，"网络图"等。

![Image](https://docs-cdn.nebula-graph.com.cn/books/images/undirectedgraph.png)

 "这才是本书所说的图"

简单来说，图论就是研究图的学问。图论始于 18 世纪初期的柯尼斯堡七桥问题。柯尼斯堡当时是普鲁士的城市(现在属于俄罗斯，更名为加里宁格勒)。普雷格尔河穿过柯尼斯堡，不仅把柯尼斯堡分成了两部分，而且还在河中间形成了两个小岛。这就将整个城市分割成了四个区域，各区域由七座桥连接。当时有一个与柯尼斯堡相关的脑筋急转弯，即如何只穿过每座桥一次，浏览整个城市的四个区域。下图为柯尼斯堡七座桥的简化图。 有兴趣的话可以试试寻找这个小游戏的答案[^171]。

![image](https://user-images.githubusercontent.com/42762957/91536940-1526c180-e948-11ea-8fe8-90f40ce28171.png)

[^171]: 图片来源 https://medium.freecodecamp.org/i-dont-understand-graph-theory-1c96572a1401.

大数学家欧拉为了解决了这一问题。通过将城市的四个区域抽象成点，将连接城市的七座桥抽象成连接点的边，欧拉证明了这一问题是无法解决的。简化的抽象图如下[^063]：

![image](https://user-images.githubusercontent.com/42762957/91538126-e578b900-e949-11ea-980c-5704254e8063.png)

[^063]: 图片来源 https://medium.freecodecamp.org/i-dont-understand-graph-theory-1c96572a1401

图中四个点代表柯尼斯堡的四个区域，点之间的线代表连接四个区域的七座桥。从图中不难看出，偶数座桥连接的区域可以轻松通过，因为来去可以选择不同的路线。奇数座桥连接的区域只能作为起点或者终点，因为同样的路线只能走一次。和节点相关联的边的条数称为节点度。现在可以证明，只有两个节点有奇数度，另外节点有偶数度时，也即两个区域必须有偶数座桥，剩下的区域有奇数座桥时，柯尼斯堡问题才能解决。然而由上图得知，柯尼斯堡的任何区域都没有偶数座桥，所以这个脑筋急转弯无解。

## 属性图

从数学角度来说，图论是研究建模对象之间关系结构的学科。但是从工业界使用的角度，通常会对基础的图模型进行扩展，称为**属性图模型**。属性图通常由以下几部分组成：

- 节点，即对象或实体。在本书中，通常简称为点（vertex）。
- 节点之间的关系，在本书中，通常简称为边（edge）。通常边是有方向或者无方向的，以表示两个实体之间有持续的关系。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/un-directed.png)

- 此外，在节点和边上，还可以有属性（properties）。

在现实生活中，有很多属性图的例子。

例如像企查查或者BoSS直聘这类的公司，用图来建模商业股权关系网络。这个网络中，点通常是一个自然人或者是一家企业，边通常是某自然人与某企业之间的股权关系。点上的属性可以是自然人姓名、年龄、身份证号等。边上的属性可以是投资金额、投资时间、董监高等职位关系。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/enterprise-relations.png)

在一个股票市场里面，点可以是一家上市公司，边可以是上市公司之间的相关性。点的属性可以为股票代码、简称、市值、板块等；边的属性可以为股价的时间序列相关性系数[^T01]。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/JGraphT01.png)

[^T01]: https://nebula-graph.com.cn/posts/stock-interrelation-analysis-jgrapht-nebula-graph/

图关系还可以是类似<权力的游戏>这样电视剧中的人物关系网[^s-01]：点为人物，边为人物之间的互动关系；点的属性为人物姓名、年龄、阵营等，边的属性（距离）为两个人物之间的互动次数，互动越频繁距离越近。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/game-of-thrones-01.png)

[^s-01]: https://nebula-graph.com.cn/posts/game-of-thrones-relationship-networkx-gephi-nebula-graph/

图也可以用于IT系统内部的治理。例如，对于像微众银行这样的公司中，通常有着非常庞大的数据仓库，以及相应的数仓管理工具。这些管理工具记录了数仓内Hive表之间通过Job实现的ETL关系[^ware]，这样的ETL关系，可以非常方便的用图的形式呈现和管理，当出现问题时也可以非常方便的追溯根源。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/dataware.png)
![image](https://docs-cdn.nebula-graph.com.cn/books/images/dataware2.png)

[^ware]: https://nebula-graph.com.cn/posts/practicing-nebula-graph-webank/

图也可以用于记录一个大型IT系统内部错综复杂的"微服务"之间的调用关系[^tice]，用于运维团队的服务治理目的；这里每个点通常用于表示一个微服务，边表示两个微服务之间的调用关系；这样，运维人员可以方便的寻找可用性低于阈值(99.99%)的调用链路，或者发现那些出故障会影响面特别大的微服务节点。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/microserve.png)

图也可以用于提升代码开发效率。可以用图存放代码之间的函数调用关系[^tice]，用于提升研发团队代码审查和测试的效率；在这里，每个点是代码中的一个函数或者变量，每个边是函数或者变量之间的调用关系；这样，当有新提交的代码之后，人们可以更方便的看到可能会受到影响到的其他接口，这样可以帮助测试人员更好的评估潜在的上线风险。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/code.png)

[^tice]: https://nebula-graph.com.cn/posts/meituan-graph-database-platform-practice/

此外，相对于静态不发生变化的属性图，我们还可以通过增加一些时间信息，发掘出更多的使用场景。

例如，在一个银行间账户资金流向网络里面[^1440w]，点是账户，边是账户之间的转账记录。边属性记录了转账的时间、金额等。同盾、邦盛、半云科技等公司采用图技术，可以方便的通过图的方式被探索发现明显的资金挪用、“以贷还贷”、“团伙贷款”等现象。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/bank-transfer.jpg)

[^1440w]: https://zhuanlan.zhihu.com/p/90635957

同样的方法也可以用于探索发现一些加密货币的流向。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/block-chain.png)

在一个黑产账户和设备网络中[^360]，其中的点可以是账户、手机设备和WIFI，边是这些账户与手机设备之间的登录关系，以及手机设备和 WIFI 之间的接入关系。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/360-user-1.png)

这些登录记录的网络构成了黑产群体网络的团伙作案特征。360数科[^360]、快手[^kuaishou]、微信[^weixin]、知乎[^zhihu]、携程金融这些公司都通过图技术实时（毫秒级的）识别超过百万个的黑产社群。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/360-user-2.png)

[^360]: https://nebula-graph.com.cn/posts/graph-database-data-connections-insight/

[^kuaishou]: https://nebula-graph.com.cn/posts/kuaishou-security-intelligence-platform-with-nebula-graph/

[^weixin]: https://nebula-graph.com.cn/posts/nebula-graph-for-social-networking/

[^zhihu]: https://mp.weixin.qq.com/s/K2QinpR5Rplw1teHpHtf4w

更进一步，除了时间这个维度外，我们通过添加一些地理位置信息，还能发现属性图更多的应用场景。

例如新冠病毒的流行病学溯源[^CoV02]，点是人物，边是人与人之间的接触；点属性为人物的身份证、发病时间等信息，边属性为人物之间发生密切接触的时间和地理位置等。为卫生防疫部门快速识别高风险人群和其行为轨迹提供帮助。

![image](https://www-cdn.nebula-graph.com.cn/nebula-blog/nCoV02.png)

[^CoV02]: https://nebula-graph.com.cn/posts/detect-corona-virus-spreading-with-graph-database/

地理位置与图的结合也可以用于一些O2O的场景，例如基于POI(Point-of-Interest)的实时美食推荐[^mt]，使得美团这类本地生活服务平台公司在消费者在打开APP的时候，实时推荐出更为合适的商家。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/meituan2.png)

![image](https://docs-cdn.nebula-graph.com.cn/books/images/meituan.png)

[^mt]: https://nebula-graph.com.cn/posts/meituan-graph-database-platform-practice/

图还可以用于更深度的知识推理，华为、ViVo、Oppo、微信、美团等公司，将图用于表征底层知识关系的数据模型。

根据文献[^Ubiquity]的统计，使用图技术最多的领域，依次是：

[^Ubiquity]: https://arxiv.org/abs/1709.03188

## 为什么要使用图数据库

虽然关系型的数据库、XML/json等半结构类型的数据库，都可以用来描述图结构的数据模型。但是，可以用一句话来回答：图（数据库）着眼于处理数据之间的关联（拓扑）关系。相比于数据本身，图（数据库）更看重数据之间的关联关系。具体来说，有这么几个优点：

- 图是一种更直观、符合人脑思考直觉的知识表示方式。这使得在抽象业务问题时，着眼于“业务问题本身”，而不是“如何将问题描述为数据库的某种特定结构（例如表格结构）”。

- 图也更容易发现数据的特征，例如转账的路径、近邻的社区。例如如果要分析<权力的游戏>中的的人物派别关系和人物重要性

表的组织方式

![image](https://www-cdn.nebula-graph.com.cn/nebula-blog/gephi-01.jpeg)

远不如图的组织方式直观。

![image](https://www-cdn.nebula-graph.com.cn/nebula-blog/game-of-thrones-01.png)

特别是当某些中心节点被删除

![image](https://www-cdn.nebula-graph.com.cn/nebula-blog/tv-game-thrones.png)

或者，增加一条边，可以彻底的改变整个图拓扑，

![image](https://www-cdn.nebula-graph.com.cn/nebula-blog/tv-game-thrones-02.png)

虽然只是个别数据的细微改变，图可以比表更直观的发现其中的重要而系统的信息。

- 图查询语言是针对图结构访问设计的，可以更加直观。例如下面是一个LDBC中的查询示例，查找某人(Person)在社交网络上发布的帖子(Posts)，以及相应的回复(Message，回复本身还会被多次回复);并且要求发帖时间、回帖时间都满足一定条件；最后根据回帖数量排序。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/efficientquery.png)

如果使用 PostgreSQL 撰写：

```SQL
--PostgreSQL	
WITH RECURSIVE post_all(psa_threadid
                      , psa_thread_creatorid, psa_messageid
                      , psa_creationdate, psa_messagetype
                       ) AS (
    SELECT m_messageid AS psa_threadid
         , m_creatorid AS psa_thread_creatorid
         , m_messageid AS psa_messageid
         , m_creationdate, 'Post'
      FROM message
     WHERE 1=1 AND m_c_replyof IS NULL -- post, not comment
       AND m_creationdate BETWEEN :startDate AND :endDate
  UNION ALL
    SELECT psa.psa_threadid AS psa_threadid
         , psa.psa_thread_creatorid AS psa_thread_creatorid
         , m_messageid, m_creationdate, 'Comment'
      FROM message p, post_all psa
     WHERE 1=1 AND p.m_c_replyof = psa.psa_messageid
     AND m_creationdate BETWEEN :startDate AND :endDate
)
SELECT p.p_personid AS "person.id"
     , p.p_firstname AS "person.firstName"
     , p.p_lastname AS "person.lastName"
     , count(DISTINCT psa.psa_threadid) AS threadCount
END) AS messageCount
     , count(DISTINCT psa.psa_messageid) AS messageCount
  FROM person p left join post_all psa on (
       1=1   AND p.p_personid = psa.psa_thread_creatorid
   AND psa_creationdate BETWEEN :startDate AND :endDate
   )
 GROUP BY p.p_personid, p.p_firstname, p.p_lastname
 ORDER BY messageCount DESC, p.p_personid
 LIMIT 100;
```

如果使用为图专门设计的图语言(Cypher)来写，是如下形式

```Cypher
--Cypher
MATCH (person:Person)<-[:HAS_CREATOR]-(post:Post)<-[:REPLY_OF*0..]-(reply:Message)
WHERE  post.creationDate >= $startDate   AND  post.creationDate <= $endDate
  AND reply.creationDate >= $startDate   AND reply.creationDate <= $endDate
RETURN
  person.id,   person.firstName,   person.lastName,   count(DISTINCT post) AS threadCount,
  count(DISTINCT reply) AS messageCount
ORDER BY
  messageCount DESC,  person.id ASC
LIMIT 100
```

- 由于存储引擎和查询引擎可以针对图的结构专门设计，图的遍历（对应SQL中的join）要高效的多。下图是知名产品 Neo4j 所做的一个对比[^mt]。

![image](https://docs-cdn.nebula-graph.com.cn/books/images/neo4jhop.png)

数据集成（知识图谱）、个性化推荐、欺诈与威胁检测、风险分析与合规、身份（与控制权）验证、IT基础设施管理，供应链与物流、社交网络研究。

2019 年，根据 Garter 的问卷调研，27% 的客户（500组）在使用图数据库，20% 有计划使用。

## RDF

受篇幅所限，本文不讨论 RDF 数据模型。