# Vim 基本配置

## Vim 的基本知识

1. `Vim` 的配置文件为 `.vimrc` 可以通过
`sudo touch ~/.vimrc`创建
2. 以下配置都可以通过

    ```
    :配置 在vim命令中执行
    // 如 set number
    // 可以使用下面命令
    :set number
    ```

3. 关闭配置一般是在命令前加`no`

    ```
    " 打开
    set number
    
    " 关闭
    set nonumber
    
    双引号开始的行表示注解
    ```

## 基本配置

1. `set number` 显示序号
2. `set nocompatible` 不与`vi`兼容
3. `syntax on`打开语法高亮
4. `set showmode`在底部显示，当前处于命令模式还是插入模式
5. `set showcmd`在底部显示，当前键入的指令
6. `set mouse=a`支持使用鼠标
7. `set encoding=utf-8`使用 utf-8 编码
8. `set t_Co=256`启用256色
9. `filetype indent on`开启文件类型检查，并且载入与该类型对应的缩进规则
10. `set autoindent`按下回车键后，下一行的缩进会自动跟上一行的缩进保持一致
11. `set tabstop=4`按下 Tab 键时，Vim 显示的空格数
12. `set shiftwidth=4`在文本上按下>>（增加一级缩进）、<<（取消一级缩进）或者==（取消全部缩进）时，每一级的字符数
13. `set expandtab`由于 Tab 键在不同的编辑器缩进不一致，该设置自动将 Tab 转为空格
14. `set softtabstop=4`Tab 转为多少个空格
15. `set relativenumber`显示光标所在的当前行的行号，其他行都为相对于该行的相对行号
16. `set cursorline`光标所在的当前行高亮
17. `set textwidth=80`设置行宽
18. `set wrap`自动折行
19. `set linebreak`只有遇到指定的符号，才发生折行，不会在单词内部折行
20. `set wrapmargin=2`指定折行处与编辑窗口的右边缘之间空出的字符数
21. `set scrolloff=5`垂直滚动时，光标距离顶部/底部的位置
22. `set sidescrolloff=15`水平滚动时，光标距离行首或行尾的位置
23. `set laststatus=2`是否显示状态栏。0 表示不显示，1 表示只在多窗口时显示，2 表示显示
24. `set ruler`在状态栏显示光标的当前位置
25. `set showmatch`光标遇到圆括号、方括号、大括号时，自动高亮对应的另一个圆括号、方括号和大括号
26. `set hlsearch`搜索时，高亮显示匹配结果
27. `set incsearch`输入搜索模式时，每输入一个字符，就自动跳到第一个匹配的结果
28. `set ignorecase`搜索时忽略大小写
29. `set smartcase`如果同时打开了ignorecase，那么对于只有一个大写字母的搜索词，将大小写敏感；其他情况都是大小写不敏感。比如，搜索Test时，将不匹配test；搜索test时，将匹配Test
30. `set spell spelllang=en_us`打开英语单词的拼写检查
31. `set nobackup`不创建备份文件。默认情况下，文件保存时，会额外创建一个备份文件，它的文件名是在原文件名的末尾，再添加一个波浪号（〜）
32. `set noswapfile`不创建交换文件。交换文件主要用于系统崩溃时恢复文件，文件名的开头是.、结尾是.swp
33. `set undofile`保留撤销历史
34. `set autochdir`自动切换工作目录
35. `set noerrorbells`出错时，不要发出响声
36. `set visualbell`出错时，发出视觉提示，通常是屏幕闪烁
37. `set history=1000`Vim 需要记住多少次历史操作
38. `set autoread`打开文件监视。如果在编辑过程中文件发生外部改变（比如被别的编辑器编辑了），就会发出提示
39. `set listchars=tab:»■,trail:■
` `set list` 如果行尾有多余的空格（包括 Tab 键），该配置将让这些空格显示成可见的小方块
40. 命令模式下，底部操作指令按下 Tab 键自动补全

    ```
    set wildmenu
    set wildmode=longest:list,full
    ```

41. 设置备份文件、交换文件、操作历史文件的保存位置

    ```
    set backupdir=~/.vim/.backup//  
    set directory=~/.vim/.swp//
    set undodir=~/.vim/.undo// 
    结尾的//表示生成的文件名带有绝对路径，路径中用%替换目录分隔符，这样可以防止文件重名
    ```

## 使用：(本人的默认配置)

1. 使用一

    ```
    // 新建文件 touch ~/.vimrc
    // 添加下面配置
    set number
    syntax on
    set showmode
    set mouse=a
    set encoding=utf-8
    set t_Co=256
    filetype indent on
    set autoindent
    set tabstop=4
    set shiftwidth=4
    set cursorline
    set scrolloff=5
    set laststatus=2
    set  ruler
    set showmatch
    set hlsearch
    set ignorecase
    set listchars=tab:»■,trail:■
    set list
    set wildmenu
    set wildmode=longest:list,full
    ```

2. 使用二

    ```shell
    # 在终端执行
    curl https://raw.githubusercontent.com/ncdhz/vim-basic-config/master/vim.sh | bash
    ```
