# mission: impossible

Đề cho mình một file GameStart.exe, một file resources và một file Gameplay

Mở GameStart.exe bằng die, đây là file một file PE64

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/1.png)

Mở bằng IDA64, vì file chứa quá nhiều hàm lẫn lộn nhau, nên mình sẽ mở View -> Open Subview -> Strings để xem có những String nào có thể khai thác được không. Và mình gặp được chuỗi ```Start game failed!```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/2.png)

Lần theo chuỗi trên ta đến được hàm ```sub_14002E620```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/3.png)

Vào hàm ```sub_140012A96``` -> ```sub_1400234D0```, đọc qua mã giả của nó mình nhận ra hàm này mở file ```resources```, kiểm tra Signature File (4 byte đầu tiên) có phải là ```sonx``` hay không. Nếu đúng thì đổi Signature File thành ```Rar!```, còn sai thì in ra ```Unknown resource signature!```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/4.png)

Vào hàm ```sub_140012933``` -> ```sub_140023070```, đọc qua mã giả mình nhận ra hàm này sẽ gọi lệnh ```x -ierr ./resources``` để giải nén một thứ gì đó ra từ file ```resources```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/5.png)

Khi mình chạy thử File ```GameStart.exe``` và để File Explorer của mình ở chế độ hiển thị cả các file bị ẩn, mình đã thấy được ba Folder như hình dưới đây.

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/6.png)

Từ đây tổng hợp lại mình hiểu được cách hoạt động của chương trình: Đầu tiên kiểm tra Signature File của ```resources``` có hợp lệ không? Nếu đúng thì đổi sang ```Rar!``` và tiến hành giải nén bằng WinRAR. Sau đó chương trình gọi ```Gameplay``` và sử dụng ba Folder được giải nén từ ```resources``` để cấu hình game. Sau khi chơi game xong, nén lại chúng vào ```resources```, trả Signature File về như cũ và kết thúc chương trình.

Vậy để "Hack" được Game, mình cần phải sửa lại cấu hình trong ```resources```. Đầu tiên, mình sẽ mở file bằng HxD và chỉnh lại Signature File về ```Rar!```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/7.png)

Sau khi chỉnh xong thì giải nén file, đây chính là 3 Folder mà mình đã thấy khi chạy Game

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/8.png)

Vào config -> config.json, ta thấy data thiết lập cho trò chơi trông như thế này. Vậy mình chỉ cần chỉnh sao cho Player có dam siêu to máu siêu nhiều, còn Enemy có dam siêu thấp máu siêu ít là được :)))))

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/9.png)

Sau khi chỉnh xong, nén 3 Folder lại thành 1 file ```resources``` mới, chỉnh Signature file về lại thành ```sonx```, sau đó chạy ```GameStart.exe``` và đấm hết mấy thằng Enemy để lấy Flag nữa là xong :))))

![](https://github.com/noobmannn/kcscrecruitment2023/blob/main/mission%3A%20impossible/Images/10.png)

# Flag

```KCSC{y0u_w0n_4nd_g3t_th3_fl4g}```
