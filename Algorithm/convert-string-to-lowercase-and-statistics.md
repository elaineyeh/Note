# 將英文字母由大寫轉為小寫並統計字母出現次數
```*py
sentence = input("Input:")
asc = list(sentence)
character = {}

for i in range(0,26):
  num = 97+i
  character[chr(num)] = 0

for i in range(0,len(asc)):
  if ord(asc[i]) >= 65 and ord(asc[i]) <= 90 or ord(asc[i]) >= 97 and ord(asc[i]) <= 122:
    if ord(asc[i]) >= 65 and ord(asc[i]) <= 90:
        asc[i] = chr(ord(asc[i])-ord("A")+ord("a"))

    character[asc[i]] += 1

for i in range(0, 26):
  if character[chr(i+97)] > 0:
    print(chr(i+97), ":", character[chr(i+97)])
```