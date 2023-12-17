# Pokemon Hof Panel Level 1

Đầu tiên mình sẽ nhập bừa một cái tên và chọn bừa một con pokemon rồi submit. Sau đó Inspect để xem Cookie Value

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cc325bd4c8a7e23aa2a6ad1ec5127627c6372334/Pokemon%20Hof%20Panel%20Level%201/Image/1.png)

Decrypt base64 giá trị cookie

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cc325bd4c8a7e23aa2a6ad1ec5127627c6372334/Pokemon%20Hof%20Panel%20Level%201/Image/2.png)

Vào file ```champ.php``` trong folder mà bài cung cấp, mình thấy để in ra flag thì giá trị ```isChampion($user)``` phải bằng 1

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cc325bd4c8a7e23aa2a6ad1ec5127627c6372334/Pokemon%20Hof%20Panel%20Level%201/Image/3.png)

Mình sẽ sửa lại kết quả decrypt khi nãy ở đoạn ```b:0``` thay bằng ```b:1```, sau đó encrypt đoạn đó thành base64

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cc325bd4c8a7e23aa2a6ad1ec5127627c6372334/Pokemon%20Hof%20Panel%20Level%201/Image/4.png)

Ném đoạn base64 vừa encode được vào cookie value, sau đó reload page, mình thu được flag

![](https://github.com/noobmannn/kcscrecruitment2023/blob/cc325bd4c8a7e23aa2a6ad1ec5127627c6372334/Pokemon%20Hof%20Panel%20Level%201/Image/5.png)

# Flag

```KCSC{n0w_y0u_kn0w_s3r1al1z3_f0m4rt}```
