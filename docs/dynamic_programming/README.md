
## 303. 区域和检索 - 数组不可变

##### 题目描述

给定一个整数数组  nums，求出数组从索引 i 到 j（i ≤ j）范围内元素的总和，包含 i、j 两点。

实现 NumArray 类：

- NumArray(int[] nums) 使用数组 nums 初始化对象
- int sumRange(int i, int j) 返回数组 nums 从索引 i 到 j（i ≤ j）范围内元素的总和，包含 i、j 两点（也就是 sum(nums[i], nums[i + 1], ... , nums[j])）

##### 示例：

```
输入：
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
输出：
[null, 1, -1, -3]

解释：
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))

```

##### 解题思路

最先想到的方法就是在sumRange去计算nums下标i到j范围内的元素和。但每次检索的时间和检索的下标有关，因此检索的时间复杂度较高，如果检索的次数较多的话，会超出时间限制。因此在初始化的时候进行预处理。

1. 在NumberArray中去创建一个长度为nums.length + 1的sums数组用来存储前i项的和。
2. 长度设为nums.length + 1是为了方便计算sumRange(i, j)。
3. 此时有sumRange(i, j) = sums[j + 1] - sums[i]

**代码**

```javascript
/**
 * @param {number[]} nums
 */
var NumArray = function(nums) {
    const n = nums.length
    this.sums = new Array(n + 1).fill(0)
    for(let i = 0; i < n; i ++) {
        this.sums[i + 1] = this.sums[i] + nums[i]
    }
};

/** 
 * @param {number} i 
 * @param {number} j
 * @return {number}
 */
NumArray.prototype.sumRange = function(i, j) {
    return this.sums[j + 1] - this.sums[i]
};

/**
 * Your NumArray object will be instantiated and called as such:
 * var obj = new NumArray(nums)
 * var param_1 = obj.sumRange(i,j)
 */
```

------
## 304. 二维区域和检索 - 矩阵不可变

##### 题目描述

给定一个二维矩阵，计算其子矩形范围内元素的总和，该子矩阵的左上角为 (row1, col1) ，右下角为 (row2, col2) 。

<table>
  <tr>
    <td>3</td>
    <td>0</td>
    <td>1</td>
    <td>4</td>
    <td>2</td>
  </tr>
  <tr>
    <td>5</td>
    <td>6</td>
    <td>3</td>
    <td>2</td>
    <td>1</td>
  </tr>
  <tr>
    <td>1</td>
    <td bgColor="red">2</td>
    <td bgColor="red">0</td>
    <td bgColor="red">1</td>
    <td>5</td>
  </tr>
  <tr>
    <td>4</td>
    <td bgColor="red">1</td>
    <td bgColor="red">0</td>
    <td bgColor="red">1</td>
    <td>7</td>
  </tr>
  <tr>
    <td>1</td>
    <td bgColor="red">0</td>
    <td bgColor="red">3</td>
    <td bgColor="red">0</td>
    <td>5</td>
  </tr>
</table>

上图子矩阵左上角 (row1, col1) = (2, 1) ，右下角(row2, col2) = (4, 3)，该子矩形内元素的总和为 8。

##### 示例:

```
给定 matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```

##### 解题思路

1. 这题思路与[303. 区域和检索 - 数组不可变](#_303-区域和检索-数组不可变)类似。
2. 同样在NumMatrix中创建一个二维数组用来存储每行的总值，即sums[i][j + 1]为第i行，第j列的值。
3. 然后在sumRegion中循环row1到row2行，而每行的总和则为sums[row][col2 + 1] - sums[row][col1]。（row为某行）
4. 最后sum即为总和。

```javascript
/**
 * @param {number[][]} matrix
 */
var NumMatrix = function(matrix) {
    const m = matrix.length
    if(m > 0) {
        n = matrix[0].length
    }
    this.sums = new Array(m).fill(0).map(() => new Array(n + 1).fill(0))
    for(let i = 0; i < m; i ++) {
        for(let j = 0; j < n; j ++) {
            this.sums[i][j + 1] += this.sums[i][j] + matrix[i][j]
        }
    }
};

/** 
 * @param {number} row1 
 * @param {number} col1 
 * @param {number} row2 
 * @param {number} col2
 * @return {number}
 */
NumMatrix.prototype.sumRegion = function(row1, col1, row2, col2) {
    let sum = 0
    for(let i = row1; i <= row2; i ++) {
        sum += this.sums[i][col2 + 1] - this.sums[i][col1]
    }
    return sum
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * var obj = new NumMatrix(matrix)
 * var param_1 = obj.sumRegion(row1,col1,row2,col2)
 */
```

------


## 338. 比特位计数

##### 题目描述

给定一个非负整数 num。对于 0 ≤ i ≤ num 范围中的每个数字 i ，计算其二进制数中的 1 的数目并将它们作为数组返回。

##### 示例一
```
输入: 2
输出: [0,1,1]
```

##### 示例二
```
输入: 5
输出: [0,1,1,2,1,2]
```

##### 解题思路

**动态规划——最高有效位**

nums|二进制|个数| 结果
:-  |:-    |:- | :-
0   | 0    | 0 |
1   | 01   | 1 | 2^0
2   | 10   | 1 | 2^1
3   | 0011 | 2 | nums[i - 2] + 1
4   | 0100 | 1 | 2^2
5   | 0101 | 2 | nums[i - 4] + 1
6   | 0110 | 2 | nums[i - 4] + 1
7   | 0111 | 3 | nums[i - 4] + 1
8   | 1000 | 1 | 2^3
9   | 1000 | 1 | nums[i - 8] + 1
10  | 1000 | 1 | nums[i - 8] + 1

- 如果 i&(i−1)=0，则令 highBit=i，更新当前的最高有效位。
- i 比 i−highBit 的「一比特数」多 1，由于是从小到大遍历每个数，因此遍历到 i 时，i−highBit 的「一比特数」已知，令bits[i]=bits[i−highBit]+1。

**代码**
```javascript
/**
 * @param {number} num
 * @return {number[]}
 */
var countBits = function(num) {
    const bits = new Array(num + 1).fill(0);
    let highBit = 0;
    for (let i = 1; i <= num; i++) {
        if ((i & (i - 1)) == 0) {
            highBit = i;
        }
        bits[i] = bits[i - highBit] + 1;
    }
    return bits;
};
```

------