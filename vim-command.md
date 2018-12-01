---
title: vim-command
date: 2016-11-13 10:47:25
tags: Vim
---

### gvim 配置环境变量
PATH：添加上D:\Program Files\Vim\vim80
在cmd/GitBash 中输入gvim [文件]，在gvim中打开
-  gvim deps.html +30 打开文件之后，光标定位在30行

### normal模式:
- w 以单词为单位的移动，移动到下一个单词的首字母
- b 以单词为单位的后退，后退到上一个单词的首字母
- e 以单位为单位的移动，移动到下一个单词的尾字母
- ge 以单词为单位的移动，移动到上一个单词的尾字母
- cw 从光标到这个单词的结尾，并进入插入模式，c表示更改
- cc 将一行替换为想替换的文本

### Visual模式:
 - v--->h/l移动选择---d>删除
 - 使用标记
     - ``  以G为命令的跳转，会记得起跳的位置
     - Ctrl+o o:older 回到之前操作的地方
     - Ctrl+i  回到后来停置光标的位置
     - :jumps 列出曾经跳转位置的列表，>为最后一个跳转的位置
     - m{a-z}  可以自己定义标记的名字
     - `{a-z} 跳会自己定义标记的地方
     - V-j-j-<||>    5>> || 5<< (normal  模式下缩进)  选择多行并缩进


### 替换模式：
- r 替换当前光标下的字符
- R 进入替换模式，按Esc退出替换模式

### 命令行模式下(Esc):
:version 查看vim的版本
j:光标向下移动
k:光标向上移动
h:光标向右移动
l:光标向左移动
w:以单词为单位移动：
ctrl+F:向下翻一页
ctrl+B:向上翻一页
ctrl+D:向下翻半页
ctrl+U:向上翻半页
ctrl+E:增加底部行数，方便查看底部代码
zt：滚动至屏幕顶端
zb：滚动至屏幕底端
zz：滚动至屏幕中间
:w [path]:保存文件到指定路径 :w Desktop/hellowvim.html(保存到桌面文件)
G:跳转到文件尾
gg:跳转到文件头部
%：光标移动到行尾
^：光标移动到行首
[num]G:跳转到具体行数
o:在当前光标下方新增一行
O:
^/$:move to the beginning/end of line
I/A：move to the beginning/end of line and insert
D/C:delete/change until end of lline
V：高亮显示当前行
删除/复制行 
 - dd 剪切一行 ，按p进行粘贴。
 - 3dd 删除当前光标所在行的以下三行，前面加个数字可以剪切n行，按p进行粘贴。
 - yy  复制一行，，按p进行粘贴。
 - 3yy 复制三行，前面加个数字可以复制n行，按p进行粘贴。
 - p 粘贴 
 - v键进入选择模式，然后按h,j,k,l移动光标，选择文本，然后按y进行复制，按p进行粘贴。
 - 全部删除：按esc后，然后ggdG
 - 全部复制：按esc后，然后ggyG
撤消与撤消的撤消 
 - u 撤消 
 - Ctrl+R 撤消的撤消
 - :earlier 2m 回车  回到2分钟之前的操作
查找字符串 
 - :/string    #正向查找string  按n显示下一次 
 - :%string    #逆向查找string 
 - 在当前字符串上按*,正向查找当前字符串 
 - 在当前字符串上按#,反向查找当前字符串
 - :noh || set nohlsearch取消搜索高亮
 单词替换
- yiw ---->viw
 替换字符串
 - :s/keyword/replaceword: 替换光标所在行第一个出现的keyword为replaceword
- :s/keyword/replaceword: 替换光标所在行u
 macro 宏
 - .(dot command)不是所有的上一次的操作都能重复的，宏可以录制，并重复
 - 记录一个宏
    ```
    q{register}  //开始记录一个宏
    do something  //当前宏的操作
    q     //退出当前宏的记录
    ```
 - 播放一个宏
    ```
    @{register}  //播放一个宏
    ```
    将光标定位到age = 26这一行 ---->qw ---->开始记录当前宏 ----->进行一系列的操作，改为 var age = 26;操作完成之后----->q-->完成当前录制------>将光标移动到favourite行------>
    @w------>favourite这行就变成了var favourite = tea; ----->对name行执行相同的操作@w
    如果想选中三行，并播放宏。----先将光标定位到name行--->V---->jj---->选中三行---->:normal @w---->播放宏
    w就是register
    ```
        var name = marong,
            age = 26,
            favourite = tea;
    ```

分屏操作
 - Ctrl-w c   关闭当前屏幕
 - Ctrl-w s   上下分屏当前打开的文件
 - Ctrl-w v   左右分屏当前打开的文
 - Ctrl-w q   关闭当前窗口，如果只剩最后一个了，则退出Vim
 - ctrl-w w   光标自动在左右侧窗口切换 #！！！
 - ctrl-W K  当期那窗口上移动同时占据整个Vim程序窗口的宽度
 - ctrl-W H  向左移动窗口
 - ctrl-W J  向下移动窗口
 - ctrl-W L  向右移动窗口动
 - ctrl-w =  调整所有窗口至相同
 - ctrl-w +  增加当前窗口的高度
 - ctrl-W > 增加当前窗口的宽度
 - ctrl-W <  减小当前窗口的宽度
 - ctrl-W x 将当前窗口与下一个窗口交换
 - ctrl-w t 将光标移动到最左边的窗口
 - ctrl-w b 将光标移动到最右边的窗口
 - ctrl-w p 将光标移动到前一个（最后打开的）窗口
 - {num}ctrl-w + 将当前窗口增加num行的高度
 - :split 新增一个窗口上下分割，光标定位到上个窗口
 - :vsplit 新增一个窗口左右分割，新窗口出现在当前窗口的左边
 - :vnew 垂直分割窗口，并打开一个新的缓冲区
折叠
- zc 折叠某一个
- zo 展开某一个折叠
- zm  折叠全部
- zR 展开全部折叠
文字断行
 - :set wrap  所有文字断行
 - :set nowrap 所有文字强制一行
文件搜索
 - 在打开的文件中，光标放在有文件路径的那一个行，按下gf，文件就自动跳转到路径下的文件，页面中的include引入的片段也可以，要在另一窗口中打开指定的文件的话, 可以以"ctrl-w f"
 - gd 把光标放在函数上，按下gd，跳转到函数定义的地方
字符类相关
 - guu || vu： 把一行的文字全都变成小写
 - gUU || vU: 把一行的文字全都变成大写
 - ga： 查看光标出字符的ASCII码
 - -g8: 查看贯标出字符的utf-8码
 - x 删除当前光标下的字符
 - X 删除当前光标之前的字符
 - D 删除光标至行尾的内容
 - dw 删除当前光标至下一个word的开头
 - db 删除当前光标至前一个word的开始
 - diw 删除当前光标所在的word(不包括空白字符)
 - daw 删除当前光标所在的word(包括空白字符)
 - dG 删除当前行至文尾的内容
 - dgg 删除当前行至文头的内容
 - p/P 在光标所在行的下方/上方粘贴
 - f/F[字符]  向后移动到/向前移动到某个字符
 - [num]f/F[字符] 向后移动到/向前移动到某个字符的第几个
 - t/T[字符]  向后移动到/向前移动到某个字符的前一个字符
 - yiw：复制当前光标所在单词，p粘贴
 - yi} || yi] || yi) || yi" ||yi'：删除光标所在花括号、中括号、小括号里的内容
 - di} || di] || di) || di" ||di'：删除光标所在花括号、中括号、小括号里的内容
 - ci} || ci] || ci) || ci' ||ci"：删除光标所在花括号、中括号、小括号里的内容，并进入insert模式
 - yt[字符]：复制当前光标直到某个字符，不包括某个字符
 - dt[字符]：删除当前光标直到某个字符，不包括某个字符
 - ct[字符]：删除当前光标直到某个字符，不包括某个字符
 - df [字符]：删除当前光标知道某个字符，比如df.(删除当前光标直到.),dfn(删除当前光标直到n)
 - c：删除，插入并替换
 - caw：删除光标所在整个单词，并进入insert模式
 - cw：删除光标所在字符到单词的结尾
 - va" 选在双引号之间的内容，并连同双引号，选中之后可以y，c,d之类操作
 - va' 选择单引号之间的内容，并连同单引号，选中之后可以y，c,d之类操作
 - .  (repeat last command,重复上一次操作）
 奖贯标定位到aa的单引号内，ci'---->YYYY-MM-DD HH:mm:ss --->normal command ----->光标一定到bb的单引号内--->.--->就和aa的操作结果是一样的
 ```
 var aa = 'YYYY-MM-DD';
 var bb = 'YYYY-MM-DD';
  var cc = 'YYYY-MM-DD';
   var dd = 'YYYY-MM-DD';
 ```
缩进
 - >> << ：向后、向前缩进
 - = ：缩进当前行，对齐缩进
 - =% ：在代码的语句块重，把光标移动到{括号上,按下=%，对齐代码块中的代码
 - G=gg || gg=G ：缩进整个文件（G是到文件结尾，gg是到文件开头

### 插入模式(i):
 - Ctrl + n:自动补齐
 - Ctrl + p: 安装上下箭头，选择要补齐的字段
 - Ctrl +x Ctrl + f Ctrl + p:自动补齐文件名
 - Ctrl + X 和 Ctrl + D 宏定义补齐
 - Ctrl + X 和 Ctrl + ] 是Tag 补齐
 - Ctrl + X 和 Ctrl + F 是文件名 补齐
 - Ctrl + X 和 Ctrl + I 也是关键词补齐，但是关键后会有个文件名，告诉你这个关键词在哪个文件中
 - Ctrl + X 和 Ctrl +V 是表达式补齐
 - Ctrl + X 和 Ctrl +L 这可以对整个行补齐
 - i：从光标当前位置开始输入文件。
 - a：从目前光标所在位置的下一个位置开始输入文字。
 - o：插入新的一行，从行首开始输入文字。
 - I：在光标所在行的行首插入。
 - A：在光标所在行的行末插入。
 - O：在光标所在的行的下面插入一行。
 - s：删除光标后的一个字符，然后进入插入模式。
 - S：删除光标所在的行，然后进入插入模式。

### 底行模式
1. :Explore
 - 浏览项目目录,hj移动项目，回车选择项目或文件
 - 输入-回到上级目录
 - R：重命名文件
 - tab键可以来回切换目录
 - d 新建文件夹
2. :ls 查看当前打开的文件
 - :e#文件编号，编辑当前选择的文件
3. :f [filename]修改当前打开的文件名
4. :new [filename]新建文件
4. 文件保存和退出
 - :w 保存不退出
 - :q 退出文件
 - :wq 保存并退出
 - :q! 不保存并退出
 - :x 退出，如果文件更改则保存
 - shift + Z +Z  退出，如果文件更改则保存（按住Shift，再按两次z）
5. 文件对比
 - 在一个窗口里输入:diffthis
 - 在另一个窗口里输入:diffthis

### 缓冲区
 - :ls :files :buffers 列出缓冲
 - :ball 编辑所有参数或缓冲区
 - :buffer n 移向缓冲区n
 - :bdelete 卸载缓冲区，并从缓冲区列表中删除


### emmet-vim
- html:5 <Ctrl y ,> 生成html片段
- ! <Ctrl y ,> 生成html片段

### nerdtree快捷键
 - o: 打开当前文件夹或直接打开文件
 - go：打开文件，当光标留在nerdtree
 - t：在新 Tab 中打开选中文件/书签，并跳到新 Tab
 - T：在新 Tab 中打开选中文件/书签，但不跳到新 Tab
 - i：split 一个新窗口打开选中文件，并跳到该窗口
 - gi：split 一个新窗口打开选中文件，但不跳到该窗口
 - ？:来回切换帮助文档
 - 设置某个路径为当前的nerdtree，打开gvim后，输入:cd D:/frontend/modules/,回车--->cd--->选择打开当前目录--->C,设置当前目录为nerdtress
 - :NERDTree : 打开nerdtree
 - B：打开或关闭bookmark,打开bookmark，之后，双击项目
 - :Bookmark [<name>] 创建bookmark
 - 删除标签，双击选中标签，按下D，输入y
 - r: 刷新当前光标所在的文件
 - R：刷新当前根节点
 - p: 返回父级
 - A：全屏幕或默认状态显示nerdtree
- m--->a directory name\   创建一个文件夹
- m--->a filename  创建一个文件
- m--->d 删除某个节点
- m--->m 重命名或移动某个节点

### nerd commenter
- \cc  注释，多行注释，V--L--\cc
- \cu  取消注释，取消多行注释，V--l--\cu

### vim-multiple-cursors
- 替换单词，光标移动到单词，Ctrl+n --->Ctrl+n---->c--->replaceword 

### tab 标签切换
- gt: 标签切换到下一tab
- gT: 标签切换到上一个tab
- {i} gt ：i是数字，到指定页，比如：5 gt 就是到第5页
- :tabm {n}
- :tabc: 关闭当前标签
- :tabs: 查看所有打开的tab
- :tabo：  关闭所有其他的 tab：
- :tabp: 前一个tab
- :tabn: 后一个tab
- :tabclose [i] – 如果后面指定了数字，那就关闭指定页，如果没有就关闭当前页
- :q : 关闭当前页
- Ctrl + O： 向后回退你的光标移动，tab级别光标移动
- Ctrl + I：向前追赶你的光标移动，tab级别光标移动


这两个快捷键很有用，可以在Tab页和Windows中向前和向后trace你的光标键，这也方便你跳转光标。

### 读取shell命令
 - :r!date   上面这个命令，:r 是:read的缩写，!是表明要运行一个shell命令，意思是我要把shell命令的输出读到vim里来。

### ctrlp 插件的使用
 - :CtrlP 再输入要查找的文件名，选择文件
 - Ctrl + p  输入要查找的文件名、后缀，模糊搜索-->Ctrl+j/k上线移动选择文件 -->Ctrl+T(在tab中打开)|| Ctrl+v(垂直窗口打开) || Ctrl+x(水平窗口打开)
 - 一次打开多个文件：Ctrl+P --->Ctrl+k/j -->Ctrl+z(选中当前文件)----->Ctrl+k/j -->Ctrl+z(选中另一个当前文件)---> Ctrl+o(打开多个文件)
 - ctrl + p ctrl + f: 在    在普通文件、Buffers或者MRU文件模式切换

## EasyGrey 插件的使用
 - 按下的速度要快，不然动作会被分解，不会执行
 - :Grep targetword 
 - \vv || :Grep word 搜索当前光标下的单词
 - \vV || :Grep !word 全词搜索匹配
 - \vr || :Replace /target/replacement/ || :Replace target replace   替换词
 - :ReplaceUndo  撤销替换，只能撤销上一次的替换
 - 如果想查询指定文件夹下的某个词：
    ```
    //进入要查询的文件夹
    :cd d:/frontend/modules
    //搜索关键词,不区分大小写，递归子文件夹查找
    :Grep foreach -ir
    //打开EasyGrep选项，输入？显示全部操作面板，e查看当前操作的文件夹
    :GrepOptions ---> ? ----> a
    ```
## fugitive 插件的使用
 - :Gstatus 查看当前状态
 - :Gdiff 对比当前文件与上一版本的改动
## tagbar 插件的安装与使用
 - 安装tagbar插件，需要下载ctags，http://ctags.sourceforge.net/，选择windows的版本，下载压缩包完成后，把解压后的文件夹放在D:/Program Files/ 下，并配置环境变量：D:\Program Files\ctags58
 - :TagbarToggle

### vim 配置, _vimrc
### 打开配置文件的时候，自动折叠,设置 vim 相关文件打开后默认折叠方式为marker，约定俗成的用三个花括号注释包裹起来，配置文件看起来就简洁多了

```
" Startup {{{ 
  filetype indent plugin on 
" vim 文件折叠方式为 marker 
augroup ft_vim 
	au!
       	au FileType vim setlocal foldmethod=marker 
augroup END 
" }}}
```
### 设置编码
```
" Lang & Encoding {{{
set fileencodings=utf-8,gbk2312,gbk,gb18030,cp936
set encoding=utf-8 //暂时删除，菜单会乱码
set langmenu=zh_CN//暂时删除，菜单会乱码
let $LANG = 'en_US.UTF-8'//暂时删除，菜单会乱码
"language messages zh_CN.UTF-8
" }}}

```

### 设置gui
```
" GUI {{{
colorscheme Tomorrow-Night

source $VIMRUNTIME/delmenu.vim
source $VIMRUNTIME/menu.vim
set cursorline
set hlsearch
set number
" 窗口大小
set lines=35 columns=140
" 分割出来的窗口位于当前窗口下边/右边
set splitbelow
set splitright
"不显示工具/菜单栏
set guioptions-=T
set guioptions-=m
set guioptions-=L
set guioptions-=r
set guioptions-=b
" 使用内置 tab 样式而不是 gui
set guioptions-=e
" set nolist
set listchars=trail:·,extends:>,precedes:<
set guifont=Inconsolata:h12:cANSI

set statusline=%f
set statusline+=%m
set statusline+=\ %{fugitive#head()}
set statusline+=%=
set statusline+=%{''.(&fenc!=''?&fenc:&enc).''}
set statusline+=/
set statusline +=%{&ff}            "file format
set statusline+=\ -\      " Separator
set statusline+=%l/%L
set statusline+=[%p%%]
set statusline+=\ -\      " Separator
set statusline +=%1*\ %y\ %*

" }}}
```

### vundle安装
在windows系统中，vim编辑器配置的文件夹是vimfiles、_vimrc
在linux系统中，vim编辑器配置的文件夹是.vim, .vimrc
参考链接：http://allotory.github.io/posts/2016/02/25/vimrc_config/
git链接：https://github.com/allotory/doricha
步骤：
1. 备份好vimfiles, _vimrc文件
2. git clone --recursive git@github.com:allotory/doricha.git vimfiles
3. 在_vimrc文件中添加以下代码，
```
if(has('win32') || has('win64'))
    source $VIM/vimfiles/vundle_vimrc
    source $VIM/vimfiles/basic_vimrc
else
    source ~/.vim/vundle_vimrc
    source ~/.vim/basic_vimrc
endif
```
更新插件:BundleUpdate
清除不再使用的插件:BundleClean,
列出所有插件:BundleList
查找插件:BundleSearch
### 安装插件步骤
1. 安装nerdtree插件
    - 在vundle_vimrc中添加插件 Plugin 
    ```
    Plugin 'scrooloose/nerdtree'
    ```
    - 在gvim中输入:PluginInstall
    - 在basic_vimrc配置nerdtress
    ```
    autocmd vimenter * NERDTree
    let NERDTreeWinSize=25
    ```
    - 将已安装的插件以子模块的形式进行管理
    ```
    $ git submodule add https://github.com/scrooloose/nerdtree.git bundle/nerdtree
`   Adding existing repo at 'bundle/nerdtree' to the index
    ```
    ```
    git submodule init //每次add之后，都需要init
    ```
    - 此时我们已经完成了所有的安装配置操作，可以正常的提交到 Github。
    
2. 安装vim-tomorrow-night theme
    - 在vundle_vimrc中添加插件
    ```
    Plugin 'chriskempson/vim-tomorrow-theme'
    ```
    - 在gvim中输入:PluginInstall
    - 在basic_vimrc配置vim-tomorrow-night
    ```
    colorscheme Tomorrow-Night
    ```
    - 将已安装的插件以子模块的形式进行管理
    ```
    $ git submodule add https://github.com/chriskempson/vim-tomorrow-theme.git bundle/vim-tomorrow-theme
`   Adding existing repo at 'bundle/vim-tomorrow-theme' to the index
    ```
    ```
    git submodule init //每次add之后，都需要init
    ```
   - 此时我们已经完成了所有的安装配置操作，可以正常的提交到 Github。
2. 安装youcompleteme
 - 参照官网windows下的安装步骤
 - 1. 如何判断是否支持python3，命令符输入vim --version 看看里面是否有+python3，如果是-python3则表示不支持
 - 安装Vundle，具体步骤请看https://github.com/VundleVim/Vundle.vim
 - 通过Vundle安装YouCompleteMe
 - Plugin Valloric/YouCompleteMe加入vimrc
 - 启动Vim，:e PluginInstall，等待安装完成
 - 下载python3最新版本安装，ycm官方推荐python3
 - 下载安装CMake，安装过程中勾选加入path比较容易
 - 下载安装7zip，编译ycm的时候需要用
 - 下载安装Visual Studio 2015 Community版本
 - 打开VS2015，创建一个新C++ Project， 会提示你下载安装C++ tools，选择安装，此过程需要每个盘符4GB空间
 - 由于包比较大，子模块下载的时候容易出错，我后来就手动安装，需要网络好的情况下

### tern_for_vim
- ternjs/tern_for_vim
- 需要安装 tern_for_vim 就可以获得 JavaScript 的语义补全
- 安装完成后，在bundle/tern_for_vim里执行npm install
- 在项目目录里，需要配置.tern-project文件
```
{
  "libs": [
    "browser",
    "jquery"
  ],
  "loadEagerly": [
    "importantfile.js"
  ],
  "plugins": {
    "requirejs": {
      "baseURL": "./",
      "paths": {}
    },
    "node":{},
    "angularjs":{}
  }
}
```

### git删除一个submodule
需要手动删除.gitmodules里面repo的配置
```
git submodule deinit <asubmodule>    
git rm <asubmodule>
# Note: asubmodule (no trailing slash)
# or, if you want to leave it in your working tree
git rm --cached <asubmodule>
rm -rf .git/modules/<asubmodule>
```
### vim配置过程中报错
1. vundle #begin方法没有找到
 - 因为vundle没有clone到本地，在vim执行:echo globpath(&rtp, 'autoload/vundle.vim')，查看vundle路径，什么都没有返回
 - 解决办法：git clone --recursive git@github.com:allotory/doricha.git vimfiles
2. NERDTree使用m-->d删除某个文件夹的时候，报错，最后发现是因为用git bash命令行窗口打开的gvim，删除文件夹的命令不好用，直接打开gvim，删除文件的命令就不报错了
3. 如果有时候执行submodule add [url] [path],在.gitmodules没有改模块的配置,No submodule mapping found in .gitmodules for path
 - 解决方法：执行git rm --cached [path]
4.YouCompleteMe:安装过程遇到的问题
1.执行git submodule update --init --recursive 的时候，注意是否有的子模块下载失败，需要网络好的情况下，由于模块多，文件大，需要足够很长的时间和耐心，如果执行失败，就多执行几次，或者进入子模块中执行。在安装过程中，ycmd模块中的子模块较多，而且子模块中有子模块，多执行几次。
2.所有文件编译好之后，还是会报错：setUpPython Pythons site module could not be loaded。这是因为电脑上有python-2.7.11导致的，卸载掉python-2.7.11就好了。编译时用的python3.5.2

### TODO
1. 替换一个文件中某一行的keyword
2. 某个项目下的keyword
3. 快速注释 
4. 安装YouCompleteMe 代码片段snipe
5. 多光标同时编辑


  [1]: ./images/1479343349079.jpg
