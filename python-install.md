---
title: pyenv安装多版本Python
date: 2017-07-18 16:38:29
tags: Python
---

```
//安装
brew install pyenv
//设置pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc

//重新加载shell
exec $SHELL -l

// 查看当前可用的版本
pyenv versions

//安装特定版本的python
pyenv install <python version>

//在特定目录下，设置局部python版本
pyenv local <python version>
```

### python 包管理工具pip
```
pip install <package name>

pip list 查看安装的包
```
