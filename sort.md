---
title: Javascript排序算法
date: 2017-04-19 18:46:36
tags: algorithm
---
* 算法的稳定性
所有相等的数经过算法的排序之后，仍能保持他们在排序之前的相对次序，这种排序算法是稳定的，反之，是不稳定的。
* 算法的时间复杂度
是指执行算法所需要的计算工作量
* 算法的空间复杂度
是指执行算法所需要的内存空间


| 算法     | 时间复杂度             | 空间复杂度 | 稳定性 |
| :----    | :----                  | :----      | :----- |
| 冒泡排序 | O(n) O(n*n)            | O(1)       | 稳定   |
| 选择排序 | O(n*n) O(n*n)          | O(1)       | 不稳定 |
| 插入排序 | O(n) O(n*n)            | O(1)       | 稳定   |
| 希尔排序 | O(n) O(n log^2 n)      | O(1)       | 不稳定 |
| 归并排序 | O(n) O(n log n)        | O(1)       | 稳定   |
| 快速排序 | O(n log n)  O(n log n) | O(n log n) | 不稳定 |

***

### 冒泡排序,是最慢的一种排序
* 原理：比较相邻的数据，根据排序要求进行互换。如按升序排，较大的数会移动的右侧，较小的数会移到左侧。

```javascript
(function(){
    var aa = [];
    var nums = 10;
    for(var i = 0; i< nums; i++){
        aa[i] = Math.floor(Math.random()* nums + 1);
    }
    function swap(arr, index1, index2){
        var tmp = arr[index1];
        arr[index1] = arr[index2];
        arr[index2]= tmp;
    }

    // 冒泡排序
    function bubbleSort(arr){
        var len = arr.length;
        for(var outer = len -1; outer >= 0; outer--) {
            for(var inner = 0; inner < outer; inner ++){
                if(arr[inner] > arr[inner + 1]){
                    swap(arr, inner ,inner + 1);
                }
            }
        }
        return arr;
    }
    var start  = new Date();
    console.log('排序2前', aa);
    bubbleSort(aa);
    console.log('排序后', aa);
    console.log('冒泡排序消耗时间: ', +new Date - start);
})();

```

### 选择排序
原理：外循环将数组元素挨个移动，内循环从循环中选中元素的下一个元素开始比较。
```javascript
(function(){
    var aa = [];
    var nums = 10;
    for(var i = 0; i< nums; i++){
        aa[i] = Math.floor(Math.random()* nums + 1);
    }
    function swap(arr, index1, index2){
        var tmp = arr[index1];
        arr[index1] = arr[index2];
        arr[index2]= tmp;
    }
    function selectionSort(arr){
        var len = arr.length;
        for(var outer = 0; outer < len; outer++){
            for(var inner = outer+1; inner < len; inner++){
                if(arr[outer] > arr[inner]){
                    swap(arr, outer, inner);
                }
            }
        }
    }
    // 选择排序
    var start  = new Date();
    console.log('排序前', aa);
    selectionSort(aa);
    console.log('排序后', aa);
    console.log('选择排序消耗时间: ', +new Date - start);
})();
```

### 插入排序
原理：外循环将数组元素挨个移动，内循环对外循环中的元素和它前面的元素进行比较。
```javascript
(function(){
    var aa = [];
    var nums = 10;
    for(var i = 0; i< nums; i++){
        aa[i] = Math.floor(Math.random()* nums + 1);
    }
    function swap(arr, index1, index2){
        var tmp = arr[index1];
        arr[index1] = arr[index2];
        arr[index2]= tmp;
    }
    function insertSort(arr){
        var len = arr.length;
        for(var outer = 0; outer < len; outer++){
            var inner  = outer;
            while(inner && arr[inner] < arr[inner - 1]) {
                swap(arr, inner ,inner -1);
                inner --;
            }
        }
    }
    // 插入排序
    var start  = new Date();
    console.log('排序前', aa);
    insertSort(aa);
    console.log('排序后', aa);
    console.log('插入排序消耗时间: ', +new Date - start);
})();

```

### 希尔排序
原理：通过定义一个间隔序列，根据间隔序列来比较元素，间隔的值会不断减小。
* 固定间隔
```javascript
(function(){
var aa = [];
var nums = 10;
for(var i = 0; i< nums; i++){
aa[i] = Math.floor(Math.random()* nums + 1);
}
function swap(arr, index1, index2){
var tmp = arr[index1];
arr[index1] = arr[index2];
arr[index2]= tmp;
}
var gaps  = [5, 3,1];
function shellSort(arr){
var len = arr.length;
for(var g = 0; g < gaps.length; g++){
for(var outer = g; outer < len; outer++){
for(var inner = outer; inner >= g && arr[inner] < arr[inner -g]; inner -= g){
swap(arr, inner ,inner-g);
}
}
}
}
var start  = new Date();
console.log('排序前', aa);
shellSort(aa);
console.log('排序后', aa);
console.log('希尔排序消耗时间: ', +new Date - start);
})();

```
* 动态间隔
```javascript
(function(){
    var aa = [];
    var nums = 10;
    for(var i = 0; i< nums; i++){
        aa[i] = Math.floor(Math.random()* nums + 1);
    }
    function swap(arr, index1, index2){
        var tmp = arr[index1];
        arr[index1] = arr[index2];
        arr[index2]= tmp;
    }
    function shellSortByDynamic(arr){
        var len = arr.length;
        var g = 1;
        while(g < len / 3){
            g = 3 * g + 1;
        }

        while(g >= 1){
            for(var outer = g; outer < len; outer++){
                for(var inner = outer; inner >=g && arr[inner] < arr[inner -g]; inner -= g){
                    swap(arr, inner ,inner - g);
                }
            }
            g = (g - 1) / 3;
        }
    }
    // 动态希尔排序
    var start  = new Date();
    console.log('排序前', aa);
    shellSortByDynamic(aa);
    console.log('排序后', aa);
    console.log('动态希尔排序消耗时间: ', +new Date - start);
})();

```

### 归并排序
原理：将数据分解为一组只有一个元素的数组，然后通过创建一组左右子数组将他们慢慢排序合并。
```javascript
(function(){
var aa = [];
var nums = 10;
for(var i = 0; i< nums; i++){
aa[i] = Math.floor(Math.random()* nums + 1);
}
function mergeSort(arr){
var len = arr.length;
if(len < 2) return;
var step = 1;
var left, right;
while(step < len){
left = 0;
right = step;
while(right + step <= len){
mergeArrays(arr, left, left+step, right, right+step);
left = right + step;
right = left + step;
}
if(right < len){
mergeArrays(arr, left, left+step, right, len);
}
step *= 2;
}
}
function mergeArrays(arr, leftStart, leftStop, rightStart, rightStop){
var leftArr = new Array(leftStop - leftStart + 1);
var rightArr = new Array(rightStop - rightStart + 1);
var k = leftStart;
for(var i = 0; i < leftStop-leftStart; i++){
leftArr[i] = arr[k];
k++
}
k = rightStart;
for(var i = 0; i < rightStop-rightStart; i++){
rightArr[i] = arr[k];
k++;
}
leftArr[leftArr.length -1] = Infinity;
rightArr[rightArr.length - 1] = Infinity;
var l = 0;
var r = 0;
for(var i = leftStart; i < rightStop;i++){
if(leftArr[l] <= rightArr[r]){
arr[i] = leftArr[l];
l++
}else {
arr[i] = rightArr[r];
r++;
}
}
}
// 归并排序
var start  = new Date();
console.log('排序前', aa);
mergeSort(aa);
console.log('排序后', aa);
console.log('归并排序消耗时间: ', +new Date - start);
})();

```

### 快速排序
原理：在列表中选择一个元素作为基准值，列表中小于基准值的元素移动到左边，大于基准值的元素移动的右边，递归调用,合并左数组、基准值、有数组并返回。

```javascript
(function(){
    var aa = [];
    var nums = 10;
    for(var i = 0; i< nums; i++){
        aa[i] = Math.floor(Math.random()* nums + 1);
    }
    function quickSort(arr){
        var len = arr.length;
        if(arr && len == 0){
            return [];
        }
        var point = arr[0];
        var left = [];
        var right = [];
        for(var i = 1; i< len; i++){
            if(arr[i] < point){
                left.push(arr[i]);
            }else {
                right.push(arr[i]);
            }
        }
        return quickSort(left).concat(point, quickSort(right));
    }
    // 快速排序
    var start  = new Date();
    console.log('排序前', aa);
    aa = quickSort(aa);
    console.log('排序后', aa);
    console.log('快速排序消耗时间: ', +new Date - start);
})();
```
当数组长度为1000时，以上排序算法消耗时间为
```
JavaScript内置sort排序消耗时间: 3
冒泡排序消耗时间:  4
选择排序消耗时间:  3
插入排序消耗时间:  2
希尔排序消耗时间:  2
动态希尔排序消耗时间:  1
归并排序消耗时间:  2
快速排序消耗时间:  2
```
当数组长度为10000时，以上排序算法消耗时间为
```
JavaScript内置sort排序消耗时间:  16
冒泡排序消耗时间:  180
选择排序消耗时间:  228
插入排序消耗时间:  57
希尔排序消耗时间:  58
动态希尔排序消耗时间:  3
归并排序消耗时间:  4
快速排序消耗时间:  16
```
#### 当数据量较大的时候，希尔排序，归并排序，快速排序，速度较快。
