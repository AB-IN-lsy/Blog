Powered by:**NEFU AB_IN**
# <font color=#6495ED size=6>Ubantu 20.04阿里云源</font>

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources_init.list
sudo vim /etc/apt/sources.list
sudo apt-get update
sudo apt-get -f install #复损坏的软件包，尝试卸载出错的包，重新安装正确版本的。
sudo apt-get upgrade
```

```bash
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```


# <font color=#6495ED size=6>Vim</font>
有些教训写在这：
* 想写$fileheader$，最好里面不要有中文，要不然$quickfix$会一直报错
* 关于$vim$主题的选择，我选择的是$solarized$
大约是这样的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210524135936431.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1ODU5MTg4,size_16,color_FFFFFF,t_70)
选择它不单是颜值高，是感觉他有VSC的味了
关于背景问题，只有$light$和$dark$供选择，这是主题问题，如果你想用自己的背景图片的话，可以像我一样**配置终端背景**，然后**vim背景配置成透明**


* 相关的$vundle$插件的安装和加载，[链接](https://blog.csdn.net/geerniya/article/details/79687400)，之后在$vim$窗口输入

	```bash
	PluginInstall
	```

* $solarized$的配置，[链接](https://github.com/altercation/vim-colors-solarized)

* $clang\_complete$的配置，[链接1](https://www.cnblogs.com/Jiajun/p/3307979.html)，[链接2][https://blog.csdn.net/Arcsinsin/article/details/50555000]

* $.vimrc$的配置放在下面
```bash
"配置clang_complete"
let g:clang_complete_copen=1
let g:clang_periodic_quickfix=1
let g:clang_snippets=1
let g:clang_close_preview=1
let g:clang_use_library=1
let g:clang_library_path='/usr/lib/llvm-10/lib/'
let g:neocomplcache_enable_at_startup = 1
set nocompatible              "这是必需的"
filetype off                  "这是必需的"

"配置YCM在此设置运行时路径"
set rtp+=~/.vim/bundle/Vundle

"vundle初始化"
call vundle#begin()
Plugin 'VundleVim/Vundle.vim'
Plugin 'Valloric/YouCompleteMe'
Plugin 'vim-airline/vim-airline'
call vundle#end()            "这是必需的"
filetype plugin indent on    "这是必需的"

hi MatchParen ctermbg=Red guibg=lightblue

"格式化代码"
cclose
set cmdheight=2
set nu
set tabstop=4
set softtabstop=4
set shiftwidth=4
set autoindent
set cursorline
set expandtab
inoremap ' ''<ESC>i
inoremap " ""<ESC>i
inoremap ( ()<ESC>i
inoremap [ []<ESC>i
inoremap { {<CR>}<ESC>O



"配置solarized"
set background=dark
syntax enable
colorscheme solarized
set t_Co=256
syntax on


" 当新建 .cpp文件时自动调用SetComment 函数
autocmd BufNewFile *.cpp exec ":call SetComment()"
" 加入注释
func SetComment()
    call setline(1,"/*")
    call append(line("."),   "*  @Copyright (C) ".strftime("%Y")." NEFU AB_IN. All rights reserved.")
    call append(line(".")+1, "*  @FileName:".expand("%:t"))
    call append(line(".")+2, "*  @Author:NEFU AB_IN")
    call append(line(".")+3, "*  @Date:".strftime("%Y.%m.%d"))
    call append(line(".")+4, "*  @Description:https://blog.csdn.net/qq_45859188")
    call append(line(".")+5, "*/")
    call append(line(".")+6, "")
    call append(line(".")+7,  "#include <bits/stdc++.h>")
    call append(line(".")+8, "using namespace std;")
    call append(line(".")+9, "#define LL                    long long")
    call append(line(".")+10, "#define MP                    make_pair")
    call append(line(".")+11, "#define SZ(X)                 ((int)(X).size())")
    call append(line(".")+12, "#define IOS                   ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)")
    call append(line(".")+13, "typedef pair<int , int>       PII;")
    call append(line(".")+14, "")
    call append(line(".")+15, "int main(){")
    call append(line(".")+16, "    IOS;")
    call append(line(".")+17, "     ")
    call append(line(".")+18, "    return 0;")
    call append(line(".")+19, "}")
endfunc

"设置透明"
hi Normal guibg=NONE ctermbg=NONE
```

若遇到`The ycmd server SHUT DOWN (restart with ':YcmRestartServer').`

```
cd ~/.vim/bundle/YouCompleteMe
apt-get install cmake
apt-get install python3-dev
/usr/bin/python3 install.py
```



# <font color=#6495ED size=6>Cpp自动运行脚本</font>
```bash
#!/bin/bash
cppname=$1              
outname=${cppname%.*}                                                                             
outname=$outname".out"                                                             
g++ $cppname -o $outname  
./$outname
```
# <font color=#6495ED size=6>Python自动运行脚本</font>

```bash
#!/bin/bash                                                                                                                
pyname=$1
python3 -u $1
```
# <font color=#6495ED size=6>JAVA自动运行脚本</font>

```bash
#!/bin/bash
javaname=$1
javac $1
outname=${javaname%.*}
java $outname
```

然后给脚本加一个执行的权力。

之后

```bash
vim ~/.bashrc
```
在里面实现一个`alias`即可

之后如果想在任何路径都能运行，就在实现`alias`的同时，把shell脚本的地址加入$PATH$

# <font color=#6495ED size=6>Git</font>
* $git   \ lfs$ 可以处理大文件，同时在$config$需要改一下缓存的大小
* 没事别$commit$回退版本，会覆盖你目前目录下的所有文件
* 如果你要**本地**和**远程库**里的内容差距过大，版本跨度太大，会有fatal问题。需要解决冲突
	* 如果想保留远程库，可以用`git pull origin master --allow-unrelated-histories`，我用了这个命令之后，导致我本地的和远程库的一样了，差点数据丢失，最好别轻易用
	* 如果想把本地全上传至库，就不管冲突什么的了，`git push --force origin master `即可
	* 最好的建议是新建一个分支，然后切换分支上传
	* 血淋淋的教训啊，一定要记得commit记录回退版本，但也别随便回退。