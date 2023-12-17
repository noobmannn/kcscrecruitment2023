# Ez_Ceasar

Đề cho một file python với nội dung như dưới

```python
import string
import random

alphabet = string.ascii_letters + string.digits + "!{_}?"

flag = 'KCSC{s0m3_r3ad4ble_5tr1ng_like_7his}'
assert all(i in alphabet for i in flag)

key = random.randint(0, 2**256)

ct = ""
for i in flag:
    ct += (alphabet[(alphabet.index(i) + key) % len(alphabet)])

print(f"{ct=}")

# ct='ldtdMdEQ8F7NC8Nd1F88CSF1NF3TNdBB1O'
```

Hàm trên sử dụng phương pháp mã hoá Ceasar, về cơ bản hàm sẽ dịch chuyển bảng chữ cái dựa trên Key và từ bảng chữ cái mới tạo ra Bản Mã

Với bài này, vì kí tự đầu tiên của flag là 'K' còn kí tự đầu của cyphertext là 't', nên mình cần dựa vào bảng chữ cái để tìm Key trước

Bảng chữ cái dựa theo đề bài là ```abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!{_}?```, từ đó suy ra Key mã hoá là 25

Từ đây mình suy ra script giải bài:

```python
import string

alphabet = string.ascii_letters + string.digits + "!{_}?"

flag = 'KCSC{s0m3_r3ad4ble_5tr1ng_like_7his}'
assert all(i in alphabet for i in flag)

key = 25

flag = ''
ct = "ldtdMdEQ8F7NC8Nd1F88CSF1NF3TNdBB1O"
for i in ct:
    flag += (alphabet[(alphabet.index(i) + key) % len(alphabet)])

print(f"{flag=}")
```

# Flag

```KCSC{C3as4r_1s_Cl4ss1c4l_4nd_C00l}```
