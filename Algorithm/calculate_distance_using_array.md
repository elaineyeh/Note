# Calculate distance using array

計算點與點之間的距離

``` py
import math

point = [
    [3, 3], [1, 5], [4, 6], [2, 8], [9, 9],
    [2, 1], [7, 2], [6, 5], [9, 4], [5, 9]
]
distance = []

for i in range(0, 10):
    for j in range(i+1, 10):
        dis_x = point[i][0]-point[j][0]
        dis_y = point[i][1]-point[j][1]
        dis = math.sqrt((dis_x*dis_x)+(dis_y*dis_y))
        distance.append(dis)

print(min(distance))
```