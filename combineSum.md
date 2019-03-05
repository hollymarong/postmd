---
title: N个数相加的和为M
date: 2018-12-01 16:15:58
tags: algorithm
---

```
var combinationSum = function(candidates, target, n) {
  if (!candidates || !candidates.length) { return []; }
  candidates.sort((a,b) => a - b);
  const solutions = [];
  const findCombos = function(candIdx, subtotal, solution, n) {
    for (let i = candIdx; i < candidates.length; i++) {
      console.log(i, candIdx, subtotal,  solution, n)
      if (subtotal + candidates[i] === target && n == 1) {
        solutions.push(solution.concat(candidates[i]));
      } else if (subtotal + candidates[i] < target && i + 1 < candidates.length && n > 1) {
        findCombos(i + 1, subtotal + candidates[i], solution.concat(candidates[i]), n - 1);

      }
      // while (candidates[i + 1] === candidates[i]) { i++; }
    };
  };
  findCombos(0, 0, [], n);
  return solutions;
};

var result = combinationSum([1,2,3,4,5,6,7,8,9], 6, 2)
console.log('result', result);

```
