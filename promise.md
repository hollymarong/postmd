---
title: promise
date: 2018-10-01 21:59:03
tags: javascript
---
```
function Promise(executor) {
  var self = this;
  self.status = 'pending';
  self.value = undefined;
  self.reason = undefined;
  self.onFulfilledArr = [];
  self.onRejectedArr = [];
  function resolve(value) {
    if (self.status === 'pending') {
      self.value = value;
      self.status = 'fulfilled';
      self.onFulfilledArr.forEach(function(fn) {
        fn(self.value);
      })
    }

  }
  function reject(resaon) {
    if (self.status === 'pending') {
      self.reason= resaon;
      self.status = 'rejected';
      self.onRejectedArr.forEach(function(fn) {
        fn(self.reason);
      })
    }

  }
  try {
    executor(resolve, reject);
  } catch(err) {
    reject(err);
  }

};

function resolvePromise(Promise2, x, resolve, reject) {

  if (Promise2 === x) {
    reject(new TypeError('循环引用'))
  }
  let called;
  if ( x !== null && (typeof x === 'function' || typeof x === 'object')) {
    let then = x.then;
    try {
      if (typeof then === 'function') {
        then.call(x, function(y){
          if (called) return;
          called = true;
          resolvePromise(Promise2, y, resolve, reject);
        }, function(err) {
          if (called) return;
          called = true;
          reject(err);
        })
      } else {
        resolve(then);
      }

    }catch(err) {
      if (called) return;
      called = true;
      if (called) return;
      reject(err);
    }
  } else {
    resolve(x);
  }
}

Promise.prototype.then = function(onfulfilled, onrejected) {
  onfulfilled = typeof onfulfilled === 'function' ? onfulfilled : function(value) { return value};
  onrejected = typeof onrejected === 'function' ? onrejected : function(reason) { return reason};
  var self = this;
  var Promise2;
  if (self.status == 'fulfilled') {
    return Promise2 = new Promise(function(resolve, reject) {
      try {
        let x = onfulfilled(self.value);
        resolvePromise(Promise2,x, resolve, reject);
        // resolve(x);
      } catch(err) {
        reject(e);
      }
    })
  }
  if(self.status === 'rejected') {
    return Promise2 = new Promise(function(resolve, reject) {
      try {
        let x =  onrejected(self.reason);
        resolvePromise(Promise2,x, resolve, reject);
      } catch(err) {
        reject(e);
      }

    });
  }
  if(self.status === 'pending') {
    return Promise2 = new Promise(function(resolve, reject) {
      self.onFulfilledArr.push(function() {

        try {
          let x = onfulfilled(self.value);
          resolvePromise(Promise2,x, resolve, reject);
        } catch(err) {
          reject(err);
        }
      });
      self.onRejectedArr.push(function() {
        try {
          let x = onrejected(self.reason);
          resolvePromise(Promise2,x, resolve, reject);
        } catch(err) {
          reject(err);
        }

      });
    });
  }
}

Promise.prototype.catch = function(cb) {
  return this.then(null, cb);
}

Promise.resolve = function(value) {
  return new Promise(function(resolve, reject){
    resolve(value);
  })
}
Promise.reject = function(reason) {
  return new Promise(function(resolve, reject) {
    reject(reason);
  });
}
Promise.all = function(promises) {
  if(Array.isArray(promises)) {
    throw new TypeError('The parameters of the promise method must be an Array');
  }
  return new Promise(function(resove, reject) {

    try{
      let arr = [];
      let count = 0;
      for (let i = 0; i < promises.length; i++) {
        promises[i].then(function(y) {
          arr[i] = y;
          count++;
          if (count === promises.length) {
            resolve(arr);
          }
        }, reject);

      };

    } catch(err) {
      reject(err);
    }
  });
}

Promise.race = function(promises) {
  return new Promise(function(resolve, reject) {
    for(let i = 0; i < promises.length; i++) {
      promises[i].then(resolve, reject);
    }
  })

}

var p = new Promise(function(resolve, reject){
  setTimeout(function() {
    resolve(200);
  }, 200);
})
p.then(function(result){
  console.log('success1', result);
  return new Promise(function(resolve, reject) {
    resolve(800);
  });
}, function(reason) {
  console.log('failed1', reason);
  return reason;
}).then().then(function(result){
  console.log('success2', result);
}, function(reason){
  console.log('failed2', reason);
})

```

