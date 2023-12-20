# The Ultimate Xor

Đề cho chúng ta một file text gồm gần 10000 dòng code Assembly cực kì dài, loằng ngoằng và khó đọc

Nhưng thực ra nếu nhìn kĩ, chúng ta có thể thấy khoảng tầm 10 đến 15 dòng code lặp lại liên tục cùng một cấu trúc như hình dưới này

![](https://github.com/noobmannn/kcscrecruitment2023/blob/e533f2e7debb71de248f562807012527154edf5f/The%20Ultimate%20Xor/Image/1.png)

Đọc qua đoạn code trên, về cơ bản nó có vai trò là Xor hai giá trị được mình khoanh tròn đỏ trên hình. Dựa vào đó mình viết thử một Script để chạy Xor toàn bộ File và in ra kết quả

```python
keyMov = []
keyXor = []  

with open('asm.txt', 'r') as f:
    for line in f.readlines():
        if (line.find('mov	dword ptr [rbp - 0x10]') != -1):
            pos = line.find(',')
            item = line[pos+2:-1]
            keyMov.append(int(item,16))
        elif (line.find('xor') != -1):
            pos = line.find(',')
            item = line[pos+2:-1]
            keyXor.append(int(item,16))

flag = ''

for i in range(len(keyXor)):
    flag += chr(keyMov[i] ^ keyXor[i])

print(flag)
```

Nhưng kết quả đoạn Script trên thu về dường như không thành công lắm :(((

![](https://github.com/noobmannn/kcscrecruitment2023/blob/e533f2e7debb71de248f562807012527154edf5f/The%20Ultimate%20Xor/Image/2.png)

Để ý kĩ thì ta sẽ thấy có khoảng 4 đến năm đoạn code không có chứa lệnh Xor như hình dưới đây

![](https://github.com/noobmannn/kcscrecruitment2023/blob/e533f2e7debb71de248f562807012527154edf5f/The%20Ultimate%20Xor/Image/3.png)

Vì thế mình sẽ sửa lại đoạn Script trên một chút và xem kết quả cuối cùng sẽ chạy ra như thế nào 

```python
keyMov = []
keyXor = []  

checkXor = False

with open('asm.txt', 'r') as f:
    for line in f.readlines():
        if (line.find('mov	dword ptr [rbp - 0x10]') != -1):
            if (checkXor):
                keyXor.append(0x0)
            pos = line.find(',')
            item = line[pos+2:-1]
            keyMov.append(int(item,16))
            checkXor = True
        elif (line.find('xor') != -1):
            pos = line.find(',')
            item = line[pos+2:-1]
            keyXor.append(int(item,16))
            checkXor = False

flag = ''

for i in range(len(keyMov)):
    flag += chr(keyMov[i] ^ keyXor[i])

print(flag)
```

Và dưới đây là kết quả mình thu được

```
Hey this is the final steps.
Go further, don't give up!

Heishiro Mitsurugi is one of the most recognizable characters in the Soul series of fighting games. Mitsurugi made his first appearance in Soul Edge and has returned for all six sequels: Soulcalibur, Soulcalibur II, Soulcalibur III, Soulcalibur IV, Soulcalibur: Broken Destiny and Soulcalibur V. He also appears as a playable character in Soulcalibur Legends and Soulcalibur: Lost Swords, as He Who Lives for Battle.

All I need here is a long text, just because I want you to be able to reverse it. I hope you'll learn some good things. Automatizing things can be really good.

The flag for this challenge is "I_reverse_all_this_and_all_I_got_is_this_flag"
```

# Flag

```KCSC{I_reverse_all_this_and_all_I_got_is_this_flag}```
