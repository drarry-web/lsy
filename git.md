# 第一章
## 1.1 关于版本控制

1. 什么是版本控制

   >所谓版本控制 是一种记录一个或者若干文件内容变化，以便将来查看修订特定版本修订情况的系统

2. 版本控制系统的分类

   - 本地版本控制系统

     ![image-20201215092340690](C:\Users\李思颖\AppData\Roaming\Typora\typora-user-images\image-20201215092340690.png)

     - 代表系统：**RCS**

     - 工作原理：在硬盘上保存补丁集，通过补丁，计算出各个版本的内容

     - 缺点：服务器宕掉就没有办法继续工作；

       ​           无法协同工作

       ​          中心数据库一旦损坏，就会丢失所有数据

   - 集中化的版本控制系统

     ![image-20201215092358552](C:\Users\李思颖\AppData\Roaming\Typora\typora-user-images\image-20201215092358552.png)

     - 代表CVCS centralized version control system
     - 具体产品： CVS Subversion Perforce
     - 工作原理：有一个单一即集中管理器保存所有文件的修订版本。协同工作的人都通过客户端连到服务器取出最新的文件或者提交更新。
  - 评价：解决了协同工作的问题，但仍有服务器压力过大，以及一旦服务器遭受攻击，所有数据都会消失的问题。
   
- 分布式版本控制系统
  
  ![image-20201215092425855](C:\Users\李思颖\AppData\Roaming\Typora\typora-user-images\image-20201215092425855.png)
  
    DVCS（Distributed Version Control System） 比如git
  
     - - 工作原理：客户端并不只提取最新版本的文件快照， 而是把代码仓库完整地镜像下来，包括完整的历史记录。 这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。 因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。

## 1.2 git

1. git是什么

   git将数据看作是小型文件的一系列快照。在git中每当你**提交更新或者保存项目状态**的时候，它会对当时的全部文件创建一个快照并为这个快照建立一个索引。如果没有修改，git不再重新弄存储该文件，而是保留一个链接指向之前的文件。

2. git的优势

   - 所有操作都在本地进行，不想集中版本控制那样，必须依靠网络和中央处理器进行通信，在没有vpn的情况下，很难进行提交或者处理。
   - git保证完整性 用文件内容的哈希值来建立索引 进行校验和
   - git一般只添加数据

3. git的安装

   ```
   sudo apt install git-all
   ```

4. git的配置

   每台计算机只用配置一次git

   ```
   git config //一个工具用控制git外观和行为的配置变量
   ```

   ```
   git config --list --show-origin  //查看git的所有配置文件以及他们所在的文件
   ```

   - 设置git的用户信息

     ```
     $ git config --global user.name "John Doe"
     $ git config --global user.email johndoe@example.com
     ```

     使用了--global选项 表示该命令只用运行一次，因为之后再系统上做任何事情都会使用这些信息。



## 第二章 Git基础-获取Git仓库

### 2.1什么是仓库

获取git项目仓库的两种方式

1. ***将尚未进行版本控制的仓库转化为git仓库***

   >- 进入到本地的目录之中
   >- 执行 git init命令

   效果：将会在改目录下创建一个.git的隐藏子目录。包含了git仓库的骨干，但项目里面还没有任何文件被追踪

2. **从其他服务器**克隆**一个已经存在的git仓库**

   ```python
git clone <url>
   
   #例子：
   git clone https://github.com/libgit2/ligbit2
   
   ```
   
   **效果**
   
   会在当前目录下面创建一个名字为“libgit2”的目录，并在该目录下初始话一个.git文件夹，从远程仓库拉取所有数据房子.git文件夹下面，然后从中读取最近版本的文件的拷贝。如果你进入到这个新建的文件夹libgit2. 所有的项目文件都已经在里面了
   
   ```python
   #指定新的目录名字为 mylibgit
   git clone https://github.com/libgit2/ligbit2 mylibgit
   ```
   
   

### 2.2 什么是跟踪 暂存和提交

- 文件的删除

  删除分为两种 

  - 一种是把本地的删除，也要删除在.git仓库中的快照，让git不在跟踪改文件

  ```c++
  rm test.md  //将本地文件从目录中删除掉
  git rm test.md //类似于git add 将修改提交到暂存区
  git commit //提交修改
  ```

  - 一种是只删除快照，即从暂存区中删除，但是不需要将其从本地上删除

    ```c++
    git rm --cached README  //使用--cached选项
    ```

    

- 文件的移动（重命名）

  ```c++
  git mv file_from file_to   //使用格式
  git mv README.md README
  ```

  结果：![](C:\Users\李思颖\AppData\Roaming\Typora\typora-user-images\image-20201215161009225.png)

- 日志的查看

  ```python
  git log
  ```

  详细信息：[日志查看](https://git-scm.com/book/zh/v2/Git-基础-查看提交历史)

- 撤销操作

  - [ ] 2.4基础 - 撤销操作

    

### 2.3  远程仓库的使用

1. 远程的含义

![image-20201215163625157](C:\Users\李思颖\AppData\Roaming\Typora\typora-user-images\image-20201215163625157.png)

```python
git remote  //查看已经配置的远程仓库服务器
git remote -v  //显得远程仓库的详细信息
git remote add pb(仓库简称) url|路径
git fetch pb //从pb仓库中拉取仓库中有但是我没有的 但是不合并到本地分支
git pull <remote> <branch>//从最初克隆的服务器上抓取数据并自动尝试合并到当前的分支
git push <remote> <branch> //推送到origin服务器（clone的时候会自动将远端仓库叫做origin服务器） 列子 git push origin master
//查看远程仓库
git remote show origin
//重命名远程仓库
git remote rename name_frem name_to
//删除远程仓库
git remote remove pb


```



###  2.4 打标签

- [ ] 没有看 记得补

## 2.5 别名

```python
git config --global alias.ci commit
```



## 第三章 分支管理

1. master分支 

   多次提交之后，会有一个指向最后那个提交对象的master分支。再下一次提交的时候，master分支会自动向前移动。

2. 分支的创建

   创建一个可以移动的指针，使用**branch**命令

3. 当前指针 **head**指针

​    head指针 指向当前所在的本地分支  

​    head是指向指针的

   指针是指向commit的blob对象的

  ***将head分支看作当前分支的别名***



   4.分支切换

   ```python
//分支的创建 只创建但不切换过去
git branch testing
//分支的切换 改变head指针的指向
git checkout testing   
//创建的同时 切换过去
git checkout -b 分支名字
//再创建分支后切换过去 再切换回master 是可以实现项目分流的
   ```



