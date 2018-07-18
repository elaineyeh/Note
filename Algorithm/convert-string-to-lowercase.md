# 將英文字母由大寫轉為小寫
```*py
sentence = input("Input:")
asc = list(sentence)

for i in range(0,len(asc)):
    if ord(asc[i]) > 65 and ord(asc[i]) < 90:
        asc[i] = chr(ord(asc[i])-ord("A")+ord("a"))

asc_str = ''.join(map(str,asc))
print(asc_str)
```

>Reference url
>>https://stackoverflow.com/questions/180606/how-do-i-convert-a-list-of-ascii-values-to-a-string-in-python
