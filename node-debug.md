---
title: NodeJs调试
date: 2017-06-18 21:09:44
tags:
---
## node-inspect
```
node-debug index.js
```
## chrome devtools
node --inspect来调试，在需要调试的地方设置下debugger, 在chrome浏览器中，打开commandline中提示的调试地址
```
    "scripts": {
        "start": "node ./worker | bunyan",
        "debug": "node --inspect --debug-brk  ./worker | bunyan",
        "lint": "eslint . --fix && echo 'Eslint Passed\\n'",
        "test": "eslint . --fix && echo 'Eslint Passed\\n' && node test/ | faucet"
    }
```
