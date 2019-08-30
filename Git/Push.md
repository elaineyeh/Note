# How to push to Github
## Step 1. 
在要進行版本控制的資料夾下
```
git init
```
## Step 2. 
`firename` 為有更動、要做紀錄的檔案名稱
```
git add firename
```
>**若是要整個專案都更新則可以將 `firename` 的位置改為 `.`**
>>```
>>git add .
>>```

#### 若是一直 add 沒有 commit，當 commit 時只會紀錄最後一次的 add

## Step 3.
為此次記錄做註解
```
git commit -m "comment"
```

## Step 4.
推上 Github，存上遠端
```
git push
```

---
# The basic instructions of Git
### 比較檔案版本前後差異  
```
git diff  
```
### 看目前 commit 過的版本  
```
git log  
```
### 看目前有沒有檔案更動  
```
git status  
```
### 查看之前的版本
```
git checkout
```