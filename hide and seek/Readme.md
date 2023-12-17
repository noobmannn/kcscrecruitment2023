# hide and seek

Mở file đề cho bằng die, ta có thể thấy đây là một file PE32

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a40a006b59f43dfba86ab33d1a7a2f7a33b04949/hide%20and%20seek/Image/1.png)

Mở file bằng IDA32 và xem Pseudocode của hàm Main

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a40a006b59f43dfba86ab33d1a7a2f7a33b04949/hide%20and%20seek/Image/2.png)

Vào hàm ```sub_3718A7``` ==> ```sub_377A70```, có thể thấy hàm này sẽ tạo một file gì đó và lưu nó vào máy tính của mình.

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a40a006b59f43dfba86ab33d1a7a2f7a33b04949/hide%20and%20seek/Image/3.png)

Tiếp tục vào hàm ```sub_37148D``` ==> ```sub_37FCD0```, đây là hàm tạo console và in ra những gì chúng ta thấy

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a40a006b59f43dfba86ab33d1a7a2f7a33b04949/hide%20and%20seek/Image/4.png)

Mình sẽ thử đặt breakpoint ở ```0x00380088```, debug và trace tới ```0x0038009D```, lúc này giá trị của ```offset word_3873E0``` có vẻ giống với một loại đường dẫn nào đó

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a40a006b59f43dfba86ab33d1a7a2f7a33b04949/hide%20and%20seek/Image/5.png)

Thử truy cập đường dẫn thì nó dẫn ta tới file này

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a40a006b59f43dfba86ab33d1a7a2f7a33b04949/hide%20and%20seek/Image/6.png)

Mở file lên và ta thu được flag của bài :)))

![](https://github.com/noobmannn/kcscrecruitment2023/blob/a40a006b59f43dfba86ab33d1a7a2f7a33b04949/hide%20and%20seek/Image/7.png)

# Flag 

```KCSC{~(^._.)=^._.^=(._.^)~}```

