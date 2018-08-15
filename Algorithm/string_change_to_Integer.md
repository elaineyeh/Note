# String change to Integer

將字串數字轉換成整數數字

``` py
string = input("Input the number:")
num = 0

for i in range(0, len(string)):
    num += int(string[i])*pow(10, len(string)-i-1)

print(num)
```

<br>

``` py
string = input("Input the number:")
num = 0

for i in range(0, len(string)):
    num *= 10 
    num += int(string[i])

print(num)
```