# String matching

比對短字串是否出現在長字串中

``` py
text = list(input("Input the sentence:"))
pattern = list(input("Input the patter:"))
flag = True

for i in range(0, len(text)-len(pattern)+1):
    for j in range(0, len(pattern)):
        if text[i+j] != pattern[j]:
            flag = False

    if flag:
        print("短字串出現在第", i+j, "個字元")

if flag == False:
    print("Not found")
```