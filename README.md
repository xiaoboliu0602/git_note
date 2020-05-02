
# git 使用指南（Windows 系统）


>### git 安装
>```js
>直接下载安装包安装；
>```
>---

>### 创建 git 用户
>
>```js
>Git Bash ->  命令窗口 -> 
>
>$ git config --global user.name "Your Name"
>$ git config --global user.email "email@example.com"
>```
>---

>### 创建版本库
>
>```js
>$ mkdir firsthub
>
>$ cd firsthub/
>
>$ git init   // 使用 git init 命令把当前目录变成 Git 可以管理的仓库  生成的 .git 文件为隐藏文件
>
>Initialized empty Git repository in D:/User/workplace/git_place/firsthub/.git/
>
>$ git add readme.txt                   // 把文件添加到仓库  第一步
>
>$ git commit -m "wrote a readme file"  // 把文件提交到仓库  第二步
>
>[master (root-commit) e01c027] wrote a readme file
>1 file changed, 2 insertions(+)
>create mode 100644 readme.txt
>
>// 注意：可以多次添加文件到仓库，最后一次提交到仓库。
>```
>---

>### 时光穿梭
>
>```js
>修改文件 readme.txt 
>
>$ git status   // 查看工作区状态
>
>$ git diff     // 查看修改内容
>
>$ git add readme.txt   // 再次添加到仓库
>
>$ git commit -m "wrote a readme file"  // 再次提交到仓库
>```
>---

>### 版本回退
>
>```js
>$ git log  // 查看版本历史记录
>
>$ git log --pretty=oneline  // 单行显示
>
>$ git reset --hard HEAD^    // 回退到上一个版本
>
>$ git reset --hard *****    // 回到指定版本  ***** 为版本号
>
>$ git reflog  // 命令记录
>```
>---

>### 撤销修改
>
>```js
>$ git checkout -- readme.txt  // 让文件回到最近一次 git commit 或 git add 时的状态
>```
>---

>### 删除文件
>```js
>$ git rm test.txt
>
>$ git commit -m "remove test.txt"
>
>注意：git rm 用于删除一个文件。如果一个文件已经被提交到版本库，永远不要担心误删，但只能恢复到最新版本。
>```
>---
   
>### 添加远程仓库
>```js
>在用户主目录下创建 SSH Key:
>
>$ ssh-keygen -t rsa -C "******@163.com"  // 一路回车采用默认值
>
>// .ssh 目录里，有 id_rsa 和 id_rsa.pub 两个密钥对文件，前者为私钥，后者为公钥；
>
>// 在 GitHub 上打开 Account settings ， SSH Keys 页面，Add SSH Key 添加公钥；
>
>登陆 GitHub，Create a new repo 创建新仓库，添加仓库名称，其他默认；
>
>$ git remote add origin git@github.com:yourgithubID/hubname.git  // 关联远程库
>
>origin 是远程仓库的名字，yourgithubID 是 GitHub 账户名，hubname是 GitHub 上的仓库名；
>
>$ git push -u origin master  // 第一次推送本地仓库的所有内容到远程库上
>
>$ git push origin master     // 本地做了提交，就可以利用该命令推送到远程
>```
>---

>### 从远程仓库克隆
>```js
>从零开发最好的方式是先创建远程库，然后从远程库克隆。
>
>登录 GitHub，创建一个新仓库，取名 gitbob ；
>
>勾选 Initialize this repository with a README ，GitHub 自动创建 README.md 文件；
>
>远程库已经创建好；
>
>用 git clone 克隆一个本地库：
>
>$ git clone git@github.com:yourgithubID/hubname.git
>```
>---

>### ! ! !
>```js
>当本地工作未完成，提交后又会对其他成员造成影响，所以可以暂时提交到私有分支上，待工作完成后进行分支合并。
>```
>---


>### 创建与合并分支
>```js
>创建 dev 分支，然后切换到 dev 分支：
>   
>$ git branch dev    // 创建分支
>$ git checkout dev  // 切换分支
>Switched to branch 'dev'
>
>也可以这样：
>
>$ git checkout -b dev // -b 表示创建并切换
>Switched to a new branch 'dev'
>
>查看当前分支：
>
>$ git branch
>dev master
>
>在 dev 分支上正常提交：
>
>$ git add readme.txt 
>$ git commit -m "branch test"  // 提交到了 dev 分支
>
>把 dev 分支的工作成果合并到 master 分支上： 
>
>$ git merge dev  // merge 用于合并指定分支到当前分支
>
>合并完成后，就可以放心地删除 dev 分支：
>
>$ git branch -d dev
>Deleted branch dev (was b17d20e).
>
>查看此时的分支：
>
>$ git branch
>master
>
>switch  
>   
>在使用 checkout 时容易与撤销更改混淆，而使用 switch 可以避免混淆；
>
>创建并切换到新的的 dev 分支： 
>
>$ git switch -c dev
>
>直接切换到已有的 master 分支： 
>
>$ git switch master
>```
>---

>### 解决冲突
>```js
>解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容后再提交。
>```
>---

>### 分支策略管理
>```js
>master 分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
>
>dev 分支是不稳定的，到某个时候，比如 1.0 版本发布时，再把 dev 分支合并到 master 上，在 master 分支发布1.0版本；
>
>使用 --no--ff 参数，表示禁用 Fast forward ，以保存合并信息：
>
>$ git merge --no-ff -m "merge with no-ff" dev  // -m 说明本次合并要创建一个新的 commit 并将说明写入
>```
>---

>### Bug 分支
>```js
>保存现在的工作现场，去处理临时出现的 Bug ；
>
>保存当前现场，使用 stash 功能：
>
>$ git stash
>Saved working directory and index state WIP on dev: f52c633 add merge
>
>查看工作现场：
>
>$ git stash list
>stash@{0}: WIP on dev: f52c633 add merge
>
>恢复工作现场，同时删除 stash 内容：
>
>$ git stash pop
>
>在 master 分支上修复的 bug，想要合并到当前的 dev 分支：
>
>$ git cherry-pick <commit>  
>```
>---

>### Feature 分支
>```js
>添加一个新的功能，最好新建一个分支；
>
>$ git switch -c new-feature         // 新建并切换分支 new-feature
>
>$ git add new-feature               // 添加
>
>$ git commit -m "add featre new"    // 提交
>
>$ git switch dev                    // 切换到 dev 分支
>
>$ git merge --no-ff -m "merge with no-ff" dev  // 合并到当前分支
>
>$ git branch -D new-feature         // 强行删除 new-feature 分支
>```
>---

>### 多人协作
>```js
>当从远程仓克隆时，Git 已经自动将本地的 master 分支和远程的 master 分支对应了起来，远程仓库的默认名称是 origin；
>   
>查看远程仓库信息：
>
>$ git remote
>origin
>
>$ git remote -v
>origin  git@github.com:michaelliao/learngit.git (fetch)  // 抓取
>origin  git@github.com:michaelliao/learngit.git (push)   // 推送
>
>在本地创建和远程分支对应的分支：
>
>$ git checkout -b branch-name origin/branch-name
>
>本地分支与远程分支关联：
>
>'$ git branch --set-upstream branch-name origin/branch-name'  // 分支名称最后相同
>
>推送分支：
>
>$ git push origin master  // 推送 master 分支
>
>$ git push origin dev     // 推送 dev 分支
>
>注意：
>
>      master 分支是主分支，因此要时刻与远程同步；
>
>      dev 分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
>
>      bug 分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
>
>      feature 分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。
>
>推送失败，先 pull 抓取，然后再次推送：
>
>$ git pull                // 抓取远程的新提交
>
>$ git push origin dev     // 推送 dev 分支
>```
>---

>### Rebase 
>```js
>$ git rebase
>
>rebase 操作可以把本地未push的分叉提交历史整理成直线；
>
>rebase 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。
>```
>---

>### 创建标签（版本号）
>```js
>$ git branch
>dev master
>  
>$ git checkout master
>Switched to branch 'master'
>
>$ git tag v1.0  // 在最新提交的 commit 上打标
>
>$ git tag
>v1.0
>   
>$ git tag v0.9 f52c633  // 将标签打在 commit ID 上
>
>$ git show v0.1         // 查看标签说明
>
>$ git tag -a v0.1 -m "version 0.1 released" 1094adb  // 指定标签名称和说明文字
>```
>---

>### 操作标签
>```js
>$ git tag -d v0.1                   // 删除本地标签
>Deleted tag 'v0.1' (was f15b0dd)
>
>$ git push origin :refs/tags/v0.9   // 删除远程标签
>
>$ git push origin v1.0              // 推送指定标签到远程
>
>$ git push origin --tags            // 一次性推送全部未推送标签
>```
>---

>### 修改 git 默认路径
>```js
> 右键 git 快捷方式图标 -> 属性 -> 去掉目标栏中包含 cd 的片段 -> 起始位置改成目标路径
>```
>---

>### 参考链接
>
>https://www.liaoxuefeng.com/wiki/896043488029600
>
>https://www.runoob.com/git/git-tutorial.html
>
>---
