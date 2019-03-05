参考：[51cto博客_算法](http://363883844.blog.51cto.com/2977542/1544700)
#### 什么是堆
* 堆的关系满足：（k<sub>i</sub><=k<sub>2i</sub> ,k<sub>i</sub><=k<sub>2i+1</sub>）或者（k<sub>i</sub>>=k<sub>2i</sub> ,k<sub>i</sub>>=k<sub>2i+1</sub>)
* 堆的分类：顺序存储堆，链式存储堆（*个人认为顺序存储堆才是真正的堆，链式存储堆不就是二叉树么*）  
  链式存储堆 = 左式堆 + 斜堆 + 二项堆
* 堆总是一颗完全二叉树，完全二叉树是其结点编号能与其所在的满二叉树完全对应的二叉树，满二叉树是每一层上的结点数都是该层的最大结点数的二叉树。

#### 顺序存储堆
* 顺序存储堆从物理结构上看是数组，从逻辑结构上看是完全二叉树。

#### 左式堆
* 每一个结点其左子树零路径长不小于右子树
* 零路径长：结点到一个叶子结点的最短路径长

#### 斜堆
* 与左式堆类似，不同之处在于合并后不管左右子树零路径大小都必须交换左右子树

#### 二项堆
* 二项堆的每个结点都是二叉树的根结点。合并时将相同度的二叉树合并。