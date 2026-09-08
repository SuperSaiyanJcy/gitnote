## Mac一些技巧

```bash
# 启用安装任何来源
sudo spctl --master-disable         # 需在 设置-隐私与安全性 中启用允许任何来源
xattr -cr /Applications/XXX.app     # 提示损坏可以尝试一下这样做
```

## homebrew

```bash
# install
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# uninstall
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
------------------------------------------------------------------------------------------
# 软链接 类似创建快捷方式 7zz,习惯使用使用7z
ln -s /opt/homebrew/bin/7zz /opt/homebrew/bin/7z
# 更安全的删除方式（仅删除链接本身）
unlink /opt/homebrew/bin/7z
------------------------------------------------------------------------------------------
```
- **终端中homebrew会提示输入3行指令到终端用以添加到系统环境变量**
- ==> Next steps:
- Run these commands in your terminal to add Homebrew to your PATH:
- echo >> /Users/jcy/.zprofile
- echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> /Users/jcy/.zprofile
- eval "$(/opt/homebrew/bin/brew shellenv zsh)"
---
## `.zprofile` `.zshrc` 系统环境变量/alias/...

若目录下只有 `.zprofile` 而没有 `.zshrc`，是因为安装的一些软件（比如 Homebrew）自动创建了 `.zprofile` 来设置环境变量，但还没人去创建 `.zshrc`，macOS 现在默认不自动帮创建，可以手动创建

1. `.zprofile` (环境配置)：

- 用途：设置 `PATH`（路径）、加载 Homebrew 等。
- 触发时机：用户登录时运行一次。
- 现状：因为 macOS 的终端默认把每个新窗口都当作“登录 Shell”，所以写在这里的东西通常也有效。
- 
2. `.zshrc` (交互配置)：

- 用途：设置 alias (别名)、命令行提示符主题、自动补全等。
- 触发时机：每次你打开一个新终端窗口，或者在终端里输入 `zsh` 进入子 Shell 时都会运行。
- 关键点：别名（Alias）属于这里！

```bash
# OrbStack Quit Alias
alias qorb="osascript -e 'quit app \"OrbStack\"'"
# mac下gcc默认是clang，设置别名
alias gcc='gcc-14'
alias gcc="gcc-14"
```

## Mac终端

```bash
osascript -e 'quit app "OrbStack"'
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
