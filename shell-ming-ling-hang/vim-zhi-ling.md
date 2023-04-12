# Vim指令

```vim
每行的行首都添加一个字符串：
:%s/^/要插入的字符串

每行的行尾都添加一个字符串：
:%s/$/要插入的字符串
```

常用配置：

```vim
set number #显示行号
set showmatch #显示匹配括号
set cc=180 #显示宽度限制线
set cursorline # 当前行高亮
syntax on #语法高亮
```
