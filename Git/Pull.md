# Git Pull
---
## 已經 commit
### 當從 Github 上拉專案到本地端，本地端跟遠端沒有衝突
```shell
$ git pull
```
**git pull = git fetch + git merge**

master-remote => git fetch 下來，檢查有沒有衝突。
根是一樣的，並且沒有衝突，git merge


```
   *  
   |c    --->   *
   |b           |b
   |a           |a
   *            *
remote        local
```

```
  *
  |c     
  |b              
  |a              
  *               
local       
```

### 當從 Github 上拉專案到本地端，本地端跟遠端有衝突
master-remote => git fetch 下來，檢查有沒有衝突。
根是不一樣的，有衝突，無法 git pull
所以要手動 merge

#### git fetch -> git merge -> edit -> git merge --continue -> git push

```

     *
     *
     |c *
     |b |d
     |a/a
     *
remote local
```
```

    | \
    |  *
    |  *
    *  |c
    |d |b
    |a/a
    *
local remote
```
---
## 還未 commit
先將本機端更動的檔案放到暫存，將遠端拉下來後再合起來
#### git stash -> git fetch -> git stash pop -> git merge
