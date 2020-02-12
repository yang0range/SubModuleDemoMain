## Git
对于我们开发人员来说，**Git**的操作真的是再熟悉不过了，但是，我们真的利用好了**Git**了吗？或者，**Git**还给我提供了哪些更好功能，更丰富的功能帮助我们更好的管理代码，更好的完成项目的构建？

今天，我就来介绍一个对于团队十分有帮助的**Git**的功能——**Git Submodule**。

## Git Submodule

**Submodule**，直译过来就是子模块的意思，顾名思义就是控制子模块的意思。  

其实在蒋鑫的《Git权威指南》当中，有比较详细的介绍：项目的版本库在某些情况虾需要引用其他版本库中的文件，例如公司积累了一套常用的函数库，被多个项目调用，显然这个函数库的代码不能直接放到某个项目的代码中，而是要独立为一个代码库，那么其他项目要调用公共函数库该如何处理呢？分别把公共函数库的文件拷贝到各自的项目中会造成冗余，丢弃了公共函数库的维护历史，这显然不是好的方法。

简单来说每个公司，随着业务的发展或者针对许多项目我们开发和抽取出一套甚至公用的代码库，可以被多个项目效用，而这个代码库不是放在一个项目当中，而且我们单独作为一个代码库来使用，同时定期维护这套公共的代码库。  

但是对于其他的业务代码来说，他们该如何调用公用的代码库呢？难道是要一遍一遍的拷贝吗？这样不仅仅是操作麻烦，而且还丢弃了公共代码库的维护历史，甚至后期维护公共代码库的时候维护起来也十分的不便。其实**Git**早就帮我们解决了这一个问题，就是通过**git submodule**来解决！


## Git submodule用例
首先我们需要两个版本库
![](https://upload-images.jianshu.io/upload_images/3258163-ba4e2093f7de34e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从名称，我们就做了区分，一个是公共的版本库
```
https://github.com/yang0range/SubModuleDemoLib.git
```
另一个是引用公共版本库的主版本库
```
https://github.com/yang0range/SubModuleDemoMain.git
```
有了这两个版本库，我们就该介绍如何把两个版本库关联起来了
## Git submodule的操作了
这里我们先介绍**Git**命令的使用，接下来，我会介绍**TortoiseGit**的使用。  
#### 添加、提交过程
###### 1.首先Clone主项目

![](https://upload-images.jianshu.io/upload_images/3258163-949f08ec98fb81f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



###### 2.接下来Clone Lib项目

![](https://upload-images.jianshu.io/upload_images/3258163-99c2b781732ff765.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


可以看到，我们这两个项目都**Clone**成功了
![](https://upload-images.jianshu.io/upload_images/3258163-9f0c8f8c5d3c0305.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###### 3.接下来为主项目添加Submodule

用的命令是
```
git submodule add <repository> <path> //添加子模块
```
执行命令
```
 git submodule add https://github.com/yang0range/SubModuleDemoLib.git SubModuleDemoLib

```
![](https://upload-images.jianshu.io/upload_images/3258163-bc3b2ae0b4f4c11e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



###### 4.查看状态
接下来，我们执行命令
```
cat .gitmodules
```
可以看到**submodule**添加成功了
![](https://upload-images.jianshu.io/upload_images/3258163-0c27e29c6be581b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

同时我们可以看到目录下多了一个**.gitmodules**的文件
![](https://upload-images.jianshu.io/upload_images/3258163-d9d7fa879d216cba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###### 5.提交仓库
执行**git cmmit**命令  
添加成功之后，再执行  

**git push**指令

关于这两个我们最常用的指令，就不多介绍了。

之后我们查看**git log**就可以看到我们的提交记录了

![](https://upload-images.jianshu.io/upload_images/3258163-58080ccc4617b93e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


以上就是完整的添加过程。

#### Clone流程
对于一个新成员来说，如果**clone**新代码也是尤为重要。   
git为我们提供了两种克隆带有子模块版本库的方法

###### 方法一

首先**clone**父项目，再初始化**submodule**，最后更新**submodule**。初始化只需要做一次，之后每次**update**就可以了。
```
git clone <main>
cd <main>
git submodule init
git submodule update
```
![](https://upload-images.jianshu.io/upload_images/3258163-64b165d672e1f754.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这种方法，较为繁琐**Git**又为我们提供了另外一个方法

```
git clone main --recursive
```
![](https://upload-images.jianshu.io/upload_images/3258163-15cc3b2e512fa510.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这里采用的是**递归**参数--recursive

#### 修改子模块
对于子模块和主模块来说，两个库两个版本都是相对独立的，也就说对主模块来说，提交修改子模块不会对主模块造成任何影响。  
修改和更新的时候也都是我们常用的指令
```
git add
git commit 
git datus
git push
...
```

#### 更新子模块
对于子模块的更新，**Clone**有两种方法，自然更新也有两种方法
###### 方法一
先**pull**主模块，然后更新**submodule**
```
cd <main>
git pull
git submodule update
```
###### 方法二
进入子模块，然后切换到对应的分支，然后对子模块独立的**pull**
```
cd <submodule>
git checkout master
cd..
git submodule foreach git pull
```

#### 删除子模块
对于子模块来说，我们也会遇到移除，删除的操作
```
git rm <submodule> 
git status
git commit -m "remove submodule"
git push origin master
```

## TortoiseGit的Git submodule的使用
**TortoiseGit**的好处自然不必多说了。那么**TortoiseGit**如何操作带有**submodule**的项目呢？
#### 添加过程
**TortoiseGit**已经为我们考虑了添加子模块的功能。
![](https://upload-images.jianshu.io/upload_images/3258163-5bfe7b145bc0e08f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/3258163-c557537e082cc5d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### Clone过程
首先，我们**Clone**出主模块

![](https://upload-images.jianshu.io/upload_images/3258163-c8792e7b3e7a8b06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**Clone**之后，我们发现，只是把子模块的目录**Clone**下来了，并没有内容！ 

![](https://upload-images.jianshu.io/upload_images/3258163-61d90aa70af451d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


别着急，通过**Submodule Update**就可以了

![](https://upload-images.jianshu.io/upload_images/3258163-d3cb45b15d09c0e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/3258163-dd6ab54b7520a924.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

是不是很简单？

#### 提交、更新过程
之前我也说过，对于主模块和子模块来说，两个是相对独立了，所以在执行命令的时候，单独对主模块，和子模块分别操作就可以了。

这些就是**TortoiseGit**的基本操作，随便网上一搜就能找到了。

## 参考
[https://blog.csdn.net/zahuopuboss/article/details/51472842](https://blog.csdn.net/zahuopuboss/article/details/51472842)
[https://blog.csdn.net/wkyseo/article/details/81589477](https://blog.csdn.net/wkyseo/article/details/81589477)
[https://blog.csdn.net/xuanwolanxue/article/details/80609986](https://blog.csdn.net/xuanwolanxue/article/details/80609986)

## 最后
相信，通过这篇文章，大家对于Git Submodule的使用有了一个全面的了解。  

如果大家有任何疑问和问题欢迎关注我的公众号，或者给我留言！

### 动动小手指点赞，收藏，关注一键三连走一波吧！

### 欢迎关注公共号
#### 关注公众号会有更多收获！
![](https://upload-images.jianshu.io/upload_images/3258163-635809c97c6586e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 个人微信
#### 我们一起讨论，进步，提高！
![](https://upload-images.jianshu.io/upload_images/3258163-26e6f43536b369d4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





