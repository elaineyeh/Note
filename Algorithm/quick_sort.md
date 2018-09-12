# Quick sort

快速排序

``` py
def quick_sort(lists):
    small = []
    big = []
    keys = []
    
    if len(lists) <= 1:
        return lists
    else:
        key = lists[0]

        for i in lists:
            if i > key:
                big.append(i)
            elif i < key:
                small.append(i)
            else:
                keys.append(i)

        small = quick_sort(small)    
        big = quick_sort(big)

        return small+keys+big    


lists = [6, 4, 3, 7, 5, 1, 2]
print(quick_sort(lists))
```