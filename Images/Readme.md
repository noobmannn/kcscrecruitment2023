# Images

Để cho 12 ảnh được chụp từ 1 program, và nó đang được hiển thị dưới dạng code Assembly trên IDA

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/Images/Image/1.jpg)

Ảnh 1 và ảnh 2 về cơ bản chỉ là khởi tạo các biến cần thiết và yêu cầu ta nhập Flag

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/Images/Image/2.jpg)

Từ ảnh 3 trở về sau hầu hết các đoạn mã asm đều khá giống nhau

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/Images/Image/3.jpg)

```asm
mov      eax, 1
imul     rax, 1Ah
movsx    eax, [rbp+rax+230h+buffer]
cmp      eax, 75
```

Đọc qua đoạn code trên mình nhận ra program yêu cầu nhập vào flag và sau đó so sánh từng kí tự của flag với các giá trị được cung cấp như trên, từ đó mình đọc hết toàn bộ ảnh và viết script để giải bài

```python
flag = [0 for _ in range(54)]

flag[0] = 75
flag[0x1a] = 110
flag[14] = 101
flag[18] = 104
flag[34] = 101
flag[23] = 110
flag[18] = 107
flag[21] = 110
flag[22] = 95
flag[9] = 111
flag[5] = 67
flag[8] = 95
flag[10] = 110
flag[14] = 95
flag[12] = 118
flag[16] = 97
flag[3] = 67
flag[30] = 105
flag[48] = 121
flag[41] = 95
flag[44] = 104
flag[39] = 110
flag[7] = 109
flag[28] = 110
flag[29] = 104
flag[15] = 100
flag[38] = 111
flag[42] = 97
flag[46] = 110
flag[37] = 100
flag[33] = 104
flag[32] = 95
flag[36] = 95
flag[31] = 110
flag[25] = 97
flag[27] = 95
flag[35] = 116
flag[45] = 95
flag[19] = 105
flag[40] = 103
flag[43] = 110
flag[47] = 97
flag[17] = 95
flag[49] = 96
flag[50] = 125
flag[4] = 123
flag[13] = 105
flag[11] = 95
flag[6] = 97
flag[2] = 83
flag[1] = 67
flag[20] = 101
flag[24] = 104

for c in flag:
    print(chr(c), end='')
print()
```

# Flag

```KCSC{Cam_on_vi_da_kien_nhan_nhin_het_dong_anh_nay`}```

