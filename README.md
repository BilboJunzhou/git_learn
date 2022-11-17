# Git学习笔记


## 什么是Git

---

Git是一个开源的分布式管理系统,用于敏捷高效的处理任何大小的项目  
其仅仅是一个版本控制系统,同时也是一个内容管理系统,工作管理系统。

**Git和SVN的区别:**
- Git是分布式的,SVN不是
- Git把内容按元数据的方式存储,而SVN是按文件存储
- Git分支和SVN分支不同
- Git的内容要更全

## Git环境安装配置 
---
Git下载地址: [https://git-scm.com](https://git-scm.com/)
之后傻瓜式安装就可以了,超级方便

**git config**工具是git用来配置用户工作环境的变量  
这些变量,决定了Git在各个环节的具体工作和行为,他们的存放路径如下

- ~/git/etc/gitconfig 文件存放通用配置,可以用以下命令来查看:
   ```shell
   $ git config --system --list
   ``` 
 - ~/.gitconfig 文件为用户目录下的配置文件只适用于该用户,通过改命令来查看
   ```shell
   $ git config --global --list
   ``` 


**用户信息设置**通常来配置个人的用户名和电子邮件地址
具体代码如下:
* 设置个人用户名
  ```shell
  $ git config --global user.name "runoob"
  ```
* 设置个人邮箱
  ```shell
  $ git config --global user.email "test@runoob.com"
  ```

## Git工作原理
---

Git本地有三个工作区:工作目录(Working Directory)、暂存区(Stage/Index)、资源库(Respository或Git Directory)/如果在加上远程的git仓库(Remote Directory)就可以划分为四个工作区  

- **工作区:** 电脑里面的工程目录
- **暂存区:** 存放在```.git ``` 下的```index```目录
- **资源库:** 工作区里面的隐藏目录```.git```,这个不算工作区,而是Git的版本库。


![output](https://note.youdao.com/yws/api/personal/file/WEB4e4d3b7e45cf1607c096bd7387e30bf8?method=download&shareKey=6dacdea9171258b704cde77a80077e53)


## Git项目搭建
---

创建仓库的方式有两种:一种是创建全新的本地仓库,一种是克隆远程仓库


### 本地仓库搭建

1. 建立全新的仓库,需要在Git管理的根目录执行:
    ```shell
    # 在当前目录创建一个新的Git代码库
    $ git init
    ```
    
    
2. 之后便可以看到,在项目目录里面多出了一个`.git`目录,版本等所有信息都在这个目录里面
	如图:
    ![](https://note.youdao.com/yws/api/personal/file/WEBcbcfd449663bb9069e5370036a7126c7?method=download&shareKey=3a8e484169d15083626b4a697b94d536)

### 远程仓库搭建
---

1. 另一种方式是远程克隆目录,将远程服务器上的仓库完全镜像一份至本地
	```shell
    # 克隆一个项目和他整个代码历史
    # git clone [url]
	```
 2. 同样会在根目录上出现一个`.git`的隐藏文件夹
	如图:
    ![](https://note.youdao.com/yws/api/personal/file/WEB872dd9c26b09d715d1133e78362e61fb?method=download&shareKey=ba235dceb1d3a612d60a2b2479cfbd84)
    
## Git文件操作

### 版本控制的四种状态:

---
 
版本控制就是对文件的版本控制,要对文件进行修改、提交等操作,首先要知道文件当前的状态,不然可能提交不了想提交的文件
 - **Untracked:** 未跟踪,此文件在文件夹中,但并没有加入到git库中,不参与版本控制,通过`git add`状态变为`Staged`
 
 - **Unmodify:** 文件已经入库,未修改,即版本库中的文件快照内容与文件夹中完全一致,这种类型的文件有两种去处,如果它被修改,则变为`Modified`. 如果使用git rm移除版本库,则变为`Unstracked`文件

- **Modified:** 文件已经修改,仅仅是修改,没有进行其他操作,这种文件也有两个去处,通过`git add`可进入暂存`staged`状态,使用`git checkout`则丢弃修改过,返回到`umodify`状态,这个`git checkout`即从库中去除文件,覆盖当前修改

- **Staged:** 暂存状态,执行`git commit`则将修改同步到库中,这时库中的文件和本地文件变为一致,文件为`Unmodify`状态。执行`git reset HEAD filename`取消暂存,文件状态变为`Modified`


### 文件状态操作

---
- 查看指定文件状态
	```shell
    $ git status [filename]
    ```
 - 查看所有文件状态
 	 ```shell
    $ git status
    ```
- 添加文件到缓存区
	```shell
    $ git add .
    ```
    
### 忽视文件

有些时候我们不想把某些文件纳入版本控制中,比如数据库文件,临时文件,设计文件等在主目录下建立".gitignore"文件,此文件有如下规则:

- 忽略文件中的空行或以井号(#)开始的行将会被忽略。

- 可以使用Linux通配符。例如:星号(*)代表任意多个字符,问号(?)代表一个字符,方括号([abc])代表可选字符范围,大括号({string1,string2,...})代表可选的字符串等。

- 如果名称的最前面有一个感叹号(!),表示例外规则,将不被忽略。

- 如果名称的最前面是一个路径分隔符(/),表示要忽略的文件在此目录下,而子目录中的文件不忽略。

- 如果名称的最后面是一个路径分隔符(/),表示要忽略的是此目录下该名称的子目录,而非文件(默认文件或目录都忽略)。


```vs
#为注释
*.txt        		#忽略所有 .txt结尾的文件,这样的话上传就不会被选中!
!lib.txt     		#但lib.txt除外
/temp      	#仅忽略项目根目录下的TODO文件,不包括其它目录temp
build/      	#忽略build/目录下的所有文件
doc/*.txt  	#会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```

### 分支操作
---

- **列出所有本地分支:**
	```shell
    $ git branch
    ```
- **列出所有远程分支:**
	```shell
    $ git branch -r
    ```
 - **新建一个分支,但依然停留在当前分支:**
	```shell
    $ git branch [branch-name]
    ```
- **新建一个分支,并切换到该分支:**
	```shell
    $ git checkout -b [branch]
    ```
- **合并指定分支到当前分支:**
	```shell
    $ git merge [branch]
    ```
 - **删除分支:**
	```shell
    $ git branch -d [branch-name]
    ```
 - **删除远程分支:**
	```shell
    $ git push origin --delete [branch-name]
    $ git branch -dr [remote/branch]
    ```


