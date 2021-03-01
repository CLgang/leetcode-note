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