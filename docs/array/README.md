## 1.两数之和

##### 题目描述：

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

你可以按任意顺序返回答案。

##### 示例 1：

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

##### 示例 2：

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

##### 示例 3：

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

##### 提示：
+ 2 <= nums.length <= 103
+ -109 <= nums[i] <= 109
+ -109 <= target <= 109
+ 只会存在一个有效答案

##### 题解：
**方法一：暴力枚举**

最容易想到的方法是枚举数组中的每一个数 x，寻找数组中是否存在 target - x。

当我们使用遍历整个数组的方式寻找 target - x 时，需要注意到每一个位于 x 之前的元素都已经和 x 匹配过，因此不需要再进行匹配。而每一个元素不能被使用两次，所以我们只需要在 x 后面的元素中寻找 target - x。

时间复杂度：O(n<sup>2</sup>)
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  for(var i = 0; i < nums.length - 1; i ++) {
    var dif = target - nums[i]
    for (var j = i + 1; j < nums.length; j ++) {
      if(nums[j] === dif) {
          return [i, j]
      }
    }
  }
  return []
};
```
**方法二：哈希表**

我们创建一个哈希表，对于每一个 x，我们首先查询哈希表中是否存在 target - x，然后将 x 插入到哈希表中，即可保证不会让 x 和自己匹配。

时间复杂度：O(n)
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  var numMap = {}
  for(var i = 0; i < nums.length; i ++) {
      var dif = target - nums[i]
      if(numMap[dif] !== undefined) {
          return [numMap[dif], i]
      }
      numMap[nums[i]] = i
  }
  return []
};
```

------

## 15.三数之和

##### 题目描述：

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

##### 示例 1：

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```

##### 示例 2：

```
输入：nums = []
输出：[]
```

##### 示例 3：

```
输入：nums = [0]
输出：[]
```

##### 提示：
+ 0 <= nums.length <= 3000
+ -105 <= nums[i] <= 105

##### 题解：
**方法一：暴力解法**

题目中要求找到所有「不重复」且和为 0 的三元组，这个「不重复」的要求使得我们无法简单地使用三重循环枚举所有的三元组。这是因为在最坏的情况下，数组中的元素全部为 0，即
```javascript
[0, 0, 0, 0, 0, ..., 0, 0, 0]
```
任意一个三元组的和都为 0。如果我们直接使用三重循环枚举三元组，会得到 O(n<sup>3</sup>)个满足题目要求的三元组（其中 NN 是数组的长度）时间复杂度至少为 O(n<sup>3</sup>)。在这之后，我们还需要使用哈希表进行去重操作，得到不包含重复三元组的最终答案，又消耗了大量的空间。这个做法的时间复杂度和空间复杂度都很高，因此我们要换一种思路来考虑这个问题。

时间复杂度：O(n<sup>3</sup>)

**方法二：排序 + 双指针**

+ 利用排序避免重复答案
+ 利用双指针找到所有解

时间复杂度:
+ 排序O(nLogn)
+ 搜索解O(n<sup>2</sup>) 

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
  nums.sort((a, b) => a - b) // 升序

  const result = []
  const len = nums.length

  for(let i = 0; i < len - 2; i ++) {
    if(nums[i] === nums[i - 1]) { // 有连续相同的值，直接跳过
      continue
    }
    let left = i + 1
    let right = len - 1

    while(left < right) {
      let sum = nums[i] + nums[left] + nums[right]
      if(sum === 0) {
        // 此处使用后++ --做双指针+-，因为此处等式成立，需同时给左指针+1，右指针-1
        result.push([nums[i], nums[left++], nums[right--]])
        while(nums[left] === nums[left - 1]) { // 有连续相同的值，一样跳到下一个
          left ++
        }
        while(nums[right] === nums[right + 1]) { // 有连续相同的值，跳到上一个
          right --
        }
      } else {
        if(sum < 0) {
          left ++
        } else {
          right --
        }
      }
    }
  }
  return result
};
```

------

## 18.四数之和

##### 题目描述：

给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

##### 示例：

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```
##### 题解：
解法与[三数之和](三数之和.md)相似，都是用排序+双指针的方法，只是多了一层循环。

**思路与算法**
最朴素的方法是使用四重循环枚举所有的四元组，然后使用哈希表进行去重操作，得到不包含重复四元组的最终答案。假设数组的长度是 nn，则该方法中，枚举的时间复杂度为 O(n<sup>4</sup>)，去重操作的时间复杂度和空间复杂度也很高，因此需要换一种思路。

为了避免枚举到重复四元组，则需要保证每一重循环枚举到的元素不小于其上一重循环枚举到的元素，且在同一重循环中不能多次枚举到相同的元素。

为了实现上述要求，可以对数组进行排序，并且在循环过程中遵循以下两点：

每一种循环枚举到的下标必须大于上一重循环枚举到的下标；

同一重循环中，如果当前元素与上一个元素相同，则跳过当前元素。

使用上述方法，可以避免枚举到重复四元组，但是由于仍使用四重循环，时间复杂度仍是 O(n<sup>4</sup>)。注意到数组已经被排序，因此可以使用双指针的方法去掉一重循环。

使用两重循环分别枚举前两个数，然后在两重循环枚举到的数之后使用双指针枚举剩下的两个数。假设两重循环枚举到的前两个数分别位于下标 ii 和 jj，其中 i<ji<j。初始时，左右指针分别指向下标 j+1j+1 和下标 n-1n−1。每次计算四个数的和，并进行如下操作：

如果和等于 *target*，则将枚举到的四个数加到答案中，然后将左指针右移直到遇到不同的数，将右指针左移直到遇到不同的数；

如果和小于 *target*，则将左指针右移一位；

如果和大于 *target*，则将右指针左移一位。

使用双指针枚举剩下的两个数的时间复杂度是 O(n)O(n)，因此总时间复杂度是 O(n<sup>3</sup>)，低于 O(n<sup>4</sup>)。

具体实现时，还可以进行一些剪枝操作：

在确定第一个数之后，如果 *nums[i]+nums[i+1]+nums[i+2]+nums[i+3] > target*，说明此时剩下的三个数无论取什么值，四数之和一定大于 *target*，因此退出第一重循环；

在确定第一个数之后，如果 *nums[i]+nums[n-3]+nums[n-2]+nums[n-1] < target*，说明此时剩下的三个数无论取什么值，四数之和一定小于 *target*，因此第一重循环直接进入下一轮，枚举 *nums[i+1]* ；

在确定前两个数之后，如果 *nums[i]+nums[j]+nums[j+1]+nums[j+2] > target*，说明此时剩下的两个数无论取什么值，四数之和一定大于 *target*，因此退出第二重循环；

在确定前两个数之后，如果 *nums[i]+nums[j]+nums[n-2]+nums[n-1] < target*，说明此时剩下的两个数无论取什么值，四数之和一定小于 *target*，因此第二重循环直接进入下一轮，枚举 *nums[j+1]* 。

**代码**
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[][]}
 */
var fourSum = function(nums, target) {
    let result = []
    const len = nums.length
    if(len < 4) {
        return result
    }
    nums.sort((a, b) => a - b)
    for(let i = 0; i < len - 3; i ++) {
        if(nums[i] === nums[i - 1]) {
            continue
        }
        if(nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3] > target) {
            break
        }
        if(nums[i] + nums[len - 1] + nums[len - 2] + nums[len - 3] < target) {
            continue
        }
        for(let j = i + 1; j < len - 2; j ++) {
            if(j > i + 1 && nums[j] === nums[j - 1]) {
                continue
            }
            if (nums[i] + nums[j] + nums[j + 1] + nums[j + 2] > target) {
                break
            }
            if (nums[i] + nums[j] + nums[len - 2] + nums[len - 1] < target) {
                continue
            }
            let left = j + 1
            let right = len - 1
            while(left < right) {
                let sum = nums[i] + nums[j] + nums[left] + nums[right]
                if(sum === target) {
                    result.push([nums[i], nums[j], nums[left++], nums[right--]])
                    while(nums[left] === nums[left - 1]) {
                        left ++
                    }
                    while(nums[right] === nums[right + 1]) {
                        right --
                    }
                } else {
                    if(sum < target) {
                        left ++
                    } else {
                        right --
                    }
                }
            }
        }
    }
    return result
};
```

------

## 697.数组的度

##### 题目描述：

给定一个非空且只包含非负数的整数数组 nums，数组的度的定义是指数组里任一元素出现频数的最大值。

你的任务是在 nums 中找到与 nums 拥有相同大小的度的最短连续子数组，返回其长度。

##### 示例 1：

```
输入：[1, 2, 2, 3, 1]
输出：2
解释：
输入数组的度是2，因为元素1和2的出现频数最大，均为2.
连续子数组里面拥有相同度的有如下所示:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
最短连续子数组[2, 2]的长度为2，所以返回2.
```

##### 示例 2：

```
输入：[1,2,2,3,1,4,2]
输出：6
```

**提示**
- nums.length 在1到 50,000 区间范围内。
- nums[i] 是一个在 0 到 49,999 范围内的整数。

##### 题解：

**哈希表方法**

记原数组中出现次数最多的数为 *x*，那么和原数组的度相同的最短连续子数组，必然包含了原数组中的全部 *x*，且两端恰为 *x* 第一次出现和最后一次出现的位置。

因为符合条件的 *x* 可能有多个，即多个不同的数在原数组中出现次数相同。所以为了找到这个子数组，我们需要统计每一个数出现的次数，同时还需要统计每一个数第一次出现和最后一次出现的位置。

在实际代码中，我们使用哈希表实现该功能，每一个数映射到一个长度为 *3* 的数组，数组中的三个元素分别代表这个数出现的次数、这个数在原数组中第一次出现的位置和这个数在原数组中最后一次出现的位置。当我们记录完所有信息后，我们需要遍历该哈希表，找到元素出现次数最多，且前后位置差最小的数。

**代码**
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var findShortestSubArray = function(nums) {
    const numsMap = {}

    for(let [i, num] of nums.entries()) {
        if(numsMap[num] !== undefined) {
            numsMap[num][0] ++
            numsMap[num][2] = i
        } else {
            numsMap[num] = [1, i, i]
        }
    }

    let maxCount = 0, minLen = 0

    for (let [count, start, end] of Object.values(numsMap)) {
        let len = end - start + 1
        if(maxCount < count) {
            maxCount = count
            minLen = len
        } else if (maxCount === count) {
            if(minLen > len) {
                minLen = len
            }
        }
    }

    return minLen
};
```

------

## 766.托普利茨矩阵

##### 题目描述：

给你一个 m x n 的矩阵 matrix 。如果这个矩阵是托普利茨矩阵，返回 true ；否则，返回 false 。

如果矩阵上每一条由左上到右下的对角线上的元素都相同，那么这个矩阵是 托普利茨矩阵 。

##### 示例 1：

1|2|3|4
--|:--:|:--:|:--:
5|1|2|3
9|5|1|2

```
输入：matrix = [[1,2,3,4],[5,1,2,3],[9,5,1,2]]
输出：true
解释：
在上述矩阵中, 其对角线为: 
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]"。 
各条对角线上的所有元素均相同, 因此答案是 True 。
```

##### 示例 2：

1|2
--|:--:
2|2

```
输入：matrix = [[1,2],[2,2]]
输出：false
解释：
对角线 "[1, 2]" 上的元素不同。
```

##### 提示：
- m == matrix.length
- n == matrix[i].length
- 1 <= m, n <= 20
- 0 <= matrix[i][j] <= 99

##### 题解：

根据定义，当且仅当矩阵中每个元素都与其左上角相邻的元素（如果存在）相等时，该矩阵为托普利茨矩阵。因此，我们遍历该矩阵，将每一个元素和它左上角的元素相比对即可，所以循环下标从1开始。

复杂度：O(mn) *m* 为矩阵的行数，*n* 为矩阵的列数。 

**代码**
```javascript
/**
 * @param {number[][]} matrix
 * @return {boolean}
 */
var isToeplitzMatrix = function(matrix) {
    const m = matrix.length, n = matrix[0].length
    for(let i = 1; i < m; i ++) {
        for(let j = 1; j < n; j ++) {
            if(matrix[i][j] !== matrix[i - 1][j - 1]) {
                return false
            }
        }
    }
    return true
};
```

------

## 1004.最大连续1的个数III

##### 题目描述：

给定一个由若干 0 和 1 组成的数组 A，我们最多可以将 K 个值从 0 变成 1 。

返回仅包含 1 的最长（连续）子数组的长度。

##### 示例 1：

```
输入：A = [1,1,1,0,0,0,1,1,1,1,0], K = 2
输出：6
解释： 
[1,1,1,0,0,1,1,1,1,1,1]
粗体数字从 0 翻转到 1，最长的子数组长度为 6。
```

##### 示例 2：

```
输入：A = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], K = 3
输出：10
解释：
[0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
粗体数字从 0 翻转到 1，最长的子数组长度为 10。
```

##### 提示：
1. 1 <= A.length <= 20000
2. 0 <= K <= A.length
3. A[i] 为 0 或 1 

##### 题解：
**滑动窗口算法**

滑动窗口中用到了左右两个指针，它们移动的思路是：以右指针作为驱动，拖着左指针向前走。右指针每次只移动一步，而左指针在内部 while 循环中每次可能移动多步。右指针是主动前移，探索未知的新区域；左指针是被迫移动，负责寻找满足题意的区间。

**代码**
```javascript
/**
 * @param {number[]} A
 * @param {number} K
 * @return {number}
 */
var longestOnes = function(A, K) {
    let left = 0, right = 0, count = 0, sum = 0
    while (right < A.length) {
        if(A[right] === 0) {
            count ++ // 统计0的个数
        }
        right ++  // 移动右端点
        while (count > K) {
            if(A[left] === 0) {
                count --
            }
            left ++ // 移动左端点
        }
        sum = Math.max(sum, right - left)
    }
    return sum
};
```