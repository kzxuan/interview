# 剑指Offer

</br>

### 数组与链表

- 数组</br>
  1. **占据一块连续的内存并按照顺序存储数据**。
  2. 需要预先指定数组大小，空间利用率较差。
  3. 但可依据下标在`O(1)`时间内完成读写操作，时间效率比较高。
  4. 简单hash表可利用数组下标为`key`值，数组中的元素为`value`值，完成`O(1)`时间内的查找。

  > 在合并两个字符串或数组时，可考虑从后向前复制，减少移动次数。

- 链表</br>
  1. 动态数据结构，内存分配不是在创建链表时一次性完成的，空间效率比数组高。
  2. 但查找第`i`个元素的操作需要`O(n)`，在末尾添加元素也需要`O(n)`的时间。

  > 递归容易造成函数调用栈溢出。

</br>

### 二叉树

- 性质：
  1. 二叉树的第`i`层最多有$2^{(i-1)}$个节点；
  2. 深度为`k`的二叉树最多有$2^k-1$个节点（即满二叉树节点数）；
  3. 对于任意一棵二叉树`T`，若叶子节点数为`n0`，度为2的结点数为`n2`，则`n0 = n2 + 1`；
  4. 深度为`k`的**完全二叉树**，至少有$2^{(k-1)}$个节点；
  5. 具有`n`个节点的**非空二叉树**共有`n-1`个分支；
  6. 具有`n`个节点的**非空完全二叉树**的深度为$\lfloor log_2^n \rfloor+1$；
  7. 具有`n0`个叶子节点的完全二叉树中共有$2*n0$个节点或$2*n0-1$个节点；
  8. `n`个节点的**完全二叉树**中：</br>
    度为`0`的节点数为$\lfloor (n+1)/2 \rfloor$；</br>
    度为`1`的结点数为$(n+1)\%2$；</br>
    度为`2`的节点数为$\lfloor (n+1)/2 \rfloor - 1$。

- 前序、中序、后序遍历：

> [!TIP|label:递归和非递归（栈）]
> 时间复杂度`O(n)`，`n`为节点个数，每个节点都被访问了一次。</br>
> 空间复杂度为`O(n)`，`n`为树的深度。

```python
class TreeNode(object):
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def DeepTraverse(root):
    if root:
        # 前序
        print(root.val)
        self.DeepTraverse(root.left)
        # 中序
        print(root.val)
        self.DeepTraverse(root.right)
        # 后序
        print(root.val)
```

- 宽度（广度）优先遍历：

> [!TIP|label:队列]
> 时间复杂度`O(n)`，`n`为节点个数。</br>
> 空间复杂度为`O(w))`，`w`为二叉树的宽度（拥有最多节点的层的节点数）。

```python
class TreeNode(object):
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def BFS(root):
    num_list = []
    root_list = [root]
    while len(root_list) and root:
        root = root_list.pop(0)
        num_list.append(root.val)
        if root.left:
            root_list.append(root.left)
        if root.right:
            root_list.append(root.right)
    return num_list
```

</br>

#### 题3 二维数组中的查找

**题目：**二维数组中，每行从左到右递增，每列从上到下递增，给出一个属，判断它是否在数组中。

**思路：**从左下角或右上角开始比较。

```python
def find_integer(matrix, num):
    """
    :param matrix: [[]]
    :param num: int
    :return: bool
    """
    if not len(matrix):
        return False
    rows, cols = len(matrix), len(matrix[0])
    row, col = 0, cols - 1
    while row < rows and col >= 0:
        if matrix[row][col] == num:
            return True
        elif matrix[row][col] > num:
            col -= 1
        else:
            row += 1
    return False
```

</br>

#### 题4 替换空格

**题目：**把字符串中的空格替换成'20%'。

**方法一：**
```python
s = input()
return s.replace(' ', '%20')
```

**方法二：**
```python
s = input()
res = ''
for i in range(len(s) - 1, -1, -1):
    if s[i] == ' ':
        res = '20%' + res
    else:
        res = s[i] + res
print(res)
```

</br>

#### 题5 从尾到头打印单链表

**题目：**输入一个链表的头节点，从尾到头反过来打印出每个节点的值。

**方法一：**栈
```python
class LinkNode(object):
    def __init__(self, val):
        self.val = val
        self.next = None

def print_links(root):
    stack = []
    while root:
        stack.append(root.val)
        root = root.next
    while stack:
        print(stack.pop())
```

**方法二：**递归
```python
def print_links(root):
    if root:
        print_links(root.next)
        print(root.val)
```

#### 题6 重建二叉树

题目：用前序和中序遍历结果构建二叉树，遍历结果中不包含重复值

思路：前序的第一个元素是根节点的值，在中序中找到该值，中序中该值的左边的元素是根节点的左子树，右边是右子树，然后递归的处理左边和右边

```python
class TreeNode(object):
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def construct_tree(pre, tin):
    if not pre or not tin:
        return None
    index = tin.index(pre[0])
    tin_left, tin_right = tin[0:index], tin[index + 1:]
    pre_left, pre_right = pre[1: 1 + len(tin_left)], pre[-len(tin_right):]
    root = TreeNode(pre[0])
    root.left = construct_tree(pre_left, tin_left)
    root.right = construct_tree(pre_right, tin_right)
    return root
```

#### 题7 二叉树的下一个节点

题目：给定一颗二叉树和其中的一个节点，如何找出中序遍历的下一个节点，树中的节点有左、右子节点指针，还有指向父节点的指针。

思路：分情况：1）当前节点有右子树，则下一个节点为右子树的最左子节点

​                           2）当前节点无右子树，2.1）若当前节点为父节点的左子节点，则下一节点为父节点

​                                                                    2.2）若当前节点为父节点的右子节点，则一直向其父节点寻找，直到找到父节点的右子节点不是当前父节点，则下一个节点为该父节点

```python
class TreeNode(object):
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.parent = None

def findnextnode(node):
    if node is None:
        return None
    if node.right:
        pright = node.right
        while pright.left:
            pright = pright.left
        return pright
    elif node.parent and node == node.parent.left:
        return node.parent
    elif node.parent and node == node.parent.right:
        pparent = node.parent
        while pparent.parent and pparent.parent.right == pparent:
            pparent = pparent.parent
        return pparent.parent
    return None

```

#### 题8 用两个栈实现队列

题目：用两个栈实现队列，分别实现入队和出队操作

思路：一个栈负责入队，另一个负责出队，出栈为空则从入栈中导入到出栈中

```python
class MyQueue(object):
    def __init__(self):
        self.enstack = []
        self.destack = []
    def push(self, val):
        self.enstack.append(val)
    def pop(self):
        if self.destack:
            return self.destack.pop()
        while self.enstack:
            self.destack.append(self.enstack.pop())
        return self.destack.pop() if self.destack else None
```

#### 题9 旋转数组的最小数字

题目：把递增数组的前面部分数字移到队尾，求数组中的最小值，例如[3,4,5,6,1,2]

思路：使用二分法O(logn)，但要考虑[1,0,0,1]这种数据，只能顺序查找

```python
def findmin(datalist):
    if len(datalist) < 1:
        return None
    left, right = 0, len(datalist) - 1
    middle = left
    while datalist[left] >= datalist[right]:
        if right - left == 1:
            middle = right
            break
        middle = (left + right) // 2
        if datalist[left] == datalist[middle] == datalist[right]:
            return min(datalist)
        if datalist[middle] >= datalist[left]:
            left = middle
        elif datalist[middle] <= datalist[right]:
            right = middle
    return datalist[middle]

```

#### 题10 矩阵中的路径

题目：请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。

思路：回溯法。定义一个布尔型矩阵标识该格子中的元素是否已经在路径中。

```python
def haspath(matrix, rows, cols, path):
    if len(matrix) < 1 or rows < 1 or cols < 1 or path == '':
        return False
    visited = [0] * cols * rows
    pathlength = 0
    for row in range(rows):
        for col in range(cols):
            if haspathcore(matrix, row, col, rows, cols, pathlength, path):
                return True
    del visited
    return False
def haspathcore(matrix, row, col, rows, cols, pathlength, path):
    if pathlength >= len(path):
        return True
    hasPath = False
    if 0<= row < rows and 0<= col < cols and matrix[row * cols + col] == path[pathlength] and visited[row * cols + col] == 0:
        pathlength += 1
        visited[row * cols + col] = 1
        hasPath = haspathcore(matrix, row + 1, col, rows, cols, pathlength, path) or haspathcore(matrix, row - 1, col, rows, cols, pathlength, path) or haspathcore(matrix, row, col + 1, rows, cols, pathlength, path) or haspathcore(matrix, row, col - 1, rows, cols, pathlength, path)
        if not hasPath:
            pathlength -= 1
            visited[row * cols + col] = 0
    return hasPath
```

#### 题11 机器人的运动范围

题目：地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 请问该机器人能够达到多少个格子？

思路：回溯法。定义一个布尔型矩阵标识该格子中的元素是否已经被访问过。

```python
def movingcount(threshold, rows, cols):
    if threshold < 0 or rows <= 0 or cols <= 0:
        return 0
    visited = [0] * cols * rows
    count = movingcountcore(visited, rows, cols, 0, 0, threshold)
    del visited
    return count
def movingcountcore(visited, rows, cols, row, col, threshold):
    count = 0
    if 0 <= row < rows and 0 <= col < cols and visited[row * cols + col] == 0 and (digitsum(row) + digitsum(col)) <= threshold:
        visited[row * cols + col] = 1
        count = 1 + movingcountcore(visited, rows, cols, row - 1, col, threshold) + movingcountcore(visited, rows, cols, row + 1, col, threshold) + movingcountcore(visited, rows, cols, row, col - 1, threshold) + movingcountcore(visited, rows, cols, row, col + 1, threshold)
    return count
def digitsum(num):
    ssum = 0
    while num:
        ssum += num % 10
        num //= 10
    return ssum
```

#### 题12 剪绳子

题目：给你一根长度为n的绳子，请把绳子剪成m段（m、n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1],...,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18

方法一：动态规划

```python
def maxproductaftercutting(length):
    if length < 2:
        return 0
    if length == 2:
        return 1
    if length == 3:
        return 2
    products = [0, 1, 2, 3]
    for i in range(4, length + 1):
        mmax = 0
        for j in range(1, i // 2 + 1):
            mmax = max(mmax, product[j] * product[i - j])
            products.append(mmax)
    mmax = products[length]
    del products
    return mmax
```

方法二：贪心

```python
def maxproductaftercutting(length):
    if length < 2:
        return 0
    if length == 2:
        return 1
    if length == 3:
        return 2
    timeof3 = length // 3
    if length - timeof3 * 3 == 1:
        timeof3 -= 1
    timeof2 = (length - timeof3 * 3) // 2
    return (3 ** timeof3) * (2 ** timeof2)
```

#### 题13 二进制中1的个数

题目：输入一个整数，输出该数二进制表示中1的个数。

【(n - 1) & n得到的结果，相当于把n的二进制标是中最右边的1变为0】

方法一：左移方法，通过将1左移后与n做与运算判断当前位置是否为1

```python
def numberof1(n):
    count, flag = 0, 1
    if n < 0:
        n = n & 0xFFFFFFFF
    for i in range(32): # 32位
        if n & flag:
            count += 1
        flag = flag << 1
    return count
```

方法二：利用与运算，(n - 1) & n将最右侧为1的位置的值，改为0

```python
def numberof1(n):
    count = 0
    if n < 0:
        n = n & 0xFFFFFFFF
    while n:
        count += 1
        n = (n - 1) & n
    return count
```

扩展：输入两个整数m, n，计算需要改变m的二进制表示中的多少位才能得到n。

思路：1）求m和n的异或，m^n。2）统计异或结果中中1的个数

```python
def numberof1(m, n):
    count = 0
    res = m ^ n
    if res < 0:
        res = res & 0xFFFFFFFF
    while res:
        count += 1
        res = (res - 1) & res
    return count
```

#### 题14 数值的整数次方

题目：给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

思路：需要考虑次方是正数、负数和0，基数是0

【除以2运算可以通过右移一位运算代替，求奇偶数的%2运算，可以通过n&0x1==1为奇数代替】

```python
InvalidInput = False

def Power(base, exponent):
    InvalidInput = False
    if base == 0.0 and exponent < 0:
        InvalidInput = True
        return 0
    absexponent = exponent if exponent >= 0 else -exponent
    res = UnsignedPower(base, absexponent)
    if exponent < 0:
        res = 1.0 / res
    return res
# method 1
def UnsignedPower(base, exponent):
    if exponent == 0:
        return 1
    if exponent == 1:
        return base
    res = 1.0
    for i in range(1, exponent + 1):
        res *= base
    return res
# method 2 O(logn)
def UnsignePower(base, exponent):
    if exponent == 0:
        return 1
    if exponent == 1:
        return base
    res = UnsignedPower(base, exponent >> 1)
    res *= res
    if exponent & 0x1:
        res *= base
    return res
```

#### 题15 打印从1到最大的n位数

题目：输入数字n，按顺序打印从1到最大的n位十进制数。如输入3，打印1，2，3到最大的三位数999。

```python
def generate(n):
    if n <= 0:
        return
    number = [0] * n + [1]
    while not number[0]:
        printnum(number)
        add, pos = 1, n
        while add:
            if number[pos] + add > 9:
                number[pos] = 0
                pos -= 1
            else:
                number[pos] += 1
                add = 0
def printnum(number):
    res = ''
    signal = True
    for i in range(len(number)):
        if signal and number[i] != 0:
            signal = False
        if not signal:
            res += str(number[i])
    print(res)
```

#### 题16 在O(1)时间内删除链表中节点

题目：给定单向链表的头指针和一个节点指针，定义一个函数在O(1)时间内删除该节点

思路：如果待删除结点指针有后继节点，则将后继节点值前移后删除后继节点；否则顺序查找，删除待删除节点指针

```python
class LinkNode(object):
    def __init__(self, val):
        self.val = val
        self.next = None
def DeleteNode(pListHead, pToBeDeleted):
    if pToBeDeleted.next:
        pnext = pToBeDeleted.next
        pToBeDeleted.val = pnext.val
        pToBeDeleted.next = pnext.next
        del pnext
    elif pListHead == pToBeDeleted:
        pListHead = None
        del pToBeDeleted
    else:
        pNode = pListHead
        while pNode.next != pToBeDeleted:
            pNode = pNode.next
        pNode.next = None
        del pToBeDeleted
    return pListHead

```

#### 题17 删除链表中重复的节点

题目：排序的链表中，删除重复的节点。

思路：在链表头节点添加一个值为空的节点，连续三个节点的值进行比较。

```python
class LinkNode(object):
    def __init__(self, val):
        self.val = val
        self.next = None
def DeleteDuplication(pHead):
    p0 = ListNode(None)
    p0.next = pHead
    p1, p2 = p0, p0
    while p2.next and p2.next.next:
        if p2.val != p2.next.val != p2.next.next.val:
            p1.next = p2.next
            p1 = p1.next
        p2 = p2.next
    if p2.next and p2.val != p2.next.val:
        p1.next = p2.next
    else:
        p1.next = None
    return p0.next
```

 #### 题18 模式匹配字符串

题目：请实现一个函数用来匹配包括'.'和'\*'的正则表达式。模式中的字符'.'表示任意一个字符，而'\*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab\*ac\*a"匹配，但是与"aa.a"和"ab\*a"均不匹配

思路：若s和pattern都是空字符，return True；

​           若s非空，pattern为空，return False；

​           若pattern[1]为"\*"：若pattern[0] == '.' or pattern[0] == s[0]，则1）s不变，pattern后移2位，2）s后移1位，pattern后移2位，3）s后移1位，pattern不变。否则，s不变，pattern后移2位。

​           若pattern[1]不为"\*"：若pattern[0] == '.' or pattern[0] == s[0]，则s后移1位，pattern后移1位。否则 return False

```python
# -*- coding:utf-8 -*-
class Solution:
    # s, pattern都是字符串
    def match(self, s, pattern):
        # write code here
        if s == '' and pattern == '':
            return True
        elif pattern == '':
            return False
        if len(pattern) > 1 and pattern[1] == "*":
            if s != '' and (pattern[0] == "." or pattern[0] == s[0]):
                return self.match(s[1:], pattern[2:]) or self.match(s[1:], pattern) or self.match(s, pattern[2:])
            else:
                return self.match(s, pattern[2:])
        if s != '' and (pattern[0] == "." or pattern[0] == s[0]):
            return self.match(s[1:], pattern[1:])
        return False
```

#### 题19 判断字符串是否表示数值

题目：请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串 "+100", "5e2", "-123", "3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

思路：字符串遵循模式A\[.\[B\]\]\[e|EC\]或.B\[e|EC]。其中A和C可以为"+"或"-"开头的0~9的字符串，B为0~9的字符串。

```python
# -*- coding:utf-8 -*-
class Solution:
    # s字符串
    def isNumeric(self, s):
        # write code here
        if s == '':
            return False
        postion = 0
        numeric, postion = self.signedinteger(s, postion)
        if postion < len(s) and s[postion] == '.':
            if postion + 1 < len(s):
                temp, postion = self.unsignedinteger(s, postion + 1)
                numeric = numeric or temp

        if postion < len(s) and s[postion] in ['e', 'E']:
            if postion + 1 < len(s):
                temp, postion = self.signedinteger(s, postion + 1)
                numeric = numeric and temp
            else:
                numeric = False
        return numeric and postion >= len(s) - 1

    def signedinteger(self, s, postion):
        if postion < len(s) and s[postion] in ['+', '-']:
            postion += 1
        return self.unsignedinteger(s, postion)

    def unsignedinteger(self, s, postion):
        signal = False
        while postion < len(s) and 0 <= int(s[postion]) <= 9:
            signal, postion = True, postion + 1
        return signal, postion
```

#### 题20 调整数组顺序使奇数位于偶数前面

题目：输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

思路：使用两个指针，前后各一个，为了更好的扩展性，可以把判断奇偶部分抽取出来。