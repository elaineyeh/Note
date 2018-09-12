# Merge sort

合併排序

``` py
def merge_sort(lists):
    size = len(lists)
    if size > 2:
        sort = []

        half = int(size/2)
        left = merge_sort(lists[:half])
        right = merge_sort(lists[half:])
        
        for i in range(size):
            if len(left) <= 0 or (len(right) > 0 and left[0] > right[0]):
                sort.append(right.pop(0))
            else:
                sort.append(left.pop(0))
                
        return sort
    elif size > 1 and lists[0] > lists[1]:
        return [lists[1],lists[0]]
    else:
        return lists

lists = [6, 4, 3, 7, 5, 1, 2]
print (merge_sort(lists))
```