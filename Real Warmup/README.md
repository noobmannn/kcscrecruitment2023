# Real warmup

Mở file đề cho bằng die, ta có thể thấy đây là một file PE64

![](https://github.com/noobmannn/kcscrecruitment2023/blob/addc32300064baa24627f9a10d45cdab01c7beff/Real%20Warmup/Image/1.png)

Mở file bằng IDA64 và xem qua PseudoCode của nó, ta có thể thấy program yêu cầu ta nhập input và kiểm tra nếu input bằng với chuỗi ```S0NTQ3tDaDQwYF9NfF98bjlgX0QzTidfVjAxJ183N1wvX0tDU0N9``` thì in ra ```Great!!!```, còn nếu sai thì in ra ```Sai roi. Hay thu lai```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/addc32300064baa24627f9a10d45cdab01c7beff/Real%20Warmup/Image/2.png)

Mình đoán chuỗi ```S0NTQ3tDaDQwYF9NfF98bjlgX0QzTidfVjAxJ183N1wvX0tDU0N9``` là một chuỗi base64. Mình mang lên CyberChef để decode nó và đây là kết quả :)))

![](https://github.com/noobmannn/kcscrecruitment2023/blob/addc32300064baa24627f9a10d45cdab01c7beff/Real%20Warmup/Image/3.png)

# Flag

```KCSC{Ch40`_M|_|n9`_D3N'_V01'_77\/_KCSC}```
