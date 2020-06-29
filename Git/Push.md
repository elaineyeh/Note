# How to push to Github
## Step 1. 
在要進行版本控制的資料夾下
```shell=
$ git init
```
## Step 2. 
`firename` 為有更動、要做紀錄的檔案名稱
```shell=
$ git add firename
```
>**若是要整個專案都更新則可以將 `firename` 的位置改為 `.`**
>>```shell=
>>$ git add .
>>```

#### 若是一直 add 沒有 commit，當 commit 時只會紀錄最後一次的 add

## Step 3.
為此次記錄做註解
```shell=
$ git commit -m "comment"
```

## Step 4.
推上 Github，存上遠端
```shell=
$ git push
```

---
# The basic instructions of Git
### 比較檔案版本前後差異  
在未 git add 之前跟上一個版本的比較
```shell=
$ git diff  
```
### 看目前 commit 過的版本  
```shell=
$ git log  
```
### 看目前有沒有檔案更動  
```shell=
$ git status  
```
### 跳回之前的版本
```shell=
$ git checkout
```
### Find log with specific condition
> $ git log --online --author="Andy"  
>> find log which author is "Andy".  

> $ git log --online --grep="fixed"  
>> find commit which has write "fixed".

> $ git log -s "git"
>> find commit which has write "git".

>$ git log --online --since="9am" --until="12pm" --after="2020-07"
>> find commit which upload after 2020 July between nine to tweleve.
### Remove file
> $ rm welecome.html  
>> Need to add file again 

> $ git rm welcome.html  
>> Already add  
### Untracked file
```shell=
$ git rm welcome.html --cached
```
### Rename file
> $ mv hello.html welcome.html
>> Need to add file again

> $ git mv hello.html welcome.html
>> Already add
### Modify the last time commit
```shell=
$ git commit --amend -m "Hello"
```
**last time commit only!**  
#### Insert another file in last time commit
```shell=
$ git add join.html  
$ git commit --amend --no-edit
```
--no-edit mean no edit commit message  
**Do not use in the file which has been pushed.**  
### Check specific file log
> $ git log filename
>> Check the file log

> $ git log -p filename
>> Check the file different in each log  
### Check who write the file
```shell=
$ git blame filename  
$ git blame -L 5,10 filename  #Display only line 5 to 10.
```

---
# Some instructions about directory
### Add empty directory
```shell=
$ mkdir images
```
But Git can not commit a empty folder, so you can add a ".keep" or ".gitkeep" file.
```shell=
$ touch images/.keep
```
### Ignore specific file
```shell=
$ touch .gitignore
```
Edit `.gitignore`. Type the filename which you want Git to ignore.  
It only can ignore the file add after the gitignore setting.  
> $ git rm --cached
>> ignore the file whiche has been tracked.
### Force to add file without gitignore
```shell=
$ git add -f filename
```