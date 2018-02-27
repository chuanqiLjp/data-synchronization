# Git命令初长成
>注：在进行任何 Git 操作之前，都要先切换到 Git 仓库目录，也就是先要先切换到项目的文件夹目录下。
1、git init：代表初始化 git 仓库；
2、git status：查看你当前 git 仓库的一些状态；
3、git add [文件名]：讲文件添加到待提交的缓存中；
4、git rm –cached：移除这个缓存；
5、git commit -m ‘该次提交的修改描述’：执行提交操作；
6、git log：查看所有产生的 commit 记录；
7、git branch：查看下当前分支情况（branch 即分支的意思，分支的概念很重要，尤其是团队协作的时候，假设两个人都在做同一个项目，这个时候分支就是保证两人能协同合作的最大利器了。举个例子，A, B俩人都在做同一个项目，但是不同的模块，这个时候A新建了一个分支叫a， B新建了一个分支叫b，这样A、B做的所有代码改动都各自在各自的分支，互不影响，等到俩人都把各自的模块都做完了，最后再统一把分支合并起来，执行 git init 初始化git仓库之后会默认生成一个主分支 master ，也是你所在的默认分支，也基本是实际开发正式环境下的分支，一般情况下 master 分支不会轻易直接在上面操作的，如果我们想在此基础上新建一个分支呢，很简单，执行 git branch a 就新建了一个名字叫 a 的分支，这时候分支 a 跟分支 master 是一模一样的内容，但是可以看到 master 分支前有个 * 号，即虽然新建了一个 a 的分支，但是当前所在的分支还是在 master 上，如果我们想在 a 分支上进行开发，首先要先切换到 a 分支上才行，所以下一步要切换分支）；
8、git checkout a：切换到分支a；
9、git checkout -b a：新建一个a分支，并且自动切换到a分支；
10、git merge：合并分支（A同学在a分支代码写的不亦乐乎，终于他的功能完工了，并且测试也都ok了，准备要上线了，这个时候就需要把他的代码合并到主分支master上来，然后发布。git merge 就是合并分支用到的命令，针对这个情况，需要先做两步，第一步是切换到 master 分支，如果你已经在了就不用切换了，第二步执行 git merge a ，意思就是把a分支的代码合并过来，不出意外，这个时候a分支的代码就顺利合并到 master 分支来了。）；
11、git branch -d a：删除a分支（如果删除失败可使用git branch -D a进行强制删除）；
12、git tag：可以查看历史 tag 记录（如果想要新建一个标签很简单，比如 git tag v1.0 就代表我在当前代码状态下新建了一个v1.0的标签，不同tag对应着不同的代码）；
13、git checkout v1.0：切换到 v1.0 tag的代码状态；
14、ssh-keygen -t rsa：指定 rsa 算法生成密钥，接着连续三个回车键（不需要输入密码），然后就会生成两个文件 id_rsa 和 id_rsa.pub ，而 id_rsa 是密钥，id_rsa.pub 就是公钥
15、git config --global user.name "stormzhang"和git config —-global user.email "stormzhang.dev@gmail.com"：设置下自己的用户名与邮箱
16、git push origin master：把本地代码推到远程 master 分支；
17、git pull origin master：把远程最新的代码更新到本地。一般我们在 push 之前都会先 pull ，这样不容易冲突。
18、git clone git@github.com:stormzhang/test.git：把 test 项目 clone 到了本地（git@github.com:stormzhang/test.git可以在GitHub项目的clone or down按钮下找到）；
19、git diff：查看代码做的改动


![Git的思维导图](http://upload-images.jianshu.io/upload_images/4143664-f920dc28e2e56b98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrevcommit --date=relative

[从0开始学习 GITHUB 系列之「初识 GITHUB」](http://stormzhang.com/github/2016/05/25/learn-github-from-zero1/)

[从0开始学习 GITHUB 系列之「加入 GITHUB」](http://stormzhang.com/github/2016/05/26/learn-github-from-zero2/)

[从0开始学习 GITHUB 系列之「GIT 速成」](http://stormzhang.com/github/2016/05/30/learn-github-from-zero3/)

[从0开始学习 GITHUB 系列之「向GITHUB 提交代码」](http://stormzhang.com/github/2016/06/04/learn-github-from-zero4/)

[从0开始学习 GITHUB 系列之「GIT 进阶」](http://stormzhang.com/github/2016/06/16/learn-github-from-zero5/)

[从0开始学习 GITHUB 系列之「团队合作利器 BRANCH」](http://stormzhang.com/github/2016/07/09/learn-from-github-from-zero6/)

[从0开始学习 GITHUB 系列之「如何发现优秀的开源项目？」](http://stormzhang.com/github/2016/07/28/learn-github-from-zero7/)

[从0开始学习 GITHUB 系列之「GITHUB 常见的几种操作」](http://stormzhang.com/github/2016/09/21/learn-github-from-zero8/)
