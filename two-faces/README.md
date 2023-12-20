# two-faces

Mở file modifyFunction.exe bằng die, đây là file PE32

![](https://github.com/noobmannn/kcscrecruitment2023/blob/b7a0d4997e9c2c878d729b3b764448e434956b5b/two-faces/Images/1.png)

Mở bằng IDA32, vào hàm ```_main``` -> ```_main_0```, về cơ bản đoạn đầu hàm yêu cầu chúng ta nhập flag, kiểm tra xem độ dài của flag có phải 22 không, sau đó loại bỏ đoạn ```KCSC{``` ở đầu và ```}``` ở cuối flag, sau đó tiếp tục kiểm tra độ dài String còn lại có phải bằng 16 không? Cuối cùng thì sắp xếp lần lượt 16 kí tự trên vào một mảng ```block``` hai chiều 4x4

![](https://github.com/noobmannn/kcscrecruitment2023/blob/b7a0d4997e9c2c878d729b3b764448e434956b5b/two-faces/Images/2.png)

Về đoạn tiếp theo, sau 1 thời gian đọc mã giả, debug và thử dựng lại hàm, mình nhận ra lần lượt vai trò của các hàm trên:

- ```sub_2212F3```: xoay trái các hàng của ```block``` theo thứ tự của hàng đó
- ```sub_221186```: xoay lên các cột của ```block``` theo thứ tự của cột đó
- ```sub_221276```: đổi chỗ 4 bit đầu và 4 bit cuối của mỗi phần tử trong ```block```, vd: 0x2f -> 0xf2
- ```sub_2211A4```: xor từng phần tử của block với 0x55 + vòng lặp hiện tại

Sau đó tiếp tục lặp đi lặp lại quá trình đó 100 lần.


![](https://github.com/noobmannn/kcscrecruitment2023/blob/b7a0d4997e9c2c878d729b3b764448e434956b5b/two-faces/Images/3.png)

Từ đó mình viết Script để lấy Flag:

```python
def unxor(block, key):
    for i in range(4):
        for j in range(4):
            block[i][j] ^= key

def unSwap4Bit(block):
    for i in range(4):
        for j in range(4):
            v4 = block[i][j]
            block[i][j] = 16 * (v4 & 0xF) + v4//16

def unShiftColumns(block):
    for i in range(4):
        tmp = [0 for _ in range(4)]
        for j in range(4):
            tmp[j] = block[j][i]
        for j in range(4):
            block[j][i] = tmp[(j-i)%4]


def unShiftRows(block):
    for i in range(4):
        tmp = [block[i][j] for j in range(4)]
        for j in range(4):
            block[i][j] = tmp[(j-i)%4]

dencFlag = 'FDA6FF91ADA0FDB7ABA9FB91EFAFFAA2'

block = [[dencFlag[i * 8 + j * 2: i * 8 + j * 2 + 2] for j in range(4)] for i in range(4)]
block = [[int(block[i][j], 16) for j in range(4)] for i in range(4)]
for i in range(100):
    unxor(block, 0x55 + 99 - i)
    unSwap4Bit(block)
    unShiftColumns(block)
    unShiftRows(block)
print('KCSC{' + ''.join([''.join([chr(block[i][j]) for j in range(4)]) for i in range(4)]) + '}')
```

Thu được kết quả là ```KCSC{3a5y_ch41leng3_!}```, thử String khi đang debug thì kết quả là in ra chuỗi ```Good. Nice work```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/b7a0d4997e9c2c878d729b3b764448e434956b5b/two-faces/Images/4.png)

Tuy nhiên khi thử kết quả trên khi không Debug, kết quả lại không đúng :((((

![](https://github.com/noobmannn/kcscrecruitment2023/blob/b7a0d4997e9c2c878d729b3b764448e434956b5b/two-faces/Images/5.png)

Sau một hồi Debug thử lại, mình nhận ra có hàm ```TlsCallback_0_0```, chạy trước cả hàm ```_main_0```, ở đây có gọi đến một hàm Antidebug ```IsDebuggerPresent```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/b7a0d4997e9c2c878d729b3b764448e434956b5b/two-faces/Images/6.png)

Đọc qua mã giả của hàm, mình nhận ra rằng program đang tạo ra một đoạn mã ASM nhỏ: ```0x68``` là opcode của lênh ```push```, ```0xC3``` là opcode của lênh ```ret``` còn ```sub_ED133E``` có vẻ là hàm check Flag thật. Sau đó hàm sử dụng lệnh ```VirtualProtect``` để cấp quyền cho vùng nhớ được trỏ tới bởi hàm ```j_strcmp```. Tiếp theo ghi đoạn mã ASM mà ta vừa phân tích ở trên vào vùng nhớ đó:

```Assembly
push sub_ED133E
ret
```

Kết luận: hàm này sửa lại hàm ```j_strcmp``` để nó có thể gọi đến hàm ```sub_ED133E```, đây là hàm checkFlag thật. Vì có gọi đến một hàm Antidebug ```IsDebuggerPresent`` nên khi debug chúng ta sẽ không thấy chương trình chạy vào được nó

Vậy để Debug bình thường, mình đặt Breakpoint tại ```0xED1860```, sau đó sửa lại giá trị của cờ ZF từ 0 thành 1 để chương trình rẽ vào nhánh đúng

![](https://github.com/noobmannn/kcscrecruitment2023/blob/b7a0d4997e9c2c878d729b3b764448e434956b5b/two-faces/Images/7.png)

Sau đó đi thẳng tới hàm ```j_strcmp``` và tiếp tục Trace, chúng ta đá vào được hàm kiểm tra Flag thật, sau khi Debug qua hàm này thì hàm này sẽ tính chuỗi đúng để so sánh với Flag bằng cách cho các giá trị được khai báo ở trên xor với chuỗi ```FDA6FF91ADA0FDB7ABA9FB91EFAFFAA2```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/b7a0d4997e9c2c878d729b3b764448e434956b5b/two-faces/Images/8.png)

Từ đây mình chỉnh sửa lại Script để in ra đúng Flag của bài:

```python

def unxor(block, key):
    for i in range(4):
        for j in range(4):
            block[i][j] ^= key

def unSwap4Bit(block):
    for i in range(4):
        for j in range(4):
            v4 = block[i][j]
            block[i][j] = 16 * (v4 & 0xF) + v4//16

def unShiftColumns(block):
    for i in range(4):
        tmp = [0 for _ in range(4)]
        for j in range(4):
            tmp[j] = block[j][i]
        for j in range(4):
            block[j][i] = tmp[(j-i)%4]


def unShiftRows(block):
    for i in range(4):
        tmp = [block[i][j] for j in range(4)]
        for j in range(4):
            block[i][j] = tmp[(j-i)%4]

# Real Check Flag
v7 = [0x07, 0x7C, 0x00, 0x07, 0x7F, 0x77, 0x78, 0x01, 0x00, 0x73, 
  0x07, 0x75, 0x00, 0x02, 0x03, 0x73, 0x07, 0x07, 0x00, 0x0C, 
  0x07, 0x72, 0x7B, 0x70, 0x04, 0x7F, 0x03, 0x04, 0x07, 0x71, 
  0x00, 0x04]
a2 = 'FDA6FF91ADA0FDB7ABA9FB91EFAFFAA2'
dencFlag = ''
for i in range(0x20):
    dencFlag += chr(ord(a2[i]) ^ v7[i])

block = [[dencFlag[i * 8 + j * 2: i * 8 + j * 2 + 2] for j in range(4)] for i in range(4)]
block = [[int(block[i][j], 16) for j in range(4)] for i in range(4)]
for i in range(100):
    unxor(block, 0x55 + 99 - i)
    unSwap4Bit(block)
    unShiftColumns(block)
    unShiftRows(block)
print('KCSC{' + ''.join([''.join([chr(block[i][j]) for j in range(4)]) for i in range(4)]) + '}')

```

# Flag

```KCSC{function_h00k1ng}```
