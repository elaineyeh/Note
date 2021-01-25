# How to merge the commit
當你有幾筆記錄類似的時候可以合併在一起
```
commit A (HEAD)
    refactor: add whitespace

commit B
    refactor: add whitespace

commit C
    refactor: add whitespace

commit D
    feat: docker

commit E
    refactor: feat: socket
```
## Step 1.
先看一下紀錄有哪幾筆要合併
```shell=
git log
```
我們合併 A, B, C

## Step 2.
選好後取要合併的前一筆記錄
```shell=
git rebase -i D
```

## Step 3.
會出現 vim 畫面，修改一下要合併的紀錄並儲存
```
pick A refactor: add whitespace
squash B refactor: add whitespace
squash C refactor: add whitespace
```
這樣 B, C 就會跟 A 合併

## Step 4.
接下來會再出現 vim 畫面，修改 commit 訊息，修改完一樣儲存

## Step 5.
最後推上線即可
**因為我們把前幾次紀錄合併，他會去找我們相同的根，我們可以有兩種做法，一種是開一個分支合併進去，一種是強制蓋過去**
**這邊使用強制蓋過去的方式，所以記得加上 `-f`**

```
git push -f
```
