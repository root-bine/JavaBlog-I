## 1、<span style="color:brown">索引概述：</span>

**1.1、介绍：**

<u>索引（`Index`）</u>是帮助MySQL<span style="color:purple">**高效获取数据**</span>的<span style="color:green">**数据结构**</span>（<span style="color:red">**有序**</span>）

**1.2、演示：**

执行SQL语句：`select * from user where age = 45;`，查找结果如下：

![image-20230305095238868](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro03.png)

- 在无索引的情况下，查找age字段为45的数据行，会进行全表扫描，效率很低
- 在有索引的情况下，会将<u>*表中数据组合成一个二叉树的节点（**指向数据行的地址**）*</u>，查找过程：
  - 45 > 36，查找树的右边；
  - 45 < 48，查找树的左边；
  - 45 = 45，找到所在的行数据地址；

**1.3、优缺点：**

|                             优势                             |                             劣势                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <span style="color:green">提高数据检索的效率，降低数据库的IO成本</span> |      <span style="color:red">索引列要占用空间的</span>       |
| 通过<u>索引列对数据进行排序</u>，<span style="color:green">降低数据排序的成本，降低CPU的消耗</span> | 索引提高了查询效率，却也<span style="color:red">降低更新表的速度</span>，<br>如：对表进行INSERT、UPDATE、 DELETE时，效率降低。 |



## 2、<span style="color:brown">索引结构：</span>

**2.1、类别：**

<u>MySQL的索引是在**存储引擎层**实现的</u>，不同的存储引擎有不同的结构，主要包含以下几种：

> ①InnoDB   ②MyISAM   ③Memory

|                   索引结构                    |                             描述                             |   支持情况    |
| :-------------------------------------------: | :----------------------------------------------------------: | :-----------: |
| <span style="color:red">**B+Tree索引**</span> |      最常见的索引类型，大部分引擎都支持<u>B+树</u>索引       |      ①②③      |
| <span style="color:green">**Hash索引**</span> | 底层数据结构是用哈希表实现的，<br><u>只有精确匹配索引列的查询才有效，**不支持范围查询**</u> |       ③       |
|              R-tree（空间索引）               | MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少 |       ②       |
|             Full-text（全文索引）             | 一种通过建立倒排索引，快速匹配文档的方式，类似于Lucene,Solr,ES | ①(5.6以后)  ② |

**2.2、二叉树&红黑树分析：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro04.png" alt="image-20230305103159540" style="zoom:67%;" />

<u>*二叉树的缺点*</u>：1、顺序插入时，会形成一个链表，查询性能大大降低；2、大数据量情况下，层级较深，检索速度慢；

<u>*红黑树的缺点*</u>：大数据量情况下，层级较深，检索速度慢；

**2.3、B-Tree（<span style="color:purple">多路平衡查找树</span>）详解：**

> 5阶B-Tree的标准：<span style="color:red">总共有5个子结点，每个节点最多存储4个key，5个指针</span>！！！

以<u>一颗最大度数为5</u>的<u>5阶B树</u>为例:

![image-20230305105255797](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro05.png)

​		在实际演变过程中，B树是采用**向上分裂**的形式进行，流程如下：插入数据`23、234、345、899`，此时已经满足五阶的条件，若在添加一条数据`1000`，那么就是不符合5阶，因此会将中间的数据（`345`）向上分裂；再分别添加两条数据`685、750`，它们都大于345，进入右节点，此时右节点满足5阶，此时再添加一条数据`900`（大于345），进入右节点，再次进行将899向上分裂；分裂完成后，根节点为：`[345,899]`，子结点分别是：`[23,234]`，`[685,750]`，`[900,1000]`；再加入数据`600、780、366`，它大于345，小于899，进入节点：`[685,750]`，此时又将`685`向上分裂；分裂后，根节点为：`[345,685]U[685,899]`，子结点为：`[23,234]`，`[366,600]`，`[750,780]`，`[900,1000]`；再加入数据`960、1100、1200`，都大于`899`，进入节点`[900,1000]`，并将`1000`向上分裂；分裂后，根节点为：`[345,685]U[685,899]U[899,1000]`，子结点：`[23,234]`，`[366,600]`，`[750,780]`，`[900,960]`、`[1100,1200]`。此时已经满足5阶B-tree标准，若现在再次添加数据，达到向上分裂条件，操作类似于上述步骤。结果如下：

![image-20230305111635121](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro06.png)

## 3、<span style="color:brown">B+Tree：</span>

**3.1、B+Tree详解：**

B+Tree是B-Tree的变种，存在以下特点：

1. <span style="color:red">B+ Tree中所有元素都会出现在叶子节点</span>；
2. <u>非叶子节点主要起到索引作用，叶子节点用于存储数据</u>；
3. <span style="color:red">所有叶子节点，会形成一个单向链表</span>；

> 最大度数为3的3阶B+Tree（2个key，3个指针）

![image-20230305114032232](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro07.png)

​		B+Tree与B树的演变过程不同，此处采用5阶演示：插入数据`232、234、567、1000`，此时符合5阶，然后再添加一条数据`890`，  将`567`向上分裂，此时根节点为：`567`，子结点为：`[232,23]`，[<u><span style="color:purple">**567**</span>,890]U[890,1000]</u>，且子结点形成了一条单向链表；插入数据`1234、2345`，它们大于`567`，进入右节点，此时`1000`向上分裂并保留在子结点中；完成分裂后，根节点为：`[567,1000]`，子结点：`[232,234]`，[<u><span style="color:purple">**567**</span>,890]</u>，[<u><span style="color:purple">**1000**</span>,1234]U[1234,2345]</u>，且子结点形成一条单向链表。最后结果为：

![image-20230305115843734](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro08.png)

**3.2、B+Tree优化：**

​		MySQL索引数据结构对经典的B+Tree进行了优化：<u>在原B+Tree的基础上，增加一个<span style="color:purple">**指向相邻叶子节点的链表指针**</span>，就形成了带有顺序指针的B+Tree，**提高区间访问的性能**</u>，图示如下：

![image-20230305121045104](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro09.png)

**3.3、为什么InnoDB存储引擎选择使用B+tree索引结构？**🤐🤐🤐

1. 相对于二叉树，层级更少，搜索效率高；
2. 对于B-tree，无论是叶子节点还是非叶子节点都会保存数据，导致一页中存储的键值和指针减少，若要同样保存大量数据，只能增加树的高度，导致性能降低；
3. 相对Hash索引，B+tree支持**范围匹配及排序**操作；



## 4、<span style="color:brown">Hash索引：</span>

**4.1、概述：**

​		哈希索引就是采用一定的hash算法，<span style="color:red">将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中</span>。如果两个(或多个)键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决。

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/MySQL_Pro10.png" alt="image-20230305160707331" style="zoom:67%;" />

**4.2、特点：**

- Hash索引只能用于对等比较(=，in)，不支持范围查询(between，>，<，...)
- 无法利用哈希索引完成排序操作

- <u>*查询效率高，通常只需要一次检索，效率通常要高于B+tree索引*</u>（无hash冲突情况）

**4.3、存储引擎支持：**

​		在MySQL中，支持hash索引的是**Memory引擎**，而<u>*InnoDB中具有自适应hash功能*</u>，hash索引是存储引擎根据B+Tree索引在指定条件下自动构建的。
