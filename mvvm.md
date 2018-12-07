---
title: 双向绑定
date: 2018-12-05 11:19:42
tags: mvvm
---

在新的框架中，通过对数据的监控，发现变化genuine
* 基于脏检查的双向绑定
每个双向绑定的元素都有一个watcher
angular对DOM事件/XHR($http)等事件做了封装，会触发digest的流程，检查所有的watcher
Angular为Scope模型上设置一个监听队列，用来监听数据变化并更新view。
当有变量在view(html)上绑定时候，angular就会在$watch队列里面插入一条$watch,用来检查监视的model里是否有变化。
当浏览器收到特定的事件后，$digest循环就会触发，$digest会遍历所有的$watch
$digest脏检查最少2次，最多10次，多于10次会抛出异常

```

```

* 数据劫持：
vue采用数据劫持结合发布-订阅模式，通过Object.defineProperty来劫持各个属性的setter、getter，在数据变动时发布消息给订阅者，触发响应的监听回调。
getter/setter的双向绑定


1. 简单实现方法
```
// html
 <input type="text" id="txt">
 <p id="showTxt"></p>

// js
var data = {};
const txtNode = document.getElementById('txt');
const showTxtNode = document.getElementById('showTxt');
Object.defineProperty(data, 'txt', {
  set: function (newValue) {
    txtNode.value = newValue;
    showTxtNode.innerHTML = newValue;
  }
})

txtNode.addEventListener('keyup', function (e) {
  data.txt = e.target.value
})
```

2.数据劫持+发布订阅
```
// html
    <div id="app" >
      <h2>{{title}}</h2>
      <input v-model="name">
      <h1>{{name}}</h1>
      <button v-on:click="clickMe">click me!</button>
    </div>

// js
function Compile(el, vm) {
    this.vm = vm;
    this.el = document.querySelector(el);
    this.fragment = null;
    this.init();
}

Compile.prototype = {
    init: function () {
        if (this.el) {
            this.fragment = this.nodeToFragment(this.el);
            this.compileElement(this.fragment);
            this.el.appendChild(this.fragment);
        } else {
            console.log('Dom元素不存在');
        }
    },
    nodeToFragment: function (el) {
        var fragment = document.createDocumentFragment();
        var child = el.firstChild;
        while (child) {
            // 将Dom元素移入fragment中
            fragment.appendChild(child);
            child = el.firstChild
        }
        return fragment;
    },
    compileElement: function (el) {
        var childNodes = el.childNodes;
        var self = this;
        [].slice.call(childNodes).forEach(function(node) {
            var reg = /\{\{(.*)\}\}/;
            var text = node.textContent;

            if (self.isElementNode(node)) {
                self.compile(node);
            } else if (self.isTextNode(node) && reg.test(text)) {
                self.compileText(node, reg.exec(text)[1]);
            }

            if (node.childNodes && node.childNodes.length) {
                self.compileElement(node);
            }
        });
    },
    compile: function(node) {
        var nodeAttrs = node.attributes;
        var self = this;
        Array.prototype.forEach.call(nodeAttrs, function(attr) {
            var attrName = attr.name;
            if (self.isDirective(attrName)) {
                var exp = attr.value;
                var dir = attrName.substring(2);
                if (self.isEventDirective(dir)) {  // 事件指令
                    self.compileEvent(node, self.vm, exp, dir);
                } else {  // v-model 指令
                    self.compileModel(node, self.vm, exp, dir);
                }
                node.removeAttribute(attrName);
            }
        });
    },
    compileText: function(node, exp) {
        var self = this;
        var initText = this.vm[exp];
        this.updateText(node, initText);
        new Watcher(this.vm, exp, function (value) {
            self.updateText(node, value);
        });
    },
    compileEvent: function (node, vm, exp, dir) {
        var eventType = dir.split(':')[1];
        var cb = vm.methods && vm.methods[exp];

        if (eventType && cb) {
            node.addEventListener(eventType, cb.bind(vm), false);
        }
    },
    compileModel: function (node, vm, exp, dir) {
        var self = this;
        var val = this.vm[exp];
        this.modelUpdater(node, val);
        new Watcher(this.vm, exp, function (value) {
            self.modelUpdater(node, value);
        });

        node.addEventListener('input', function(e) {
            var newValue = e.target.value;
            if (val === newValue) {
                return;
            }
            self.vm[exp] = newValue;
            val = newValue;
        });
    },
    updateText: function (node, value) {
        node.textContent = typeof value == 'undefined' ? '' : value;
    },
    modelUpdater: function(node, value, oldValue) {
        node.value = typeof value == 'undefined' ? '' : value;
    },
    isDirective: function(attr) {
        return attr.indexOf('v-') == 0;
    },
    isEventDirective: function(dir) {
        return dir.indexOf('on:') === 0;
    },
    isElementNode: function (node) {
        return node.nodeType == 1;
    },
    isTextNode: function(node) {
        return node.nodeType == 3;
    }
}



function Watcher(vm, exp, cb) {
  this.cb = cb;
  this.vm = vm;
  this.exp = exp;
  this.value = this.get();  // 将自己添加到订阅器的操作
}

Watcher.prototype = {
  update: function() {
    this.run();
  },
  run: function() {
    var value = this.vm.data[this.exp];
    var oldVal = this.value;
    if (value !== oldVal) {
      this.value = value;
      this.cb.call(this.vm, value, oldVal);
    }
  },
  get: function() {
    Dep.target = this;  // 缓存自己
    var value = this.vm.data[this.exp]  // 强制执行监听器里的get函数
    Dep.target = null;  // 释放自己
    return value;
  }
};



function Observer(data) {
    this.data = data;
    this.walk(data);
}

Observer.prototype = {
    walk: function(data) {
        var self = this;
        Object.keys(data).forEach(function(key) {
            self.defineReactive(data, key, data[key]);
        });
    },
    defineReactive: function(data, key, val) {
        var dep = new Dep();
        var childObj = observe(val);
        Object.defineProperty(data, key, {
            enumerable: true,
            configurable: true,
            get: function getter () {
                if (Dep.target) {
                    dep.addSub(Dep.target);
                }
                return val;
            },
            set: function setter (newVal) {
                if (newVal === val) {
                    return;
                }
                val = newVal;
                dep.notify();
            }
        });
    }
};

function observe(value, vm) {
    if (!value || typeof value !== 'object') {
        return;
    }
    return new Observer(value);
};

function Dep () {
    this.subs = [];
}
Dep.prototype = {
    addSub: function(sub) {
        this.subs.push(sub);
    },
    notify: function() {
        this.subs.forEach(function(sub) {
            sub.update();
        });
    }
};
Dep.target = null;

function SelfVue (options) {
  var self = this;
  this.data = options.data;
  this.methods = options.methods;

  Object.keys(this.data).forEach(function(key) {
    self.proxyKeys(key);
  });

  observe(this.data);
  new Compile(options.el, this);
  options.mounted.call(this); // 所有事情处理好后执行mounted函数
}

SelfVue.prototype = {
  proxyKeys: function (key) {
    var self = this;
    Object.defineProperty(this, key, {
      enumerable: false,
      configurable: true,
      get: function getter () {
        return self.data[key];
      },
      set: function setter (newVal) {
        self.data[key] = newVal;
      }
    });
  }
}

new SelfVue({
  el: '#app',
  data: {
    title: 'hello world',
    name: 'canfoo'
  },
  methods: {
    clickMe: function () {
      this.title = 'hello world';
    }
  },
  mounted: function () {
    window.setTimeout(() => {
      this.title = '你好';
    }, 1000);
  }
});

```
* 参考链接:http://www.cnblogs.com/canfoo/p/6891868.html
