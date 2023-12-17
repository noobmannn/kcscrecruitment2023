# dynamic function

Mở file đề cho bằng die, ta thấy đây là một file PE32

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a446f3abeab7512a4fc2f97151671d15b3642e3d/dynamic%20function/Image/1.png)

Mở file bằng PE32. đọc qua Pseudocode của hàm main, về cơ bản đoạn đầu hàm yêu cầu chúng ta nhập flag, kiểm tra xem độ dài của flag có phải 30 không? Hàm ```sub_4113ED``` để loại bỏ đoạn ```KCSC{``` ở đầu và ```}``` ở cuối flag, sau đó tiếp tục kiểm tra độ dài String còn lại có phải bằng 24 không?

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a446f3abeab7512a4fc2f97151671d15b3642e3d/dynamic%20function/Image/2.png)

Ở phần tiếp theo, chương trình cấp phát một vùng nhớ ảo cho biến ```lpAddress```, sau đó xor từng byte với ```0x41```. Lúc này ```lpAddress``` đã trở thành 1 hàm và sau đó chương trình gọi đến hàm này. Gọi xong chương trình giải phóng vùng nhớ vừa được cấp phát cho hàm.

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a446f3abeab7512a4fc2f97151671d15b3642e3d/dynamic%20function/Image/3.png)

Đặt breakpoint tại ```0x003E58BD```, sau đó tiến hành debug và Trace vào hàm ```lpAddress```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a446f3abeab7512a4fc2f97151671d15b3642e3d/dynamic%20function/Image/4.png)

Nhấp chuột phải chọn Create Function, sau đó chuyển sang chế độ Pseudocode. Lúc này ta thấy code đã dễ nhìn hơn rất nhiều

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a446f3abeab7512a4fc2f97151671d15b3642e3d/dynamic%20function/Image/5.png)

Từ đây có thể thấy: chương trình sẽ Encrypt flag thông qua hàm ```lpAddress```, sau đó check từng kí tự của kết quả với ```byte_3E7CF4```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a446f3abeab7512a4fc2f97151671d15b3642e3d/dynamic%20function/Image/6.png)

Dựa vào đó mình sẽ viết script để giải bài này

```python
from z3 import *

enc = [0x44, 0x93, 0x51, 0x42, 0x24, 0x45, 0x2E, 0x9B, 0x01, 0x99,
                0x7F, 0x05, 0x4D, 0x47, 0x25, 0x43, 0xA2, 0xE2, 0x3E, 0xAA,
                0x85, 0x99, 0x18, 0x7E]

v5 = 24
str_chars = [BitVec(f'str_{i}', 8) for i in range(v5)]
v4 = 'reversing_is_pretty_cool'
v4_chars = [ord(c) for c in v4]
i = 0
res = []
solver = Solver()
str = ''

while i < v5:
    tmp = 16 * (str_chars[i] % 16) + UDiv(str_chars[i], 16)
    res.append(tmp ^ v4_chars[i])
    solver.add(res[i] == enc[i])
    i = i + 1

if solver.check() == sat:
    model = solver.model()
    solution = [model[str_chars[i]].as_long() for i in range(v5)]
    str += ''.join([chr(c) for c in solution])

print('KCSC{' + str + '}')
```

# Flag

```KCSC{correct_flag!submit_now!}```
