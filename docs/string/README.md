## 14. 最长公共前缀
##### 题目描述

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。
##### 示例一
```
输入：strs = ["flower","flow","flight"]
输出："fl"
```
##### 示例二
```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

##### 解题思路

逐位比较，比较全部通过时result增加当前字符，不通过时直接返回result。

```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    let result = ''
    if(strs.length === 0) {
        return result
    }
    const str = strs[0]
    for(let i = 0; i < str.length; i ++) {
        for(let j = 0; j < strs.length; j ++) {
            if(strs[j][i] !== str[i]) {
                return result
            }
        }
        result += str[i]
    }
    return result
};
```