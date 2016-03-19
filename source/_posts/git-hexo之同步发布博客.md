title: git+hexo之同步发布博客
date: 2016-03-19 15:47:56
tags:
- git
- hexo
---
使用hexo搭建的github博客在电脑A上，其他电脑可以同步发布博客。
方法：文件夹下的public和.deploy_git文件夹只需要用hexo g命令即可生成，故不需要放到github上，只要将node_modules、scaffolds、source、themes、.gitignore、.gitmodules、_config.yml、db.json、package.json同步到github仓库中即可。

### **提交到github**
+ 若使用了其他人发布的主题，若直接提交，在github上该文件夹会变成灰色，而且在其他电脑上clone时，该文件夹下内容为空。
解决方法：(https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)
将别人的主题fork过来，然后在工程下添加子模块（假设添加了yilia主题），自动生成.gitmodules文件，然后将代码提交到远程仓库。

以我的为例，添加子模块yilia主题，放在路径themes/yilia下，提交到OliveLv.github.io的source分支
```shell
git submodule add <要添加的子模块git地址> <添加到工程指定的路径下>
git submodule add  https://github.com/OliveLv/hexo-theme-yilia  themes/yilia
git add .
git commit -a -m "commit source"
git push origin source
```
+ 还有使用了别人的主题，没有按照上面的方法做，在下载到其他地方时，会提示找不到该主题的submodule git而出错。。


### **其他电脑克隆**

```shell
git clone -b source --recursive https://github.com/OliveLv/OliveLv.github.io
或者
git clone -b source https://github.com/OliveLv/OliveLv.github.io
git submodule init
git submodule update --remote
```

### **提交修改**
通常方法就是
```git
git add .
git commit -a -m "comment"
git push origin source
```
对submodule进行了修改，提交方法（http://stackoverflow.com/questions/5814319/git-submodule-push）
首先进入submodule的文件夹，将修改的文件提交
然后返回工程目录，提交修改。。。即可

以我的为例，主题为yilia，放在目录themes/yilia下，使用的分支是develop；项目保存在分支source下。
```shell
cd themes/yilia
git commit -a -m "commit in submodule"
git push origin develop
cd ..
cd ..
git add .
git commit -a -m "commit in main project"
git push origin source
```
### **本地更新**
通常使用 git status查看本地与远程仓库的差异，使用git pull将远程的更新同步到本地。
注意：submodule与主工程是相互独立的。因此当远程代码有更新，本地同步时，也要分别对主项目、submodule进行同步。
网上有个直接一次性同步的命令（http://stackoverflow.com/questions/1030169/easy-way-pull-latest-of-all-submodules）

```shell
git pull origin source && git submodule --recursive
```

### **参考**
http://www.clarkhan.com/2015/06/03/install-hexo/
https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97
http://stackoverflow.com/questions/5814319/git-submodule-push
http://stackoverflow.com/questions/1030169/easy-way-pull-latest-of-all-submodules
