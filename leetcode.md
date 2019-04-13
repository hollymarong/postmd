---
title: leetcode
date: 2019-01-21 17:11:41
tags: algorithm
---
1. 整数反转
```
// 示例1:
输入：123
输出：321


// 示例2:
输入：-123
输出：-321

// 示例3：
输入：230
输出：32

integer range: [−231,  231 − 1]
```
代码：
```
function reverse(num) {
  var MIN_VALUE = Math.pow(2, 31) * -1;
  var MAX_VALUE = Math.pow(2, 31) - 1;
  var result = parseInt(Math.abs(num).toString().split('').reverse().join('')) * Math.sign(num);
  return num >= (MIN_VALUE) && num <= MAX_VALUE? result : 0;
}
```
2. 两数之和
在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标
```
示例
nums = [2, 7, 11, 15] target = 9;
nums[0] + nums[1] = 2 + 7 = 9;
return [0, 1];
```
solution:
```
var twoSum = function(nums, target) {
  const result = [];
  const cache = {};
  for(var i = 0; i < nums.length; i++) {
    if (cache.hasOwnProperty(nums[i])) {
      result.push(cache[nums[i]]);
      result.push(i);
    }
    cache[target - nums[i]] = i;
  }
  return result;
};

```
3. 无重复字符的最长子串
```
var lengthOfLongestSubstring = function(s) {
  var res = 0; // 用于存放当前最长无重复子串的长度
  var str = ""; // 用于存放无重复子串
  var len = s.length;
  var end = 0;
  var longestStr = '';
  for(var i = 0; i < len; i++) {
    var char = s.charAt(i);
    var index = str.indexOf(char);
    if(index === -1) {
      str += char;
      res = res < str.length ? str.length : res;
      if (res >= str.length) end = i;
    } else {
      str = str.substr(index + 1) + char;
    }
  }
  longestStr = s.substr(end - res + 1 ,res);
  console.log('str',res, longestStr, end);
  return res;
};

var dd = lengthOfLongestSubstring('pwakepwj');

```

4. 最长回文字符串
```
var longestPalindrome = function(s) {
  var max = '';
  for (var i = 0; i < s.length; i++) {
    for (var j = 0; j < 2; j++) {
      var left = i;
      var right = i + j;
      while (s[left] && s[left] === s[right]) {
        left--;
        right++;
      }
      if ((right - left - 1) > max.length) {
        max = s.substring(left + 1, right);
      }
    }
  }
  return max;
};

const result = longestPalindrome('zabbasbbbbb');
console.log('result', result);
```

5. Z 字形变换
```
```
