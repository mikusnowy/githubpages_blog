---
title: "VIM 命令"
date: 2022-12-02
draft: false
tags: ["notes"]
categories: ["notes"]
authors: 
- GaGe
---
==这里不列举简单基础操作，只列举一些便捷的操作==

## 移动 
- `<S-Left>` 或 `<C-Left>` 左移一个单词
- `<S-Right>` 或 `<C-Right>` 右移一个单词
- `CTRL-B` 或 `<Home>` 命令行行首
- `CTRL-E` 或 `<End>` 命令行行尾
- 使用 `gj` 和 `gk` 命令可以只移动一个屏幕行（适用于较长的行，有回绕时候）
   - 可以使用`:map <Up> gk`，` :map <Down> gj` 在vimrc中定义

## 删除 
- `CTRL-W` 删除光标前整个单词
- `CTRL-U`  删除命令行上全部文字

## 切换 
- 单独文件切换
   - `CTRL-Z`退出挂起vim+`fg`返回vim
   - `:!{command}`
   - 使用`：mark` 查看所有的标记指向何处，用 `’0` 至`’9` 切换不同的工作。
   - 使用`：oldfiles` 查看最近编辑过的文件，
      - 使用`:e #<2`打开第2个文件或者`:split #<3`分割窗口打开第3个文件
      - 便捷操作：使用`:browse oldfiles` 显示所有文件，按q停止，输入数字+\<Enter> 进行文件
   - 使用`:wviminfo` 和`:rviminfo`保存和还原信息
      - example：
         在第一个 Vim 里执行:
         `:wviminfo! ~/tmp/viminfo`
         而在第二个 Vim 里执行:
         `:rviminfo! ~/tmp/viminfo`
   - 储存session `:mksession vimbook.vim`
      - 读取session `:source vimbook.vim` 或者 `vim -S vimbook.vim`
- 多文件切换
   - 使用`:edit .`打开当前目录，使用`<enter>`或者`<ctrl-o>`前进或后退，可以打开对应文件
   - 光标移动到需要打开的文件上，使用`gf`打开当前文件中存在的其他文件。
      - 如果存在路径需要修改的问题，使用`:set path+=`增加目录。如果没有，vim会以当前目录所在目录为起点，进行搜索（支持绝对路径和相对路径）
      - 可以使用`<ctrl-o>`返回
      - 使用`:find <filename>`可以实现同样功能，针对于文件中没有想要寻找的文件名称
      - 如果需要在新窗口打开对应文件，使用`<ctrl-w> f`
         - 对应的`:find`命令使用`:sfind`
   - 多个窗口时候，使用`<ctrl-w>`进行切换
- 缓冲区（buffer）
	- 使用`:buffers`查看所有的buffer
	- 使用`:buffer 2`打开相应的缓冲区
	- 可以使用`:buffer <filename>`，用文件名或者部分文件名打开buffer
	- 使用`:sbuffer 3`在新窗口打开缓冲区
	- `:bnext` 编辑下一个缓冲区
	- `:bprevious` 编辑前一个缓冲区
	- `:bfirst` 编辑第一个缓冲区
	- `:blast` 编辑最后一个缓冲区
	- buffer前标志：
	    - \u 列表外缓冲区 unlisted-buffer 。
	    - \% 当前缓冲区。
	    - \# 轮换缓冲区。
	    - \a 激活缓冲区，缓冲区被加载且显示。
	    - \h 隐藏缓冲区，缓冲区被加载但不显示。
	    - \= 只读缓冲区。
## 窗口
- 分割`:split` （默认是竖直分割）
	- 垂直分割：`:vsplit`
- 跳转
	`CTRL-W h` 跳转到左边的窗口
	`CTRL-W j` 跳转到下面的窗口
	`CTRL-W k` 跳转到上面的窗口
	`CTRL-W l` 跳转到右边的窗口
	`CTRL-W t` 跳转到最顶上的窗口
	`CTRL-W b` 跳转到最底下的窗口



## 编辑 
- 使用`CTRL-P`，vim会自动执行补全操作
   - 使用`CTRL-P`和`CTRL-N`进行选择。`CTRL-N` 意为下一个匹配，而 `CTRL-P` 意为前一个匹配。
   - 补全匹配和`:set ignorecase`相关
   - 补全明确类型的文本
      - `CTRL-X` `CTRL-F` 文件名
      - `CTRL-X` `CTRL-L` 整行
      - `CTRL-X` `CTRL-D` 宏定义 (包括包含文件里的)
      - `CTRL-X` `CTRL-I` 当前文件以及所包含的文件
      - `CTRL-X` `CTRL-K` 字典文件内的单词
      - `CTRL-X` `CTRL-T` 同义词词典文件内的单词
      - `CTRL-X` `CTRL-]` 标签
      - `CTRL-X` `CTRL-V` Vim 命令行
- 使用`CTRL-A`，vim执行重复输入（输入你上次在insert模式下输入的文本）
   - `CTRL-@` 命令会完成 `CTRL-A` 的操作后退出插入模式
- 使用`:iabbrev`命令来进行缩写
   - example：
      - `:iabbrev ad advertisement`只有在输入 ad 时候会被扩展成advertisement，输入add无影响
      - `:iabbrev #b /****************************************
         `:iabbrev \#e <Space>********************************/`
         **可以作为输入注释的方式**
   - `：iab`等同于`:iabbrev`
   - 该功能还可用来修正打字错误，把容易打错的字扩展成正确的字
   - `:abbreviate`列出所有缩写
   - `:unabbreviate`删除缩写
      - example：
         - `:unabbreviate @f`删除 f 的缩写
      - `:abclear`删除全部缩写
   - 防止缩写被映射，使用`:noreabbrev`避免映射
      - example：
         - `:noreabbrev @a adder`a的映射
- 使用`CTRL-O {command}`，在插入模式下使用任何普通模式的命令（但只能执行1个）
- `vimdiff` 可以使两个文件互相比较
	- `:dp`将把文字从左边拷到右边，从而消除两边的差异。"dp" 代表 "diff put"。
	- `:do`这把文本从左边拷到右边，从而消除差异。
## 查找
- `:set ignorecase` 忽略大小写
   - `:set ic`为该命令简写
   - `:set noignorecase`为开启大小写
   - `:set ignorecase smartcase` 如果你采用的模式里至少有一个大写字母，查找就成了大小写敏感的。
- `/` 表示正向查找，`？` 表示反向查找
- 在查找命令中加入`/`表示偏移
   - example
      - `/默认/2`，表示找到后使光标越过匹配的模式而前移两行，并停留在该行的行首。
      - `/默认/-1`，表示找到后使光标越过匹配的模式而回退两行，并停留在该行的行首。
   - **负数不要和**`n` **一起使用**。
      - example：`/const/-2`
      - 这个命令找到下一个单词 "const"，然后上移两行。如果你用命令 "n" 再找，Vim 就从当前位置开始，找到同一个 "const" 匹配。然后再一次在偏移的作用下，回到开始的地方。
- 组成一项的方法就是在它前面加 `\(`，后面加 `\)`
   - example:`/\(ab\)*` ,匹配ab这个整体0-n次
- 要避免匹配空字串，使用 `\+`。这表示前面一项可以被匹配一次或多次。
   - example ： `/ab\+`，匹配 "ab"、"abb"、"abbb" 等等。它不匹配后面没有跟随 "b" 的 "a"。
- 在一个查找模式中，"或" 运算符是 "`\|`
   - example：`/end\(if\|while\|for\)`，这个命令匹配 "endif"、"endwhile" 和 "endfor"。
- 为了避免匹配到一个特定的字符，在字符范围首位使用 `^` 。这样方括号项 [] 就会匹配任何括号内不包括的字符。
   - example: `/"[^"]*"` ，这个命令匹配 "foo" 和 "3!x"，包含双引号在内。
- 预定义：
   - `\a` 字母字符 等同于`[a-zA-Z]`
   - `\d` 数位 等同于`[0-9]`
   - `\D` 非数位 `[^0-9]`
   - `\D` 非数位 `[^0-9]`
   - `\x` 十六进制数位 `[0-9a-fA-F]`
   - `\X`非十六进制数位 `[^0-9a-fA-F]`
   - `\s`空白字符 `[ ]` (\<Tab> 和 \<Space>)
   - `\S` 非空白字符 `[^ ]` (非 \<Tab> 和 \<Space>)
   - `\l` 小写字母 `[a-z]`
   - `\L` 非小写字母 `[^a-z]`
   - `\u` 大写字母 `[A-Z]`
   - `\U` 非大写字母 `[^A-Z]`
   - `\_` 匹配一个换行符
      - `\_.` 匹配任意字符或一个换行符。

## 折叠 
- `zf` F-old creation (创建折叠)
- `zo` O-pen a fold (打开折叠)
- `zc` C-lose a fold (关闭折叠)
- `:mkview` 保存折叠
- `:loadview` 恢复折叠
- 可以根据 缩进、表达式、语法、标志进行折叠，具体参考UG

## 脚本 
-
   - `let {变量} = {表达式}` ，给变量赋值，此时为全局变量
      - 使用`let s:{变量} = {表达式}`，将变量定义为局部变量
      - `unlet` 删除变量
      - 如果你想在这样的字符串内使用双引号，在之前加上反斜杠即可
         - example：`let name = "\"peter\""`
   - 条件语句，支持 if - else if - else 结构
      - 在比较时候出现大小写比较时，。"`#`" 表示大小写敏感；"`?`" 表示忽略大小写。
         - `==?` 比较两字符串是否相等，不计大小写。
  ```
  :if {condition}
  {statements}
  :else
  {statements}
  :endif
  ```
   - 循环语句（while）
      - `:continue` 跳回 while 循环的开始；继续循环
      - `:break` 跳至 `":endwhile`"；循环结束
  ```
  :while {条件}
  : {语句}
  :endwhile
  ```
   - 循环语句（for）
      - 可以配合`range()`函数一起使用，listexpression处添加range函数
  ```
  :for {varname} in {listexpression}
  : {commands}
  :endfor
  ```
   - "`:execute`" 命令可以执行一个表达式的结果。
   - vim 函数（只列举会用到的）
      - `get()` 得到项目，错误索引不报错
      - `len()` 列表的项目总数
      - `insert()` 在列表某处插入项目
      - `add()` 在列表后附加项目
      - `sort()` 给列表排序
      - `repeat()` 重复列表多次
      - `count()` 计算字典里某值的出现次数
      - `line()` 光标或位置标记所在行
      - `search()` 查找模式的匹配
      - `mkdir()` 建立新目录
      - `delete()` 删除文件
      - `rename()` 重命名文件
      - `system()` 得到字符串形式的外壳命令结果
      - `getenv()` 得到一个环境变量
      - `setenv()` 设置一个环境变量
   - 自定义函数
      - 如果要重定义一个已经存在的函数，在 "function" 命令后加上 !
         - example：`:function! Min(num1, num2, num3)`
  ```
  :function {name}({var1}, {var2}, ...)
  : {body}
  :endfunction
  ```
   -  "`:function`" 命令列出所有用户自定义的函数及其参数
      - 如果要查看该函数具体做什么，用该函数名作为 "`:function`" 命令的参数即可
   - `:delfunction`删除函数
   - 双引号字符 `"` 标记注释的开始
   - 部分命令对于空格十分敏感，如无需要，不要设置空格

## 不常用但有趣的option 
- 加密：使用`vim -x <filename>`
   - 或者使用`set key=`，但是key会显示在屏幕上，不建议这么使用
- 打开二进制文件`vim -b datafile`
   - 对于部分不可显示的字符，使用`:set display=uhex`或`:%!xxd`
