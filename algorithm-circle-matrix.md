---
title: 回形矩阵算法
date: 2017-06-07 09:38:01
tags: algorithm
---
1   2  3 4
12 13 14 5
11 16 15 6
10 9  8  7
解决思路：
按圈进行循环,一个圈的循环包括从左上-右上，右上-右下，右下-左下，左下到左上,完成一圈数值的设定

```javascript
/**
 * @param { Number } m, 矩阵的宽
 * @param { Number } n，矩阵的长
 * @param { Number } initialValue，初始值
 * @returns { Array } 数组
 */
function buildCyclicRectangle(m, n, initialValue) {

    var result = [],
        startM = 0,
        endM = m-1,
        startN = n-1,
        endN = 0,
        num = initialValue,
        i;

    for(var i = 0; i < n; i++){
        result[i] = [];
    }

    var isMLessEqualN = m <= n;
    if(!m || !n) return [];
    if(m == n && (m == 1)) return [[initialValue]];

    //当矩阵的m<=n时，循环圈数根据m的值来判断
    //当矩阵的m>n时，循环圈数根据n的值来判断
    while(isMLessEqualN && (startM < endM) || !isMLessEqualN &&(endN < startN)){
        for(i = startM; i <= endM; i++){
            result[startM][i] = num;
            num++;
        }
        for(i = endN+1; i <= startN; i++){
            result[i][endM] = num;
            num++;
        }
        for(i = endM-1; i>= startM; i--){
            result[startN][i] = num;
            num++;
        }
        for(i = startN-1; i > endN; i--){
            result[i][startM] = num;
            num++;
        }
        startM++;
        endM--;
        startN--;
        endN++;
    }
    //当存在奇数列的情况
    if(isMLessEqualN && startM == endM){
        for(i = endN; i <= startN;i++){
            result[i][startM] = num;
            num++;
        }
    }
    //当存在奇数行的情况
    if(!isMLessEqualN && startN == endN){
        for(i = startM; i <= endM; i++){
            result[startN][i] = num;
            num++;
        }
    }
    return result;
}
var result = buildCyclicRectangle(10,10,1);
console.log(result);
```
