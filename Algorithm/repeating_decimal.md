# Repeating decimal

找出循環節數

``` py
appear = {}  # 建立查詢表，記錄餘數是否已經出現
num = 0  # 遞推次數
mod = 1  # 餘數
quotient = 0  # 商數

# 1/53，所以餘數只可能是 0 ~ 52
for i in range(0, 54):  # 建立 53 格查詢表，每一格對應一個餘數
    appear[i] = 0

while appear[mod] == 0:
    appear[mod] = num+1
    num += 1
    
    mod *= 10
    quotient = mod/53
    mod = mod%53

print(appear)
print("循環節長度是", num-appear[mod]-1)
```