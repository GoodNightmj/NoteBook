## Git基础
### 克隆远程仓库事项
- 如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以通过额外的参数指定新的目录名：

```
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

### git的三种状态
* Git 有三种状态，你的文件可能处于其中之一： **已提交（committed）**、**已修改（modified）** 和 **已暂存（staged）**。
	- 已修改表示修改了文件，但还没保存到数据库中。
	- 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
	- 已提交表示数据已经安全地保存在本地数据库中。
![image.png](https://raw.githubusercontent.com/GoodNightmj/PicGo/master/202411271924769.png)
基本的 Git 工作流程如下：

1. 在工作区中修改文件。
    
2. 将你想要下次提交的更改选择性地暂存，这样只会将更改的部分添加到暂存区。
    
3. 提交更新，找到暂存区的文件，将快照永久性存储到 Git 目录。


* 工作目录下的每一个文件都不外乎这两种状态：**已跟踪** 或 **未跟踪**。
![image.png](https://raw.githubusercontent.com/GoodNightmj/PicGo/master/202411271905228.png)


git add这是个多功能命令：
	可以用它开始跟踪新文件
	或者把已跟踪的文件放到暂存区
	还能用于合并时把有冲突的文件标记为已解决状态

### gitignore
一个 `.gitignore` 文件的例子：

```
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```


### 查看已暂存和未暂存的修改

git diff命令比较的是工作目录中当前文件和暂存区域快照之间的差异。 也就是修改之后还没有暂存起来的变化内容。
git diff --staged比对已暂存文件与最后一次提交的文件差异


提交时记录的是放在**暂存区域**的快照。 任何还未暂存文件的仍然保持已修改状态，可以在下次提交时纳入版本管理。 每一次运行提交操作，都是对你项目作一次快照，以后可以回到这个状态，或者进行比较。


### 删除文件
使用`git rm`命令从 Git 中移除某个文件，如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 `-f`（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删尚未添加到快照的数据，这样的数据不能被 Git 恢复。
你想让文件保留在磁盘，但是并不想让 Git 继续跟踪可以使用 `--cached` 选项：
```console
$ git rm --cached README
```


### 移动文件
要在 Git 中对文件改名，可以这么做：

```console
$ git mv file_from file_to
```

相当于运行了下面三条命令：

```console
$ mv README.md README
$ git rm README.md
$ git add README
```

如此分开操作，Git 也会意识到这是一次重命名，所以不管何种方式结果都一样。 两者唯一的区别在于，`git mv` 是一条命令而非三条命令，直接使用 `git mv` 方便得多。 不过**在使用其他工具重命名文件时，记得在提交前 `git rm` 删除旧文件名，再 `git add` 添加新文件名。**


### 查看提交历史（git log）
 `git log` 的常用选项

|选项                | 说明                                                                    |
| ----------------- | --------------------------------------------------------------------- |
| `-p`              | 按补丁格式显示每个提交引入的差异。                                                     |
| `--stat`          | 显示每次提交的文件修改统计信息。                                                      |
| `--shortstat`     | 只显示 --stat 中最后的行数修改添加移除统计。                                            |
| `--name-only`     | 仅在提交信息后显示已修改的文件清单。                                                    |
| `--name-status`   | 显示新增、修改、删除的文件清单。                                                      |
| `--abbrev-commit` | 仅显示 SHA-1 校验和所有 40 个字符中的前几个字符。                                        |
| `--relative-date` | 使用较短的相对时间而不是完整格式显示日期（比如“2 weeks ago”）。                                |
| `--graph`         | 在日志旁以 ASCII 图形显示分支与合并历史。                                              |
| `--pretty`        | 使用其他格式显示历史提交信息。可用的选项包括 oneline、short、full、fuller 和 format（用来定义自己的格式）。 |
| `--oneline`       | `--pretty=oneline --abbrev-commit` 合用的简写。                             |
