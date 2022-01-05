---
title: 'git:一些撤销操作'
date: 2021-02-07 19:43:43
tags: Tools
---


<br>


<img src="git-一些撤销操作/0.png" width = 100% height = 50% /> 

<br>



[如何撤销 Git 操作？](http://www.ruanyifeng.com/blog/2019/12/git-undo.html)


<br>


### 一、撤销提交

<br>

`git revert HEAD`

<br>


撤销上次提交. 

(会在当前提交后面，新增一次提交，抵消掉上一次提交导致的所有变化,所有记录都会保留)



<img src="git-一些撤销操作/2.png" width = 100% height = 50% /> 


<img src="git-一些撤销操作/3.png" width = 100% height = 50% /> 



<br>


<img src="git-一些撤销操作/4.png" width = 100% height = 50% /> 


<img src="git-一些撤销操作/5.png" width = 100% height = 50% /> 




<br>


---


<br>


### 二、撤销某次merge

<br>


`git merge --abort`


<br>


---


<br>


### 三、替换上一次提交

<br>


`git commit --amend -m "新的提交信息"`

<img src="git-一些撤销操作/6.png" width = 100% height = 50% /> 


<img src="git-一些撤销操作/7.png" width = 100% height = 50% /> 


<br>


可以修改上一次的提交信息

<img src="git-一些撤销操作/8.png" width = 100% height = 50% /> 


<img src="git-一些撤销操作/9.png" width = 100% height = 50% /> 



<br>


<img src="git-一些撤销操作/10.png" width = 100% height = 50% /> 


<img src="git-一些撤销操作/11.png" width = 100% height = 50% /> 



<br>


---


<br>


### 四、从暂存区撤销文件

<br>

如果不小心使用了`git add`命令, 把一个文件本不想添加到暂存区的文件加到了暂存区，可用下面的命令撤销



`git rm --cached [filename]`



<img src="git-一些撤销操作/12.png" width = 100% height = 50% /> 





---


<br>



更多:


[Git的撤销和回滚命令总结](https://www.jianshu.com/p/38921d19ba0a)

[恢复GIT不同区域的修改](https://kingofamani.gitbooks.io/git-teach/content/chapter_2/repo.html)



<img src="git-一些撤销操作/1.png" width = 100% height = 50% /> 




