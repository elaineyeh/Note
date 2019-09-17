# How to push to Github
## Step 1. 
在要進行版本控制的資料夾下
```=shell
git init
```
## Step 2. 
`firename` 為有更動、要做紀錄的檔案名稱
```=shell
git add firename
```
>**若是要整個專案都更新則可以將 `firename` 的位置改為 `.`**
>>```=shell
>>git add .
>>```

#### 若是一直 add 沒有 commit，當 commit 時只會紀錄最後一次的 add

## Step 3.
為此次記錄做註解
```=shell
git commit -m "comment"
```

## Step 4.
推上 Github，存上遠端
```=shell
git push
```

---
# The basic instructions of Git
### 比較檔案版本前後差異  
在未 git add 之前跟上一個版本的比較
```=shell
git diff  
```
### 看目前 commit 過的版本  
```=shell
git log  
```
### 看目前有沒有檔案更動  
```=shell
git status  
```
### 跳回之前的版本
```=shell
git checkout
```