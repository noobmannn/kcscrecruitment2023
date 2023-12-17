# Awg Mah Back

Bài cho một file python có nội dung như dưới đây, về cơ bản program sẽ đọc flag từ file ```flag.txt``` sau đó thực hiện các bước mã hoá phía dưới và ghi kết quả vào ```output.txt```

```python
from pwn import *

with open('flag.txt', 'rb') as (f):
    flag = f.read()
a = flag[0:len(flag) // 3]
b = flag[len(flag) // 3:2 * len(flag) // 3]
c = flag[2 * len(flag) // 3:]
a = xor(a, int(str(len(flag))[0]) + int(str(len(flag))[1]))
b = xor(a, b)
c = xor(b, c)
a = xor(c, a)
b = xor(a, b)
c = xor(b, c)
c = xor(c, int(str(len(flag))[0]) * int(str(len(flag))[1]))
enc = a + b + c
with open('output.txt', 'wb') as (f):
    f.write(enc)
```

Vậy thì mình chỉ cần đọc file ```output.txt``` mà đề cung cấp rồi reverse lại quá trình trên là được

```python
from pwn import *

with open('output.txt', 'rb') as (f):
    flag = f.read()
a = flag[0:len(flag) // 3]
b = flag[len(flag) // 3:2 * len(flag) // 3]
c = flag[2 * len(flag) // 3:]
c = xor(c, 2)
c = xor(b, c)
b = xor(a, b)
a = xor(c, a)
c = xor(b, c)
b = xor(a, b)
a = xor(a, 3)
denc = a + b + c
print(denc)
```


