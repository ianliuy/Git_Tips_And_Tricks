# yiyangiliu_personal_git_tips_and_tricks

[English](./README.md) | 中文

## 一、前置工作

##### 1. 设置全局用户名和邮箱

```
git config --global user.name "yiyangiliu"
git config --global user.email "i@yiyangliu.me"
```

##### 2. 设置自己的ssh公钥和私钥pair
 ```
ssh-keygen -t rsa -C "i@yiyangliu.me"
cat ~/.ssh/id_rsa.pub
打开gitlab -> settings -> SSH Keys
粘贴id_rsa.pub的内容到SSH Keys中，Add key。
```

##### 3. (optional) 将自己的公钥传到线上开发机，配置mapping。
##### 4. (optional) 配置缩写
```
# 配置别名
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.bra "branch -a"
git config --global alias.bk "checkout --"
git config --global alias.readd "reset HEAD"
git config --global alias.com "checkout master"
git config --global alias.ss status
git config --global alias.lg "log --graph -10"
git config --global alias.cm "commit -m" 
git config --global alias.br branch
git config --global alias.nb "checkout -b"
git config --global alias.return "checkout -- "
git config --global alias.po "push origin"
git config --global alias.plo "pull origin"
git config --global alias.pl pull
git config --global alias.ps push
git config --global alias.rbm "rebase master"
git config --global alias.rl reflog 

## 删除全局配置
git config --global --unset alias.xxx
git config --global --unset user.xxx
```

## 二、名词解释

##### 1. 名词解释

![](https://www.programmersought.com/images/955/957a0a5fe309f24588f1194a9e7fa8cb.png)
##### 2. 图中名词解释
```
Remote：远程仓库：https://xxx.com/xxx/tree/master
Repository：本地仓库
Workspace：工作区
Index / Stage：暂存区
```

## 三、工作流程
##### 1. master -> branch

```
git com # = git checkout master
git plo # = git pull origin

git nb mybranch # = git checkout -b mybranch
#  mybranch的命名规范：
#     如果是新增一个功能：branch_name = feature-<user>-<name>
#     如果是修复一个bug: branch_name = bug-<user>-<name>
#  注：其中user代表用户名，name代表功能或者bug名；
#     branch_name有命令规范要求，开发流程中必须按照规范要求命名。
```

##### 2. commit -> branch
```
git add . # = git add --all
git ss # = git status
git cm "ADD:xxxxx" # = git commit -m "ADD:xxxxx" # verbose列出diff的结果
    # feat: 新增功能
    # fix: bug修复
    # docs: 只更新文档内容
    # style: 更新代码并不影响现有的功能，只涉及简单的格式化，去空格，缺少分号等
    # refactor: 代码重构
    # perf: 代码更新用于提高性能
    # test: 单元测试相关代码更新，例如增加、删除、修改单元测试
    # chore: 辅助工具更新，例如构建脚本、Dockerfile等更新
```

##### 3. daily pull
代码开发过程中，branch和master分支经常被改变。如果想让自己的代码跟上开发的进度，就要时常更新自己的分支。
```
# 暂存自己分支的工作（optional）
git stash push

# 在master分支上拉取最新代码
g​it com # = git checkout master
git plo # = git pull origin

# 在自己分支上合并。达到最新状态
git co mybranch # = git checkout mybranch
git rbm # = git rebase master

# 在自己分支上保存工作
git stash pop
git add xxx
git cm "xxx" # = git commit -m "xxx"
git plo # = git pull origin
```

##### 4. 有的时候代码写错了，放弃更改
```
git add $filepath # 保存自己想要的更改，文件为粒度
git co . # 删掉其他的更改
```

##### 5. 代码出现冲突了
```
git com
git pull
git 
```
![](https://imgs.developpaper.com/imgs/2016926162921327.jpg)

## 四、commit规范
【强制】commit message 应该按照以下格式，每个commit message由三部分组成 header、body 以及 footer，同时每个 commit message 长度应该小于 100 个字符。
```
<type>(<scope>): <subject>
//空行
<body>
//空行
<footer>

1. <type>: 枚举值，标识本次commit类型，必填字段
  - feat: 新增功能
  - fix: bug修复
  - docs: 只更新文档内容
  - style: 更新代码并不影响现有的功能，只涉及简单的格式化，去空格，缺少分号等
  - refactor: 代码重构
  - perf: 代码更新用于提高性能
  - test: 单元测试相关代码更新，例如增加、删除、修改单元测试
  - chore: 辅助工具更新，例如构建脚本、Dockerfile等更新
2. <scope>: 本次变更文件的范围，可选字段。可以是某个类名、包名、函数名、文件名等，注意全小写
3. <subject>: 本次commit的主题，言简意赅，全小写，不需要使用`.`结尾，必填字段
4. <body>: 本次commit的详细内容，可以分点描述，可选字段
5. <footer>: 本次commit是否涉及到不兼容变更或者关闭某个issue，可选字段
```

// Case 1
```
style: modify notes
```

// Case 2
```
feat(kafka): add aliyun kafka client

add kafka package, aliyun kafka client for operator kafka instance:
1.support create new topic
2.support list topic
3.support get opic status
```

// Case 3
```
fix(kafka): modify calculate signature

modify kafka/client.go calculateSignature function, add sort url query params
Closes #392
```

// Case 4
```
docs: update readme

update README file, add content for use aliyun kafka 
```

// Case 4
```
style(http): add couple of missing semi colons

http/http.go add couple of missing semi colons
```

// Case 5
```
test(http): add unit test case

http_test.go add TestRequest function
```
