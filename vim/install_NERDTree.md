vim插件 NERDTree的安装
========================

[Github](https://github.com/scrooloose/nerdtree)

## 安装

1. 安装pathogen.vim

[Github](https://github.com/tpope/vim-pathogen)

在NERDTree的安装过程中有命令需要用到这个小工具,所以首先要安装这个东西。

```bash
# 下载工具
mkdir -p ~/.vim/autoload ~/.vim/bundle && \
curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim

# 在~/.vimrc中添加以下命令
execute pathogen#infect()
```

2. 安装NERDTree

```bash
git clone https://github.com/scrooloose/nerdtree.git ~/.vim/bundle/nerdtree

# 重新打开vim,在vim中执行命令1/2
# 1
:helptags ~/.vim/bundle/nerdtree/doc/
# 2
:Helptags

# 检查是否安装成功,若显示THE NERD TREE Reference Manual即安装成功
:help NERDTree.txt
```

3. 使用

```bash
# vim中输入下面命令即可看见目录树的效果
:NERDTree

4. 分屏切换

由于NERDTree用的是vim的分屏功能,所以在目录树和文档之间的切换需要以下命令

```bash
# h:左 j:下 k:上 l:右
ctrl + w, h/j/k/l
```