---
title: 正则表达式 
date: 2016-06-07 11:45:08
tags:
---
* String.prototype.match()
* String.prototype.replace()

* RegExp.prototype.exec() 
* RegExp.prototype.test()

## 元字符
| 字符   | 含义                                                                   |
| :----  | :----                                                                  |
| ^      | 匹配字符串的开头，多行检索中，匹配一行的开头                           |
| $      | 匹配字符串的结尾，多行检索中，匹配一行的结尾                           |
| \b     | 匹配一个单词的边界                                                     |
| \B     | 匹配而非单词的边界                                                     |
| (?=p)  | 零宽正向先行断言，要求接下来的字符都与p匹配，但不能包括匹配p的那些字符 |
| (?!p)  | 零宽负向先行断言，要求接下来的字符不与p匹配                            |
| (?<=p) |  javscript不支持                                                          |
| (?<!p) |  javascript不支持                                                         |
| (?>p)  | 固化分组, javascript不支持                                                    |


## 量词
| 字符 | 含义        |
| :----  | :----         |
| ?      | 0次货1次      |
| +      | 一次或多次    |
| *      | 0次或多次     |
| {n}   | n次  |
| {n,}  | n次或更多次 |
| {n, m} | 次数在n,m之间,至少n次，但不能超过m次  |

### .
. 不能匹配回车和换行\n, \r
### 匹配包含p, 但不包含h的所有单词
```javascript
var web_development = "python php ruby javascript jsonp perhapsphpisoutdated";
var reg = /\bp(?!h)\w*\b/g;
web_development.match(reg); //["python", "perhapsphpisoutdated"]

```
### 零宽断言

js 中，对于四种零宽断言，只支持 零宽度正预测先行断言 和 零宽度负预测先行断言 这两种。

js 中，正则表达式后面可以跟三个 flag，比如 /something/igm。

他们的意义分别是，

i 的意义是不区分大小写
g 的意义是，匹配多个
m 的意义是，是 ^ 和 $ 可以匹配每一行的开头。

javascript不支持“零宽度正回顾后发断言”

### (?: 子表达式)
- 定义非捕获组,只分组不捕获
```
Write(?:Line)
var reg = /Write(?:Line)/;
var dd = 'Console.WriteLine()';
dd.match(reg)  //['WriteLine']

var reg = /Write(Line)/;
var dd = 'Console.WriteLine()';
dd.match(reg)  //['WriteLine', 'Line'];

```
### (?= 子表达式)
- 仅当子表达式在此位置的右侧匹配时才继续匹配。例如，\w+(?=\d) 与后跟数字的单词匹配，而不与该数字匹配
```
var ss = /^\W*(?=空气质量)|[^>]*(?=<\/b>)/g;
var dd = '北京空气质量：<b>90</b>';
var rr = dd.match(ss); //["北京", "90", ""]
```

### (?<= 子表达式)

- （零宽度正回顾后发断言。） 仅当子表达式在此位置的左侧匹配时才继续匹配。例如，(?<=19)99 与跟在 19 后面的 99 的实例匹配。
```javascript
var str = 'Homes Tea is good, hometown is beautiful';
var reg = /(?<=\bhome)(?=s\b)/gi;
var reg = /(?=s\b)(?<=\bhome)/gi;//位置变化没有影响
str.replace(reg, "'");
```
格式化数字
```javascript
var num = '1234876';
var reg = /(?<=\d)(?=(\d\d\d)+$)/gi;
num.replace(reg, ',');

var num = 'hello, here is 1234876 str';
var reg = /(?<=\d)(?=(\d\d\d)+\b)/gi;
num.replace(reg, ',');

var num = 'hello, here is 1234876kg str';
var reg = /(?<=\d)(?=(\d\d\d)+(?!\d))/gi;
num.replace(reg, ',');
```
匹配空格间隔的字符串
```javascript

var cls="item current";
var reg = /(?<=\s*)([^\s]+)(?=\s*)/;
cls.match(reg);
``` 


1.[...]字符组
一个字符组只能匹配目标文本的单个字符，在字符组内部，只有连字符才是元字符（如果连字符出现在字符组的开头，只表示一个普通的字符），?.等元字符在字符组内部就是一个普通字符, \在字符组内需要转义。
```javascript
//真正的特殊字符只有两个连字符
var reg = /[1-9A-Z_!.?]/;
```
2. | 或，子表达式称为多选分支
```
//匹配gray，grey
var reg = /gray|grey/;
var reg = /gr(a|e)y/;
var reg = /gr[ae]y/;
```
3.反向引用
匹配与表达式先前部分匹配的同样的文本。在一个表达式中，可以使用多个括号，\1,\2,\3等用来表示第一，第二，第三组括号匹配的文本。括号是按照开括号(从左到右的出现顺序进行的。
```
//匹配字符串中重复出现的单词
var ss = 'hello world  world;
var reg = /\b([A-Za-z]+) +\1\b/;
reg.exec(ss)
```

## 例子
* 匹配引号内的字符串
```
var ss = 'Some on said "Programing is fun"';
var reg = /"[^"]*"/;
var result = reg.exec(ss);
```
* 匹配时间
```
//9:17 am 12:30pm
var reg = /(0?[1-9]|1[012]):[0-5][0-9] *(a|m)p/;
//09:59 24小时制
var reg = /([01]?[0-9]|2[0-3]):[0-5][0-9]/;
```

### 正则表达式的匹配原则
* 量词? * + {min, max}都是贪婪匹配的，尽可能多的匹配字符，直到达到匹配上限为止。
* 量词?? *? +? {min, max}?都是懒惰匹配的，尽可能少的匹配字符，只需满足匹配下限。
* 占有优先量词: ?+ *+ ++ {min, max}+都是贪婪匹配的，在匹配的过程中不会交还任何匹配到的字符(javascript不支持)。
* 先来先服务原则
先满足先来的量词的贪婪匹配，后来的量词满足最基本的匹配.
.*会贪婪匹配整个字符串,只有在全局匹配需要的情况下，才会被迫交出一些字符
为了满足\d{4}的匹配，强迫.\*从右到左交还字符（保证.\*匹配的最基本的条件），直到满足匹配，下面能匹配2017.
```javascript
var str = "this year is 2017";
var reg = /^.*+(\d{4})/;
var result = reg.exec(str);
result[1] //2017
```
下面的结果只能匹配到7,根据先来先服务的原则，满足\d+最基本的匹配一次
```javascript
var str = "this year is 2017";
var reg = /^.*(\d+)/;
var result = reg.exec(str);
result[1] //7
```
下面的结果什么都匹配不到,根据先来先服务的原则，满足\d*最基本的匹配0次
```javascript
var str = "this year is 2017";
var reg = /^.*(\d*)/;
reg.exec(str);
var result = reg.exec(str);
result[1] //''
```
### 排除类和排除环视

```javascript
var str = 'Tom said "hello what\'s your name?", Jim answered:"hello, my name is Jim"';
var reg = /"(.*)"/;
reg.exec(str); //"hello what\'s your name?", Jim answered:"hello, my name is Jim"
```
* 采用排除法
如果匹配一个字符用排除法
```javascript
var str = 'Tom said "hello what\'s your name?", Jim answered:"hello, my name is Jim"';
var reg = /"([^"]*)"/;
reg.exec(str); //"hello what\'s your name?"
```
* 采用.*?,忽略优先量词的匹配
```javascript
var str = 'Tom said "hello what\'s your name?", Jim answered:"hello, my name is Jim"';
var reg = /"(.*?)"/;
reg.exec(str); //"hello what\'s your name?"
```
* 采用排除环视，来匹配某个字符串
匹配出<b></b>之间的内容,排除法，忽略匹配优先的量词的方法都不能满足;
排除法针对的单个字符，而不是有着特定顺序的字符串
忽略匹配优先的量词的方法，对于<b>...<b>..</b>这种情况匹配不正确;
```javascript
var str = "<b>hello <em>marong></em></b> <b>wow</b>";
var reg = /<b>((?!</?b>).)*<\/b>/;
reg.exec(ss);
```

### 固化分组(?>)和占有优先的量词(?+,\*+, {min, max}+)
固化分组和占有优先的量词能提高效率
固化分组匹配成功后，会抛弃固化分组里的备用状态
```perl
//perl
$str = "hello";
$str =~ /(\w+):/;//\w+匹配整个字符串后，不断后退回溯，来匹配:,直到匹配到e，表达式宣告失败，效率低 
$str =~ /(?>\w+):/; //固化分组
$str =~ /\w++:/; //占有优先
```

```perl
//perl
$num = 27.624300009;
# $num =~ s/(\.\d\d[1-9]?)\d*/$1/; //27.624, 对于正好三位数27.625也会进行匹配，效率不高
# $num =~ s/(\.\d\d[1-9]?)\d+/$1/; //27.62, 对于27.625不能正确匹配
$num =~ s/(\.\d\d(?>[1-9]?))\d+/$1/;//27.624, 固化分组，匹配成功后，不会再释放
$num =~ s/(\.\d\d([1-9]?+))\d+/$1/;//27.624, 占有优先量词,匹配成功后，不会再释放
print "\$num: $num .\n";
```
javascript不支持,固化分组和占有优先的量词，但可以通过环视模拟来实现固化分组
对于环视，只会匹配成功或失败，不会保留备用状态，跟固化分组的行为一直，只是环视虽然能捕获，但不是全局匹配的一部分。
```
var str ="hello";
//var reg = /(?>\w+)/;//如果javascript支持固化分组
var reg = /(?=(\w+))\1/; //模拟捕获
reg.exec(str);
```
```javascript 
var num = 23.624;
var reg = /(\.\d\d[1-9]?)\d*/;
var reg = /(\.\d\d(?=[1-9])\2)\d*/;
reg.exec(num);
```
### 多选结构|
需要仔细安排多选结构的顺序
```perl
$str = "Jan 31 is Dad's birthday";
# $str =~ /Jan (0?[0-9]|[12][0-9]|3[01])/; //Jan 3,第一个表达式能匹配到3
$str =~ /Jan (3[01]|[12][0-9]|0?[0-9])/;  //Jan 31,调整多选的顺序
print "\$str: $&";
```
### 正则实例
* 匹配路径和文件名
```javascript
//javascript
var path = '/Users/marong/test/index.js';
var path = '/Users//marong/test///index.js';
var reg = /(.*\/)?(.*)/;
reg.exec(path);
```
* 匹配一个浮点数
下面这个正则表达式能匹配到任何数字或字符串，
```javascript
var num = 2.12;
var reg = /-?[0-9]*\.?[0-9]*/; //能匹配到任何数组,因为表达式的每一项都不是必须的，能匹配到任何字符串开头的空字符
var reg = /-?([0-9]+(\.[0-9]+)?|\.[0-9]+)/; //能匹配到任何数
var reg = /^-?[0-9]*\.?[0-9]*$/; //能匹配到任何数字和空字符串
var reg = /^-?([0-9]+(\.[0-9]+)?|\.[0-9]+)$/; //能匹配到任何数
reg.test(num);
```
* 匹配双引号之间的内容
```javascript
var str = 'Dash Symbol: "/-|-\\" or "[*_*]"';
var reg = /"(\\.|[^"])*"/;
reg.exec(str);
```
* 去掉首尾的字符
```javascript
var str = "  hello  world, I will test some regexp speed, check the regexp speed    ";
var regStart = /^\s+/;
var regEnd = /\s+$/;
var result;
var start = new Date();
var count = 1000000;
for(var i = 0;i < count; i++){
    result = str.replace(regStart,'').replace(regEnd, '');
}
console.log('normal time', +new Date - +start, 'result', result);//317

var reg = /^\s+(.*?)\s+$/;
start = new Date();
for(var i = 0;i < count; i++){
    result = str.replace(reg,'$1');
}
console.log('? time', +new Date - +start, 'result', result);// 314

var reg = /^\s+|\s+$/g;
start = new Date();
for(var i = 0;i < count; i++){
    result = str.replace(reg,'');
}
console.log('| time', +new Date - +start, 'result', result);//309

var reg = /^\s+((?:.*\S)?)\s+$/;
start = new Date();
for(var i = 0;i < count; i++){
    result = str.replace(reg,'$1');
}
console.log('\\S time', +new Date - +start, 'result', result);//260
```

* 匹配HTML标签
```javascript
var str = '<span data-sp=">" class="item" title=""></span>';
var reg = /<[^>]*>/g;//不能匹配标签里含有>的情况
var reg = /<("[^"]"|'[^']'|[^'">])*>/g;//能匹配标签里含有>的情况
var result = str.match(reg);
console.log(result);  //result [ '<span data-sp=">" class="item" title="">', '</span>' ]

```
* 匹配HTML Link
```javascript
var str = '<a class="navitem" data-sp=">" data-name=test href="http://marong.me/blog"   title="marong blog"   >hello marong</a>';
// var regA = /<a\b([^>]*)>(.*?)<\/a>/;
var regA = /<a\b(('[^']*'|"[^"]*"|[^'">])*)>(.*?)<\/a>/;
var aResult = str.match(regA);
var regHref = /href\s*=\s*('[^']*'|"[^"]*"|[^'">\s]*/;
var hrefResult = aResult && aResult[1] && aResult[1].match(regHref);;
console.log('aResult',aResult);;
console.log('hrefResult',hrefResult);
console.log('href=',hrefResult[1], '  value=',aResult[3]);
//href =  "http://marong.me/blog" value =  hello marong
```

## 优化正则表达式
* 字符组比多选结构的效率高
多选结构可能需要回溯，捕获型的括号处理也需要时间
```javascript
var timesTodo = 100000;
var testStr = 'ababadedfg';

for(var i = 0; i < 10000; i++){
    testStr += testStr;
}
var count = timesTodo;
var start  = new Date();
while(count--){
    /^(a|b|c|d|e|f|g)$/.test(testStr);
}
console.log('alternation takes seconds.\n', new Date - start);//107

var count = timesTodo;
var start  = new Date();
while(count--){
        /^[a-g]$/.test(testStr);
}
console.log('charactor class takes seconds.\n', new Date - start);//12
```
* 使用非捕获型括号(?:)
如果不需要引用括号内的文本，节省捕获的时间，坚守回溯使用的状态的数量。
* 使用起始锚点
如果要匹配某个字符串的开头位置字符，添加起始锚点，能提高匹配的速度。
* 将文字文本独立出来,提高引擎识别的可能性
- 从量词中提取必须的元素
```
/x+/ ---> /xx*/
/-{5,7}/ ---> /-----{0, 2}/

```
- 提取多选结构开头必须的元素
```
/(?:that|this)/ ---> /th(?:at|is)/
```
* 匹配优先，忽略匹配优先，排除性字符组
/.*:/
匹配优先，匹配到最后一个冒号，如果分号比较接近结尾，速度更快
/.*?:/
忽略优先匹配，匹配到第一个冒号,如果分号比较接近开头，速度更快
/[^:]*:/
匹配到第一冒号，一般比忽略匹配优先效率要高
