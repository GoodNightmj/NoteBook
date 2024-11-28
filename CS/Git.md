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


## 撤销操作

用于提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。
提交信息写错了
```console
$ git commit --amend
```
刚刚进行一次提交后立马执行，快照保存不变，只是更改了提交信息
***
提交完了才发现漏掉了几个文件没有添加
该命令会将暂存区的文件全部提交，如果是第一种情况，即

```console
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

## 远程仓库
查看远程仓库：显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。
```console
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
```
添加远程仓库：
`git remote add <shortname> <url>`
查看远程仓库的信息：
`git remote show <name>`
删除
`git remote rm <name>`

## git alias
```console
$ git config --global alias.<to> <from>
```



## Git 分支

### 分支简介
例子：
假设现在有一个工作目录，里面包含了三个将要被暂存和提交的文件。 暂存操作会为每一个文件计算校验和（SHA-1 哈希算法），然后会把当前版本的文件快照保存到 Git 仓库中 （Git 使用 _blob_ 对象来保存它们），最终将校验和加入到暂存区域等待提交：
```console
$ git add README test.rb LICENSE
$ git commit -m 'The initial commit of my project'
```
当使用 `git commit` 进行提交操作时，Git 会先计算每一个子目录（本例中只有项目根目录）的校验和， 然后在 Git 仓库中这些校验和保存为树对象。随后，Git 便会创建一个提交对象， 它除了包含上面提到的那些信息外，还包含指向这个树对象（项目根目录）的指针。 如此一来，Git 就可以在需要的时候重现此次保存的快照。
Git 仓库中有五个对象：三个 _blob_ 对象（保存着文件快照）、一个 **树** 对象 （记录着目录结构和 blob 对象索引）以及一个 **提交** 对象（包含着指向前述树对象的指针和所有提交信息）。
![image.png](https://raw.githubusercontent.com/GoodNightmj/PicGo/master/202411281329190.png)
![image.png](https://raw.githubusercontent.com/GoodNightmj/PicGo/master/202411281415074.png)

常见命令
```
git branch <name> 创建一个分支
git branch -d <name> 删除某分支
git checkout <name> 切换到某分支
git checkout -b <name> 创建并切换到某分支

```

###  分支的新建与合并

**当你新建和合并分支的时候，所有这一切都只发生在你本地的 Git 版本库中 —— 没有与服务器发生交互。**

当切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。 Git 会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样。
三种合并情况
1. 合并master分支和hotfix分支![image.png](https://raw.githubusercontent.com/GoodNightmj/PicGo/master/202411281420411.png)
```console
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```
由于c4是c2的直接后继，那么 Git 在合并两者的时候， 只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧——这就叫做 “快进（fast-forward）”。
***
2.合并master分支和iss53分支，对于这种开发历史从一个更早的地方开始分叉开来（diverged）。即`master` 分支所在提交并不是 `iss53` 分支所在提交的直接祖先，Git 不得不做一些额外的工作。 出现这种情况的时候，Git 会使用两个分支的末端所指的快照（`C4` 和 `C5`）以及这两个分支的公共祖先（`C2`），做一个简单的三方合并。
![image.png](https://raw.githubusercontent.com/GoodNightmj/PicGo/master/202411281422880.png)
![image.png](https://raw.githubusercontent.com/GoodNightmj/PicGo/master/202411281425831.png)
和之前将分支指针向前推进所不同的是，Git 将此次三方合并的结果做了一个新的快照并且自动创建一个新的提交指向它。 这个被称作一次合并提交，它的特别之处在于他有不止一个父提交。

***
3.带有冲突的提交，即在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改
这种情况下，git会做出合并但不会进行提交，打开冲突文件会类似
```html
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```
解决冲突，你必须选择使用由 `=======` 分割的两部分中的一个，或者你也可以自行合并这些内容

###  远程分支
如果你在本地的 `master` 分支做了一些工作，在同一段时间内有其他人推送提交到 `git.ourcompany.com` 并且更新了它的 `master` 分支，这就是说你们的提交历史已走向不同的方向。 即便这样，只要你保持不与 `origin` 服务器连接（并拉取数据），你的 `origin/master` 指针就不会移动。
![image.png](https://raw.githubusercontent.com/GoodNightmj/PicGo/master/202411281557861.png)
如果要与给定的远程仓库同步数据，运行 `git fetch <remote>` 命令（在本例中为 `git fetch origin`）。  从中抓取本地没有的数据，并且更新本地数据库，移动 `origin/master` 指针到更新之后的位置。
![image.png](https://raw.githubusercontent.com/GoodNightmj/PicGo/master/202411281557887.png)
#### 推送
如果希望和别人一起在名为 `serverfix` 的分支上工作，你可以像推送第一个分支那样推送它。 运行 
`git push <remote> <branch>`=`git push <remote> <local branch > <remote branch> ` 相同 
