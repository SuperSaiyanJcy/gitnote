## Mac软件安装地址

- [python](https://www.python.org/downloads/)
- [cmake](https://cmake.org/download/)
- [arm-gcc](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)

## Mac一些技巧

```shell
# 启用安装任何来源
sudo spctl --master-disable         # 需在 设置-隐私与安全性 中启用允许任何来源
xattr -cr /Applications/XXX.app     # 提示损坏可以尝试一下这样做
```

## Mac一些终端命令

```shell
sudo su             # 切换一个管理员权限的账户来操作
sudo [order]        # 以管理员身份执行,e.g. sudo vim
cd ~                # ~代表用户目录 /Users/jcy
cd /                # 根目录
------------------------------------------------------------------------------------------
# ""中shell会对变量进行扩展（如$HOME），shell会在执行时替换这些变量的值
# ''中shell不会对其中任何字符进行扩展，即$~和其他特殊字符都回原样输出不会被解释
# alias mydir="cd $HOME/Documents" 在这种情况下，$HOME 会被扩展成实际的路径，例如 /Users/username
------------------------------------------------------------------------------------------
alias gcc='gcc-14'
alias gcc="gcc-14"
```

## homebrew

```shell
# install
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# uninstall
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
```
- **终端中homebrew会提示输入3行指令到终端用以添加到系统环境变量**

## zshrc与系统环境变量

```shell
/etc/zshrc                  # 开机加载的
~/myprofile                 # 自定义我的配置文件
------------------------------------------------------------------------------------------
# 配置系统环境变量
# 在myprofile中写入
# alias gcc='gcc-14'
# PATH="path to XXX":"$PATH"
# e.g. PATH="/Applications/CMake.app/Contents/bin":"$PATH"
# zshrc中添加 source ~/myprofile
------------------------------------------------------------------------------------------
```

## VIM

```shell
vim filename    # 打开文件
# 进入普通模式
#  • 启动 vim 后，默认是处于 普通模式，在这个模式下，你不能直接编辑文本。你可以使用箭头键或 h、j、k、l 键来导航文件。
#  • 在普通模式下，你可以执行删除、复制、粘贴、搜索等操作
# 进入插入模式
#  • 按 i 键：在光标前面插入文本（insert）。
#  • 按 I 键：在当前行的开头插入文本（Insert at the beginning of the line）。
#  • 按 a 键：在光标后面插入文本（append）。
#  • 按 A 键：在当前行的末尾插入文本（Append at the end of the line）。
#  • 按 o 键：在当前行下方创建新行并进入插入模式（open a new line below）。
#  • 按 O 键：在当前行上方创建新行并进入插入模式（open a new line above）。
# 推出插入模式
#  • 按 Esc 键可以从插入模式返回到普通模式
# 保存和退出
#  • 保存文件：按 : 键进入命令模式，然后输入 w（write），然后按 Enter 键
#  • 退出 vim：按 : 键进入命令模式，然后输入 q（quit），然后按 Enter 键。如果文件没有修改，会直接退出
#  • 如果文件有修改并且你想保存并退出，可以使用 :wq 或者 :x 或者:wq!
#  • 如果你不想保存修改并强制退出，可以使用 :q!
# ! 用来表示强制执行某个操作。结合在 :wq 后面，:wq! 变成了 强制保存并退出
```
