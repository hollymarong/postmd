---
title: emacs-command
date: 2016-12-27 10:47:18
tags: Emacs
---

### info
- M-x info(C-h i) -->  Emacs lisp Intro

## Shell
- eshell

## Magit
- Magit is a full Git interface

## 快捷键
- Ctrl-H t  Built in Tutorial
- Ctrl-x Ctrl-c  退出ecma
- Ctrl-v 翻到下一页
- Alt-v 翻到上一页
- Ctl-l 光标所在行的文字移动到中间，再次输入，移动到顶端，再次输入，移动到底端
- Ctrl-p 上一行
- Ctrl-n 下一行
- Ctrl-f 向右移动
- Ctrl-b 向左移动
- M-f  对英文以单词为单位向右移动，对中文以标点符号为单位向右移动
- M-b 对英文以单词为单位向左移动，对中文以标点符号为单位向左边移动
- Ctrl-a 移动到一行的头部
- Ctrl-e 移动到一行的尾部
- M-a 移动到一句的头部
- M-e 移动到一句的尾部
- M-< 将光标移动到头部(Alt-Shift-<)
- M-> 将光标移动到尾部(Alt-Shift->)
- Ctrl-u-数字 Ctrl-f 将光标向右移动8个单词 
- Ctrl-u-8 m  --在当前光标下输入8个m
- Ctrl-g 用来终止命令   
- Ctrl-/|Ctrl_(Ctrl) 撤销命令
- Ctrl-d 删除贯标后的一个字符
- M-d 删除贯标后的一个单词
- M-del 删除光标前的一个单词
- del 删除高标前的一个字符
- Ctrl-k  移除从光标到行尾的字符
- M-k 移除从光标到行尾的字符
- Ctrl-@  移动到要删除的字符处 Ctrl+w
- Ctrl-y  粘贴
- C-h k C-p  在新视图中查看命令的详细说明
- C-h f 函数名 解释一个函数名
- C-h k 按键  查看快捷键
- C-h v 

## 窗口
- Ctrl-x-1  只保留一个窗口
- Ctrl-x-2  将屏幕垂直划分成两个窗格，光标停留在上方的窗格

## 文件
- Ctrl-x Ctrl-f 查找文件
- Ctrl-x Ctrl-s 存储文件
- 创建新文件的方法，就是查找一个不存在的文件，然后保存
- Ctrl-x Ctrl-b 列出缓冲区
- Ctrl-x b+缓冲区的名字（按tab自动补全）  跳转到之前编辑的文件
- Ctrl-x s 保存多个缓冲区
- Ctrl-z 暂时离开emacs,不杀死emacs的进程
- Ctrl-s 向前搜索
- Ctrl-r 向后搜索
- M-x kill-buffer 从buffer list删除buffer文件
- M-x ibuffer 显示buffer list , D 删除buffer
- C-x C-e 执行lisp语句

### 批量重命名文件
- C-x C-j 打开当前文件所在目录 Dired模式
- Shift+6 跳转到上级目录
- C-; iedit模式在dired模式下快捷键失效，启用重新绑定的M-s e
- C-x C-q 进入到dired模式的Editable编辑模式-->C-=(选中要重命名的文件)-->M-s e(同时编辑)-C-c C-c(编辑完成保存)
- C-c Esc 退出修改


### 搜索和替换
- search speed:ag(Linux)>pt(所有平台)>ack>grep
- helm-ag 安装 
- M-x helm-do-ag-project-root <字段 !需要屏蔽的字段> --> C-c C-e (打开buffer 能直接编辑搜索出的结果)
- C-c p s 快捷键绑定搜索
- C-c c-k 取消
- 报错Error running time helm-ag--do-ag-cand..,是因为没有安装ag
- 项目下全局搜索和替换
```
SPC s a f --> <keyword> ---> C-c C-e 能编辑接口 ---> C-c C-c 提交修改
```

### 安装ag
```
brew install the_silver_searcher
ag --version
```
### helm-ag
*  搜索工具，linux下工具，在windows下需要Cygwin工具
*  用babun安装,编译完成后把ag.exe放到bin下，添加环境变量
C:\Users\marong\.babun\cygwin\bin
```
$pact install libpcre-devel
$pact install liblzma-devel

$git clone https://github.com/ggreer/the_silver_searcher.git

$cd the_silver_searcher
$./build.sh

```

### imenu
- M-x imenu 列出函数列表，能列出lisp，不能列出js函数列表
- M-x counsel-imenu  能列出js函数列表
- M-s i：列出函数列表，调用counsel-imenu


### flycheck 代码检查
- 安装全局的eslint
```
npm install -g eslint
//配置文件~/.eslintrc
//找到一个js文件，flycheck-verify-setup --> javascript-eslint --> 就会看到eslint打开了
// M-x flycheck-mode就能看到错误提示
```
- 使用项目中安装和配置的eslint
```
SPC e S(flycheck-set-checker-executable)--> javascript-eslint --> 项目下/node_modules/eslint/bin/eslint 
--> M-x flycheck-mode就能看到错误提示
```
- 只对js文件进行代码检查,windows系统不好用
- 安装过程玩后报错require let-alist not　provided,在elpa文件下有let-alist的包，最后发现let-alist.el文件没有内容，卸载后重新下载后，flycheck能用了
- M-x flycheck-verify-setup
```
(add-hook 'js2-mode-hook 'flycheck-mode)
```






- M-x flycheck-list-errors 开启一个新的buffer来展示错误，在buffer具体错误上回车，可以跳转到相应的代码

### evil 安装过程遇到的问题
* 安装evil后报错提示undo-tree goto-chg两个package未安装，分别安装两个packages
* M-x evil-mode找不到evil模式，重新卸载M-x package-list-packages --> d --> x 卸载之后，重新安装后，就能打开evil模式了


### yasnippet
-  可以自定义
- al Tab ---> alert
- f Tab ---> function
- com Tab ---> /* */
- log  Tab ---> console.log

### auto-yasnippet
- M-x aya-create
- M-x aya-expand
```
(add-to-list~AAA path~AAA name~AAA)
M-x aya-create
M-x aya-expand
```

## 模式
- Ctrl—x text-mode
- M-x fundamental-mode 缺省模式
- M-x auto-fill-mode 自动换行模式
- 设置自动换行边界数字：Ctrl-u 70 Ctrl-x f
- M-x linum-mode 设置行号
- M-x recentf-mode 该模式保存最近打开的文件


### Org Mode
- Tab 展开/关闭
- C-c C-t Todo/Done/None切换
- M-Return 自动排序号
- <s Tab 自动补全 #+BEGIN_SRC #+END_SRC
- C-c C-c 保存
- C-x C-e 导出html/LaTex(PDF)格式 --> ho(导出html，并打开)
- C-c a s  搜索日程

### M-x org-capture 记笔记
#### 创建todo
- SPC C(capslock) c --> t -->新建TODO事项
- M-x org-capture -->t-->新建TODO事项
- C-c C-s 设置日程开始时间
- C-c C-s 设置日程结束时间
- C-c a-a 查看日程 
- C-c C-c 退出 

#### org-pomodoro
- 时间管理工具
- 安装完成后需要require
- 在gtd.org里C-c a -->打开agenda--> 在一个gtd里输入M-x org-pomodoro--->在状态栏里就能看到时间管理

### Occur模式 
* 搜索模式
* M-s o <搜索内容>
* 回车跳转到搜索的地方
* C-x o在搜索的buffer和popwin之间切换
* M-x occur-edit-mode(e)  能直接在搜索结果里进行修改
* 增强occurmode，M-s o能直接抓取光标所在的单词，或选中的字段
- SPC o d 搜索光标所在单词

### web-mode
- M-x customize-group --> web-mode --> 定制
- C-h v web-mode-markup-indent-offset 查看html 当前缩进值
- C-c t i 切换当前缩进值2/4
- C-M \ 整个buffer缩进
- M-; toggle 注释

### js2-refactor
- 提取函数:选中重构的函数，C-c C-m --> ef
- 提取方法函数：C-c C-m --> em
- 提取本地变量为函数参数： C-c C-m --> ip
- 提取函数参数为函数内变量： C-c C—m -->lp

### Dired模式
* Dired模式下可以拷贝、删除、重命名文件
- C-x d <文件> 显示制定文件夹下文件详情 大小、时间等
- + 创建文件夹和文件，回车进入新建的文件夹里    
- C-x C-f : create file
- g: refresh dired buffer
- C: copy file  
- D: delete file
- d: mark delete file 
- u: cancle mark 
- x: execute
- R: rename filename
```
//能打开当文件所在的目录C-x C-j
(required 'dired-x)
```

### expand-region
- C-= 能选中光标所在单词，按=逐级往外扩展选中
- SPC v

### iedit
- M-x iedit-mode 定位光标所在的单词，同时编辑多个区域
- C-; 绑定的快捷键 多编辑toggle
- M-s . (搜索光标所在的单词), C-s 正向查找， C-r反向查找
- M-s o (搜索内容) -> e 进入过occur编辑模式 --->C-=(expand-region) 选中要编辑单词 -> M-s e 编辑多个区域

### evil 
1. vim 80% 的功能
2. the difference between Normal and Emacs state
    - C-u vim中向上翻页 Emacs中Universal args， 设置之后Ctrl-u/d 可以上下翻页了
3. evil state
    - evil normal state
    - evil insert state
    - evil visual state
    - evil motion state
    - evil operator state
4. C-z(M-x evil-emacs-state) toggle between Normal and Emacs state

### evil-leader
```
(evil-leader/set-key
  "e" 'find-file
  "b" 'switch-to-buffer
  "k" 'kill-buffer)
```
### window-numbering
- M-<num> 窗口跳转

### powerline 
- 增强mode-bar

### evil-surround
- viw S "|' 给单词加上双引号
- cs '" 把单引号换成双引号
- cs "(  把双引号换成括号，如果按9上的(会有空格
- cs ") 把双引号换成括号，如果按0上的）不会有空格
- SPC v s 

### evil-nerd-commenter
- M;  注释切换
- V jk --> M;
- gv 选择上次选中的区域，M; -->取消注释

### use-package
- 更安全的require
  ```
  ;;  会报错
  (require 'xxx)  
  ;; 不会报错
(use-package xxx)


  ```
###spacemacs
- 直接glone 到d:/Emacs24.4/.emacs.d目录
- 安装初始化选项，在安装的过程中
- spc+f+j 打开当前文件所在的目录
- spc+qq 退出emacs
- SPC-b-f  在finder中打开文件
- SPC-f-e-R  安装包
- SPC-f-e-d 打开.spacemacs.d/init.el
- SPC-f-e-i 打开.emacs.d/init.el
- SPC-f-e-r 安装包
- SPC-h-SPC <layer> 查看layer安装的packages
- Ctrl-s 搜索
- M-x (SPC-SPC)命令
- C-h f查看lisp函数定义的文档
- C-h v查看lisp的变量
- C-h C-f 查看函数
- SPC-s-j 查看函数列表文件
- SPC-h-l  查看安装的elpa package文件
- SPC-h-r 查看spacemacs全部文档
- SPC-h-R 查看spacemacs 安装包的文档
- SPC-f-f find file
- SPC-f-j dired mode
- SPC-o-y  有道搜索单词
- ,-d-m-e 展开宏函数,q退出宏函数
- SPC-<num> 窗口间的跳转
- SPC-w-d 删除打开的窗口
- SPC-p-f 查找项目下的文件
- SPC-f-f　查找当前项目下的文件夹
- SPC-f-L  查找本地文件
- SPC-J-= 格式化全文
- C-h 返回上级目录
* insert 模式下
 - C-a 移动到行首
 - C-w 移动到行尾

### spacemacs 文件操作
- SPC-p-f 查找当前文件下的目录
- SPC-f-L 查找整个系统的文件
- SPC-f-l
- SPC-f-o 使用外部程序打开文件
- SPC-f-E sudo-edit 以root权限编辑文件host
- SPC-f-d 删除当前文件及buffer
- SPC-f-j 打开当前buffer的dired目录
- SPC-f-r　打开当前常用的文件
- SPC-f-R 重命名当前文件
- SPC-f-v 给当前项目设置lisp的变量
- SPC-f-y 查看当前buffer的路径并复制
- SPC-f-e-d 打开/.spacemacs.d/init.el文件
- SPC-f-e-i 打开/.emacs.d/init.el 文件
- SPC-f-c copy file
- SPC-f-b　jump bookmark
- SPC-f-s 保存当前buffer
- SPC-f-S 保存所有的buffer

### spacemacs buffer 操作
- SPC-b-. 显示buffer操作提示n/p切换buffer
- SPC-b-b 查找buffer
- SPC-b-d 删除buffer
#### spacemacs 创建自己的layer

### 2048game
- C-z  切换Normal--> Emacs state

### Tips and Tricks
* Use "Search" instead of cursor movement
- Ctrl-S and word to jump to 
- Alt-N to jump to same word cursor is on
- F13 to jump to a character 
* Repeat a command with a prefixed number, i.e.
 - Alt-9 Alt-B to jump back 9 words
 - Ctrl-5 Ctrl-P to jump up 5 lines
 - linum-relative mode is helpful

### 插件
- Evil:EvilVim 模拟器
- Options/Manage Emacs Packages/Company/install
    - 安装完成之后，在文件中开启M-x company-mode 打开自动补全功能 M-n M-p上下选择要补全的内容
    - M-x global-company-mode 打开的任何文件都会自动补全
- 换color theme 
    - M-x load-theme monokai

## packages
- M-x package-list-packages === Options/Manage Emacs Packages(因为init.el中添加了melpa源，packagelist 里有3000多个包)
- 选择要安装的包，光标放在要安装的包上，i
- 取消选择包，光标放在要取消的包上，u
- 搜索包，C-s 包名
- 选择要删除的包，光标放在要删除的包上，d
- 执行命令，x
- 更新所有的packages, U

### custorm-group
* 修改插件配置
 - M-x customize-group
 - js2-mode
 - external-varialable 点击向下的箭头
 - 修改颜色为darkgrey
 - 点击state -> Save for future sessions
 - q 退出 
 - 未定义的js变量为灰色
 - 在init.el 最后面，多了一行
```
(custom-set-faces
    '(js2-external-variable ((t (:foreground "dark gray"))))
```
### nodejs-repl
M-x nodejs-repl
,sb 运行当前buffer的代码

### smex 安装步骤
- 在d:/Emacs24.4/.emacs.d/init.el中包列表中加入smex
- 增加M-x的插件，C-s切换选项
```
//加载
(require 'smex) ; Not needed if you use package.el
(smex-initialize) ; Can be omitted. This might cause a (minimal) delay
                  ; when Smex is auto-initialized on its first run.
//配置快捷键
(global-set-key (kbd "M-x") 'smex)

```

### swiper 插件的安装
- 在d:/Emacs24.4/.emacs.d/init.el中包列表中加入swiper
- 配置完快捷键后，发现与counsel有关的函数不能执行
- 用M-x package-list-packages 重新安装的counsel

## 调试
-- emacs --debug-init
-- emacs -Q
    -- 这样Emacs就不会加载任何配置文件， 你可以对比Emacs默认的行为来判断是否真的是因为配置文件引起的问题。
## mac 下安装步骤
brew install emacs --with-cocoa --with-gnutls
ln -s /usr/local/Cellar/emacs/25.3/Emacs.app /Applications 
## windows下安装步骤
- 到这个网址可以下载到Emacs的windows版本：http://ftp.gnu.org/pub/gnu/emacs/windows/
- 双击bin文件夹里的addpm.exe进行安装
- 在D创建文件夹Emacs24.4
- - 为了方便全局调用，请添加bin路径到环境变量（比如我的，D:\Emacs24.4\bin）。
- 在cmd里，输入emacs -nw，以终端模式来运行emacs；只输入emacs，以GUI模式来运行

- 启动emacs，用鼠标点击Options菜单，随便点击一两个选项，比如点击一下Active Region Highlighting，然后点击Save Options,这样做的目的是让emacs自动创建.emacs.d目录以及.emacs文件
- 在Emacs窗口的最后一行，显示Wrote C:\Users\marong\AppData\Roaming\.emacs

### 遇到的问题
* windows 下flycheck不太兼容
* tern libray　not found 

    ```
    //window 下
    npm install -g tern
    ```
macos ，在.spacemacs.d/init.el的user-init里添加
```
  (setenv "PATH" (concat (getenv "PATH") ":/Users/marong/.nvm/versions/node/v7.10.0/bin"))
  (setq exec-path (append exec-path '("/Users/marong/.nvm/versions/node/v7.10.0/bin")))
```

### 国内镜像
- 用清华的镜像稳定一点，elpachina不太稳定
1.popkit
```
  (setq package-archives
            '(("popkit" . "http://elpa.popkit.org/packages/")))

```
2.elpachina
http://elpa.emacs-china.org/
```
(setq package-archives '(("gnu"   . "http://elpa.emacs-china.org/gnu/")
                         ("melpa" . "http://elpa.emacs-china.org/melpa/")))
```

### elisp(emacs lisp)
* 写elisp 
- 导航栏里/Buffers/*scratch*   默认开启Lisp Interaction mode

* 一般计算表达
- 输入(+ 2 2)  --- C-x C-e  在最下面提示4
- 输入;2 +3*4 回车之后 输入内容居中 
- C-h m  查看当前模式 内容居中的原因是开启了electric-indent mode
- M-x electric-indent-mode 关闭该模式
- 重新输入; 2 + 3 * 4 --- 分号是注释的意思
- (+ 2 (* 3 4)  --->C-x C-e  --->结果14
* 设置变量
- 设置变量(setq my-name "marong") C-x C-e
- 输出变量(message my-name) C-x C-e
* 设置函数
```
(defun my-func()   //没有变量
    //定义成交互式函数，就能M-x my-func
    (interactive)
    (message "hello, %s" my-name))  //%s加上是为了输入my-name
(my-func)

//为my-func函数执行绑定快捷键
(global-set-key (kbd "<f2>") 'my-func)
```

- M-x eval-buffer 执行整个buffer文件
### macro
*  可以生成代码的代码

### markdown编辑
1.markdown中写表格|name|age|gender ->回车 ->生成表格
2.Tab 切换表格单元格
3.Shfit-Tab 切换到上一个单元格

### 对齐问题
不同mode对齐缩进需要单独设置
2/4空格的切换：,ti => SPC = j

### email 邮箱设置
设置邮箱的发送方式:send mail

### 打开光标坐在路径文件open file path under cursor
SPC f l === M-x find-file-at-point

### 编辑
S s e: 编辑光标所在单词
选中多行，M-x -> edit line: 编辑多行
