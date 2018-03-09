# 常用的Hexo指令

[hexo文档](https://hexo.io/zh-cn/docs/)

[next的主题](http://theme-next.iissnan.com/getting-started.html)

```
分类：F:\ljpBlog>hexo new page categories，执行结果：INFO Created: F:\ljpBlog\source\categories\index.md
TAG: F:\ljpBlog>hexo new page tags ,执行结果：INFO Created:F:\ljpBlog\source\tags\index.md
hexo new post "博客名"
开启本地访问：hexo s
更新GitHub： hexo d -g
清除缓存：hexo clean
报错：ERROR Deployer not found: git 解决：F:\ljpBlog>npm install --save hexo-deployer-git
```

# A电脑（最开始搭建的电脑）：
删除theme下next主题下的git、GitHub、.gitignore文件，在.gitignore文件添加
```
/.deploy_git
/public
```
在本地博客根目录下使用git指令上传项目到Github:
```
// git初始化
git init
// 添加仓库地址
git remote add origin git@github.com:chuanqiLjp/chuanqiLjp.github.io.git
// 新建分支并切换到新建的分支
git checkout -b 分支名
// 添加所有本地文件到git
git add .
// git提交
git commit -m ""
// 文件推送到hexo分支
git push origin hexo
```

# B电脑(后续的电脑)
```
git clone -b hexo git@github.com:chuanqiLjp/chuanqiLjp.github.io.git
在本地新拷贝的chuanqiLjp.github.io文件夹下通过CMD(或Git bash)依次执行下列指令：
npm install hexo、
npm install、
npm install hexo-deployer-git（记得，不需要hexo init这条指令）。
——————————————————————
提交代码前：git pull origin master  //同步服务器最新的代码
git add .、
git commit -m “…”、
git push origin hexo、
```
