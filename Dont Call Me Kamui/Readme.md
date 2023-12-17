# Dont Call Me Kamui

Mở file bằng die

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cf501ffeb26182d386b810bcdd07e996a26f7eb2/Dont%20Call%20Me%20Kamui/Image/1.png)

Bài cho chúng ta một file PE32, khi chạy thử chương trình, nó yêu cầu mình nhập Key, sai thì trả về ```Try again!```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cf501ffeb26182d386b810bcdd07e996a26f7eb2/Dont%20Call%20Me%20Kamui/Image/2.png)

Tiến hành load file vào IDA32 và xem hàm ```main``` của nó

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cf501ffeb26182d386b810bcdd07e996a26f7eb2/Dont%20Call%20Me%20Kamui/Image/3.png)

Mình sẽ xem thử bên trong hàm ```sub_4011E0```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cf501ffeb26182d386b810bcdd07e996a26f7eb2/Dont%20Call%20Me%20Kamui/Image/4.png)

Dựa vào các giá trị ```0x6A09E667```, ```0xBB67AE85```, ```0x3C6EF372```, ```0xA54FF53A```, ```0x510E527F```, ```0x9B05688C```, ```0x1F83D9AB```, ```0x5BE0CD19```, mình nhận ra rằng hàm này sẽ biến đổi Key đầu vào mình nhập thành một đoạn mã SHA-256 (có thể xem cụ thể cách nó hoạt động ở đây: https://viblo.asia/p/cach-hoat-dong-cua-sha-256-1VgZvJPmZAw)

Bởi việc viết Script để Decrypt một đoạn mã SHA-256 là một việc cực kì khó khăn, nên mình sẽ giải bài này theo một cách khác.

Đặt breakpoint tại ```0x00401B99``` sau đó tiến hành Debug 

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cf501ffeb26182d386b810bcdd07e996a26f7eb2/Dont%20Call%20Me%20Kamui/Image/5.png)

Theo dõi sự thay đổi giá trị của thanh ghi ```esi```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cf501ffeb26182d386b810bcdd07e996a26f7eb2/Dont%20Call%20Me%20Kamui/Image/6.png)

Lúc này giá trị của thanh ghi ```esi``` là ```1cf18a243c25a56a993c8207d1161a9c2de5f34b952d382704b94dc5e888b108```, đây chính là giá trị được đem ra so sánh với mã SHA-256 được biến đổi từ Key nhập vào

Mình thử ném đoạn mã trên vào trang web https://md5hashing.net/hash/sha256 và thử Decrypt xem được không, và kết quả trả về chuỗi gốc ban đầu là ```goldfish```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cf501ffeb26182d386b810bcdd07e996a26f7eb2/Dont%20Call%20Me%20Kamui/Image/7.png)

Chạy lại chương trình với Input là đoạn Key mình vừa tìm được, và ta có được Flag :)))))

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cf501ffeb26182d386b810bcdd07e996a26f7eb2/Dont%20Call%20Me%20Kamui/Image/8.png)

# Flag

```KCSC{het_y_tuong_roi_nen_di_an_trom_idea_KMACTF_hjhj}```
