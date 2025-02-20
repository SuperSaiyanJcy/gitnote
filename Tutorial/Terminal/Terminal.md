#### 创建文件夹

```shell
# mkdir 目录名
mkdir dir1
mkdir dir1 dir2 dir3
---------------------------------------------
# -p（创建父目录）： 使用 -p 选项时，mkdir 会创建指定目录的父目录（如果它们不存在）
mkdir -p /home/user/projects/my_project/src
# 创建 src 目录以及其父目录 my_project 和 projects（如果它们不存在的话）
---------------------------------------------
# 嵌套使用
mkdir -p /home/user/work/project/src /home/user/work/project/bin
```

#### 创建文件

```shell
touch file1.txt file2.txt file3.txt
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

#### 复制文件

```shell
cp myfile.txt copy_myfile.txt
# 复制文件到目录
cp file1.txt file2.txt file3.txt /home/user/backup/
# 复制目录，使用递归 -r
cp -r my_folder backup_folder
cp -r my_folder/ backup_folder/
# 强制覆盖
cp -f myfile.txt copy_myfile.txt
# -a archive，保留源文件的所有属性，如符号链接、权限、时间戳等，等价-dR --preserve=all
cp -a my_folder/ backup_folder/

```

#### 移动文件（重命名）

```shell
# mv 源文件/目录 目标文件/目录
mv myfile.txt /home/user/documents/
# 重命名
mv old_name.txt new_name.txt
mv file1.txt file2.txt /home/user/backup/
```

#### 删除文件

```shell
# 删除文件 rm 文件名
rm file1.txt
# 删除多个文件
rm file1.txt file2.txt file3.txt
# -r（递归删除）： 如果删除的对象是一个目录，使用 -r（recursive）选项。这个选项会递归地删除目录中的所有文件和子目录
rm -r my_folder
# -f（强制删除）： 使用 -f（force）选项可以强制删除文件，而不显示任何警告或提示，甚至在文件不存在时也不会报错
rm -f my_file.txt
---------------------------------------------------------------
# e.g.
# 删除文件夹及其内容（不提示）
rm -rf my_folder
# 删除文件夹及其内容（提示）
rm -ri my_folder
```
#### echo

```shell
# name="John"
echo "Hello, $name!"
echo "Hello" "World!"
# Hello
# World
# -n 默认情况下，echo在输出的文本末尾会自动添加一个换行符,使用 -n 不会输出换行符
echo -n "Hello, World!"
# -e echo默认不处理转义字符，-e启用转义
echo -e "Hello\nWorld"
# Hello
# World
# -E 选项可以显式地禁用转义字符,即使在某些系统中默认启用了转义字符处理
echo -E "Hello\tWorld"
# 输出特殊字符（例如 $、"、' 等）
echo "\$100"
# 使用 > 将文本写入文件（如果文件已存在，将覆盖文件内容）
echo "Hello, World!" > output.txt
# 使用 >> 将文本追加到文件末尾
echo "Hello again!" >> output.txt
# 将 echo 的输出通过管道（|）传递给另一个命令
echo "Hello, World!" | tr 'a-z' 'A-Z'
# 输出HELLO, WORLD!
```

#### find

```shell
# 按文件名称查找
find /path/to/dir -name "filename"
find /path/to/dir -name "*.txt"
# 按文件类型
find /path/to/dir -type f
find /path/to/dir -type d
# 按文件大小
find /path/to/dir -size +100M
find /path/to/dir -size -50k
find /path/to/dir -size 10M
# ......
```