
----
## :pushpin: 终端中一些前置小知识

### 1. 引用/变量替换 `'` `"` `$S`

- **单引号 (`' '`)：** 所见即所得，里面写什么就是什么。
- **双引号 (`" "`)：** 保留大部分字符的字面意思，但允许“变量”和“命令”发挥作用。

**1. 单引号 `' '` (强引用)**
单引号是最严格的引用。在单引号内部，没有任何字符具有特殊含义。
- **特性：**
  - 变量不替换：`$HOME` 只是 5 个字符，不会变成你的路径。
  - 转义不生效：`\n` 就是两个字符 `\` 和 `n`，不会变成换行。
  - 命令不执行：反引号 `` ` `` 或 `$()` 都会被当成普通文本。
- **示例：**
```bash
name="jcy"
echo 'Hello $name'
# 输出: Hello $name
# (系统完全忽略了 $ 符号)
```
- **适用场景：**
  - 当你想要原封不动地输出一段包含特殊符号（如 `$`, `\`, `*`, `!`, `"`）的文本时。
  - 定义 `alias` 时，为了防止变量在定义瞬间被展开（如刚才那个 `alias show_dir='echo $PWD'` 的例子）

**2. 双引号 `" "` (弱引用)**

双引号比较智能。它会把大多数特殊字符（如空格、`*`、`?`）当成普通字符保护起来，但**会允许以下 3 种特殊情况发生：**
   1. `$` **(美元符号)：** 用于变量替换（`$VAR`）或命令替换（`$(cmd)`）。
   2. `\` **(反斜杠)：** 仅当它后面跟着 `$,` `"`, `\`, 或换行符时，起转义作用。
   3.  `` ` `` **(反引号)：** 用于命令替换（老式写法）。
  - **示例：**

```bash
name="jcy"
echo "Hello $name"
# 输出: Hello jcy
# (系统看到双引号，就把 $name 换成了真实的值)

echo "当前路径是: $(pwd)"
# 输出: 当前路径是: /home/jcy
# (命令被执行了)
```
  - **适用场景：**
    - 当你需要在字符串里插入变量时。
    - 当你处理**可能包含空格的文件名**时（**这是双引号最重要的用途**，见下文）。
  
**3. 最经典的坑：文件名中的空格**

这是双引号存在的最大意义之一，假设有一个文件名叫 `my file.txt` (中间有个空格)。
```bash
filename="my file.txt"

# 1. 不加引号 (危险 ❌)
rm $filename
# 等同于执行: rm my file.txt
# 结果: 删除了 "my" 和 "file.txt" 两个文件，报错找不到 "my"。

# 2. 单引号 (错误 ❌)
rm '$filename'
# 等同于执行: rm $filename
# 结果: 试图删除一个名字真的是 "$filename" 的文件。

# 3. 双引号 (正确 ✅)
rm "$filename"
# 等同于执行: rm "my file.txt"
# 结果: 成功删除那个带空格的文件。
```
**结论：** 在脚本中引用变量（尤其是文件名变量）时，养成无脑加双引号的习惯：`cp "$src" "$dest"`。

**4. 引号嵌套使用**

```bash
# 这是最简单的。双引号里的单引号就是普通字符
echo "It's a nice day"
# 输出: It's a nice day

# 这也是普通字符。
echo 'He said "Hello"'
# 输出: He said "Hello"
```

| 特性               | 不加引号              | 单引号 `' '`          | 双引号 `" "`            |
| :----------------- | :-------------------- | :-------------------- | :---------------------- |
| **空格处理**       | 视为分隔符 (分割参数) | 视为普通字符          | 视为普通字符 (保护整体) |
| **$变量替换**      | ✅ 替换                | ❌ **不替换**          | ✅ 替换                  |
| **$(cmd)命令执行** | ✅ 执行                | ❌ **不执行**          | ✅ 执行                  |
| **通配符 (*, ?)**  | ✅ 展开 (变成文件名)   | ❌ 不展开 (就是星号)   | ❌ 不展开 (就是星号)     |
| **转义符 (\\)**     | ✅ 生效                | ❌ **失效** (显示 `\`) | ⚠️ 部分生效              |

### 2. 变量命名潜规则

一些约定俗成

- **全大写 (UPPERCASE) = 环境变量 / 全局变量**
  - 这些变量通常由操作系统或 Shell 预设，对当前 Shell 及其**所有子进程**生效。
  - **例子：** `PATH`, `HOME`, `USER`, `PWD`, `SHELL`, `EDITOR`。
  - **禁忌：** 永远不要在脚本里把自己的变量命名为 **PATH**，否则所有命令都找不到（`ls command not found`），因为覆盖了系统查找命令的路径。

- **全小写 (lowercase) = 局部变量 / 用户自定义变量**
  - 仅在当前脚本或当前 Shell 这一会儿有效。
  - **例子：** filepath, count, my_config。
  - **最佳实践：** 写脚本时，自己的变量无脑用小写，绝对安全。

### 3. "沉默是金"与返回码 `$?`

在 C 语言里，`main` 函数最后通常写 `return 0;`。这个 `0` 给了 `Shell`。

- **规则：**
  - `0` 代表 **成功 (Success)**。
  - `非0` 代表 **失败/错误 (Error)**。

- **如何查看：** 命令执行完后，立刻输入 `echo $?` 可以看到上一个命令的“遗言”。

- 实战使用 ( `&&` 和 `||`)：
```bash
mkdir build && cd build && cmake .. && make
```
  - `&&` (AND)：只有前面成功了 (返回0)，才执行后面。
  - `||` (OR)：只有前面失败了 (非0)，才执行后面（常用于报错）。
  - `make || echo "编译炸了！"`
    ```text
    情况一：make 成功了 (True)
    逻辑变成：True || ???
    Shell 想：“前面已经是真了，‘或’逻辑已经满足了，后面那个不用看了。”
    结果：echo 被短路跳过（不执行）。
    情况二：make 失败了 (False)
    逻辑变成：False || ???
    Shell 想：“前面是假，但我还得看看后面那个是不是真，不然整个式子就假了。”
    结果：必须执行 echo。
    ```
### 4. 三个
---
## Linux常用终端命令

### 1. `mkdir` `touch`创建文件夹/文件

```bash
# mkdir 目录名
mkdir dir1
mkdir dir1 dir2 dir3
---------------------------------------------
# -p（创建父目录）： 使用 -p 选项时，mkdir 会创建指定目录的父目录（如果它们不存在）
mkdir -p project/drivers/uart # 相对路径创建
mkdir -p /tmp/test/logs       # 绝对路径创建
mkdir -p /home/user/work/project/src /home/user/work/project/bin
---------------------------------------------
# 场景：一次性创建头文件和源文件
touch main.c main.h utils.c utils.h
# 场景：一次性创建 10 个测试文件 (file1.txt 到 file10.txt)
touch file{1..10}.txt
# -c 如果指定文件不存在，touch将不会创建该文件
touch -c myfile.txt
# -a 更新文件访问时间
touch -a myfile.txt
# -m 更新文件修改时间
touch -m myfile.txt
# -t 设置自定义时间戳（访问、修改时间）[[CC]YY]MMDDhhmm[.ss] CC：世纪
touch -t 202312250830.45 myfile.txt
# -d 设置指定时间，访问时间和修改时间设置为2023年12月25日08:30:45
touch -d "2023-12-25 08:30:45" myfile.txt
```
#### ⚠️注意区分
| 命令                       | 作用                 | 文件已存在时会发生什么？        |
| :------------------------- | :------------------- | :------------------------------ |
| **`touch file.txt`**       | 创建**空**文件       | **内容不变**，只更新时间 (安全) |
| **`echo "hi" > file.txt`** | 创建**带内容**的文件 | **内容被覆盖/清空** (危险 ⚠️)    |
| **`nano file.txt`**        | 打开编辑器创建       | 打开文件进行编辑                |

#### 👉小技巧
  假设你在写 `C` 语言代码，用 `make` 来编译。有时候你只改了配置，但文件代码没变，`make` 认为不需要重新编译。 这时候你可以 `touch` 一下源文件。
```bash
touch main.c
```
### 2. `ln`(link)和`unlink`
在 Linux 开发中，99% 的情况都是用 “软链接” (Soft Link / Symbolic Link)。 相当于 Windows 的 “快捷方式”。
```bash
# ln -s  [真实存在的源文件]      [你想创建的快捷方式]
ln -s /home/jcy/my-config/.zshrc  /home/jcy/.zshrc
ln -s /opt/homebrew/bin/7zz /opt/homebrew/bin/7z
# 更安全的删除方式（仅删除链接本身）
unlink /opt/homebrew/bin/7z
# rm也可删除软链接，但是如果指向文件夹，加了/，会删除原始文件夹下的内容！
rm /opt/homebrew/bin/7z
```
- 使用绝对路径，防止出现一些意想不到的问题

### 3. `cp` `mv` 复制/移动/重命名

### 4. `list` 列出文件

### 5. `pwd` 查看绝对路径

```bash
jcy@ubuntu:~$ pwd
/home/jcy
```










## :pushpin:SSH

### 1. 生成密钥
```bash
ssh-keygen -t rsa -b 4096   # 默认路径为：~/.ssh
ssh-keygen -t ed25519
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
-  `-t rsa` ：指定密钥类型为RSA（常用类型） `-t ed25519`
-  `-b 4096` ：指定密钥位数为4096位，增加密钥安全性（2048/4096?）
-  `-C "your_email@example.com"` ：为密钥添加注释
- 使用`SSH`密钥进行无密码登录需要将公钥 `id_rsa.pub` 或 `id_ed25519.pub` 添加到远程服务器的 `~/.ssh/authorized_keys` 文件中(没有则手动添加)

`id_rsa`为私钥，`id_rsa.pub` 为公钥，公钥相当于锁，私钥相当于是钥匙，把锁放于远程服务器的门上，然后自己有钥匙前去开锁

### 2. 设置SSH权限

Linux的SSH对权限十分敏感，权限放太开，SSH会为了安全拒绝登录
```bash
# 只有你自己能读写这个文件夹
chmod 700 ~/.ssh
# 只有你自己能读写这个文件
chmod 600 ~/.ssh/authorized_keys
```

### 3. 可选配置/设置

- 卸载并重装 SSH 服务：
```bash
sudo apt remove openssh-server
sudo apt install openssh-server
```
- 修改配置文件
```bash
sudo nano /etc/ssh/sshd_config
```
按需修改（取消注释 `#` 号）：

- `Port 2222`
  在Windows-wsl情况下下，Windows自带openssh服务会占用22端口
- `PermitRootLogin no` 
  `yes` `no` `prohibit-password` 允许root登录，但仅限于密钥
- `ListenAddress 0.0.0.0` (监听所有网卡)

`nano`下，`^+O` 保存(O,Write **O**ut)，弹出提示，`File Name to Write: ...` 回车确认, `^+X` 退出(E**x**it)。

### 4. 启动/查询SSH服务
```bash
sudo service ssh start
sudo service ssh status # 查看ssh运行状态
```

### 5. SSH连接（局域网/本地/穿透）
```bash
# 格式：ssh -p 端口 Linux用户名@Linux的IP
ssh -p 22 jcy@192.168.1.100 # 远程
ssh -p 22 jcy@127.0.0.1     # 本地
```

## :pushpin:Linux常见配置

1. 权限设置
Linux的SSH对权限十分敏感，权限放太开，SSH会为了安全拒绝登录
```bash
# 只有你自己能读写这个文件夹
chmod 700 ~/.ssh
# 只有你自己能读写这个文件
chmod 600 ~/.ssh/authorized_keys
```

2. 在 Linux 终端，强制停止当前正在运行的命令，`^+C` (ctrl)

3. 加入自启/查看用户名/查看IP....
```bash
# 查看用户名
whoami
# 获取IP
hostname -I 
# 设置自启动
sudo systemctl enable ssh
```

4. 添加到系统环境变量
```bash
# 临时添加，进当前窗口有效
export PATH=$PATH:/你的/自定义/路径
export PATH=$PATH:/home/jcy/my-toolchain/bin
# 永久添加，当前用户生效
# 确认shell echo $SHELL 如果是 /bin/bash，就改 .bashrc
nano ~/.bashrc
# My Custom Path
export PATH=$PATH:/home/jcy/my-toolchain/bin
```
- `export PATH=`：声明环境变量。
- `$PATH`：非常重要！意思是“保留原来的所有路径”。如漏写，系统基本命令（如 `ls`, `cd`）都会失效。
- `:`：这是分隔符（就像 Windows 里的分号 `;`）。

```bash
# 立即生效
source ~/.bashrc
# 验证
echo $PATH
```


## `apt`包管理器

### 1. `build-essential` 基础开发工具链，含`gcc g++ make libc6-dev(GNU c库)`

```bash
# 1. 更新软件源列表（新装系统必须先执行这一步，否则可能找不到包）
sudo apt update
# 2. 安装构建全家桶
sudo apt install build-essential
```
### 2. 安装/卸载/清理
```bash
# 1. 更新软件源列表（新装系统必须先执行这一步，否则可能找不到包）
sudo apt update
sudo apt install <包名>
# 只删除软件本身，但保留配置文件（比如你修改过的 /etc/ 下的配置）。以后重装，配置还能用
sudo apt remove <包名>
# 删除软件本身，并且删除所有的全局配置文件
sudo apt purge <包名>
# 安装软件 A 时，它可能自动安装了依赖包 B 和 C。卸载 A 后，B 和 C 就变成了没人用的垃圾文件。 建议每次卸载完后，顺手运行一下这个命令来清理系统垃圾
sudo apt autoremove
```


## Linux网络代理相关问题(WSL/OrbStack)

测试代理是否生效
```bash
curl -I https://github.com
```
不可用 `ping` ， `ping` 使用的是 `ICMP` 协议，代理软件通常只代理 `TCP/UDP`
- 如果返回 `HTTP/2 200` 或 `301` 等信息，说明网络是通的，开发环境没问题。
- 如果长时间没反应或报错，说明终端的代理没配置好。


## Windows-WSL

```bash
wsl --shutdown
```

### 1. 镜像Windows网络
仅限于Windows11(22H2以后),在用户目录下,找到 `.wslconfig` 文件，没有则手动创建，写入一下内容，重启WSL
```text
[wsl2]
networkingMode=mirrored
dnsTunneling=true
firewall=true
autoProxy=true
```
Windows下remote-ssh配置
```text
Host 127.0.0.1
  HostName 127.0.0.1
  Port 2222
  User jcy
  IdentityFile "C:\Users\JCY\.ssh\id_rsa"
```
### 2. 非镜像网络
```bash
# 端口转发
netsh interface portproxy add v4tov4 listenport=2222 listenaddress=0.0.0.0 connectport=2222 connectaddress=172.x.x.x
# 删除端口转发
netsh interface portproxy delete v4tov4 listenport=2222 listenaddress=0.0.0.0
```



