# Stairs climbing

計算第一階到第五階的爬法種類數目
#### 遞歸法
```py
table = list() #儲存全部問題的答案
solve = list() #紀錄問題是否計算完畢

for i in range(0,6):
    table.append(0)
    solve.append("False")

def times(num):
    if solve[num] == True:
        return table[num]
    elif num <= 1:
        table[num] = 1 #可寫可不寫
        return 1

    table[num] = times(num-2) + times(num-1)
    solve[num] = True
    return table[num]

print("爬完五階的做法:有", times(5), "種")
print(table)
```

```py
table = list() #將 table 跟 solve 結合，節省程式碼

for i in range(0, 6):
    table.append(0)

def times(num):
    if num <= 1:
        table[num] = 1;
        return table[num]
    elif table[num]:
        return table[num]

    table[num] = times(num-2) + times(num-1)
    return table[num]

print("爬完五階的做法:有", times(5), "種")
print(table)
```

#### 遞推法
```py
table = list()
for i in range(0, 6):
    table.append(0)

def dynamic_programming():
    table[0], table[1] = 1, 1

    for i in range(2, 6):
        table[i] = table[i-1] + table[i-2]

dynamic_programming()
print("爬完五階的做法有：", table[5], "種")
print(table)
```

```py
table = list()
for i in range(0, 6):
    table.append(0)

def dynamic_programming():
    table[0] = 1

    for i in range(0, 6):
        if (i+1) < 6 :
            table[i+1] += table[i]
        
        if (i+2) < 6:
            table[i+2] += table[i]

dynamic_programming()
print("爬完五階的做法有：", table[5], "種")
print(table)
```