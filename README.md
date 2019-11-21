# 时间复杂度

## 从小到大排序

`O(1) -> O(n) -> O(logn) -> O(nlogn) -> O(n^2) -> O(n^3) -> O(2^n)`

> 时间复杂度越大, 即随着数据规模的增大, 算法所需时间的增长率就越大

## 所有对数均可看作logn

如`log2(n)`和`log5(n)` : `log2(n) = log5(2) * log2(n)`, 可以省去常数项系数`log5(2)`, 也即`log2(n) <=> log5(n)`; 从函数图像角度来看, 随n的变大, 两函数增长率变大的幅度处于同一个级别。

# 小技巧

1.  如果递归的过程中有很多重复计算, 考虑将依赖的计算先算出来并做缓存
2.  避免使用`HashMap/HashTree/HashSet`做缓存, 而使用数组等简单数据结构
3.  尽量使用贴近题目规则的数据结构辅助解题, 如约瑟夫环问题可使用循环链表
4.  对于解题过程中感觉无从下手的, 子数据集计算过程如出一辙的, 可考虑采用递归进行分而治之, 如题目`括号的分数`
5.  避免使用`*, /, %, 乘方, 开方`, 而多使用`^, &, >>`
    1.  `n % m` => `n - (m > n ? 0 : m) `(`n >= 0, m > 0, n < 2m` `)
    
6.  二叉树的性质
    1.  非空二叉树的第i层的节点数最多为`2^(i-1)`
    2.  高为h的非空二叉树节点数最多为`2^(h-1)`
    3.  节点的子节点个数称为该节点的度, 树的度为树中节点的度的最大值, 二叉树是度为2的树
    4.  节点的高度是该节点到叶子节点的最长路径经过的节点数, 包括他自己和叶子节点; 节点的深度是该节点到根节点路径中经过的节点数, 包括他自己和根节点; 树的高度就是树的深度
    5.  二叉树的边数之和`T`等于
        1.  从节点的度的角度来看, 就是`T = 度为1的节点数*1 + 度为2的节点*2`, 即`T = n1 + 2 * n2`
        2.  从每条边都会指向一个节点来看, 只有根节点没有被指向, 因此为`T = 节点数 - 1`, 即`T = n0 + n1 + n2 - 1`
        3.  综上有`n0 = n2 + 1`, 使用满二叉树举例可快速验证
    6.  真二叉树: 只有度为0和2的节点; 满二叉树: 在真二叉树的条件下, 度为0的节点都是叶子节点; 完全二叉树: 只有最后两层包含叶子节点, 且最后一层中的叶子节点从左至右依次排列
    
7.  完全二叉树的性质
    1.  完全二叉树: 度为1的节点树要么为1个要么为0个; 节点数相同的不同二叉树中完全二叉树的高度最小
    2.  若完全二叉树的高度为`h`, 则有
        1.  前`h-1`层为满二叉树, 最后一层至少有1个节点, 因此节点数`n >= [2^(h-1) - 1] + 1`, 即`n >= 2^(h-1)`
        2.  高为`h`的二叉树节点数最多为`2^h - 1`, 因此总结点数`n < 2^h - 1`
        3.  综上有: `2^(h-1) <= n < 2^h`, 即`h-1 <= log2(n) < h`, 由于`h`只能取整, 因此`h = floor(log2(n)) + 1`, `floor`为向下取整, `ceiling`为向上取整; 向上向下取整与四舍五入无关, 仅看小数位数是否为零, `ceiling(4.0) = 4`, `ceiling(4.1
        ) = ceiling
        (4.8
        ) = 5`.
    3.  若按从上到下, 从左到右的顺序对节点从1开始进行编号, 那么对于某个节点`i`有:
        1.  `i = 1`, 根节点
        2.  `i > 1`, 他的父节点编号为`floor(i/2)`
        3.  `2i <= n`, 他的左子节点编号为`2i`; `2i > n`, 则无左子节点; `2i + 1 <= n `, 他的左子节点编号为`2i + 1`; `2i + 1 > n`, 则无左子节点; 若从`0`开始编号, 对应将`n`换成`n - 1`即可
    4.  已知完全二叉树节点个数`n`, 求叶子节点个数`n0`
        1.  `n = n0 + n1 + n2`
        2.  `n0 = n2 + 1`
        3.  综上, 有`n = 2 * n0 + n1 - 1`, 由于完全二叉树度为1的节点数只可能为0或1, 由此题解
    5.  上述`7.iv`中为了避免使用`%2`以判断`n`的奇偶性, 可做如下优化
        1.  当`n`为偶数时, `n1 = 1`, `n0 = n/2`
        2.  当`n`为奇数时, `n1 = 0`, `n0 = (n + 1) / 2 = n/2 + 1/2`
        3.  假设有一个函数`fn`可以综上以上两种情况, `n0 = fn(n)`, 则`fn`应该为`floor`; 因此书写代码时, 以`java`为例, `if-else + %2`可优化为`n0 = (n + 1) >> 1`, 因为`java`中的运算浮点转整型时默认向下取整. 
        4. 总结: `n0 = floor( (n+1)/2 )`, `n1+n2 = floor( n/2 )`

8. 二叉树的遍历
    1.  先序遍历：拿到根节点就直接访问，然后将其右、左子节点依次入栈
    2.  中序遍历：要遍历完左子树才能遍历根节点，借助一个遍历指针初始指向根节点，循环入栈遍历节点并将指针指向左孩子，直到左孩子为空则弹出栈顶节点进行访问，然后将指针指向该节点的右孩子（如果为空，则继续弹出栈顶节点，直到右孩子不为空或栈为空），进入下一次循环
    3.  后序遍历：根节点要最后访问，将根节点入栈，栈非空循环，瞥一眼栈顶节点，如果栈顶节点是叶子节点或上次访问的节点是该节点的子节点，则弹出栈顶节点并进行访问，否则依序将其非空的右、左子节点入栈
    4.  层序遍历：可借助队列，将根节点入队，队不空循环，出队一个节点进行访问，依序将其不空的左、右子节点入队；如果要求分别每一层有哪些节点，则可借助两个队列或插入null值标识
    5.  翻转二叉树：将二叉树中每个节点的左右子节点交换位置，这相当于对每个节点进行访问操作（交换左右指针的值）