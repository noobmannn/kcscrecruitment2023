# password checker

Đề cho 3 file ```passwordChecker```, ```hash.so``` và ```wordlist.txt```

Đầu tiên mình mở file ```passwordChecker``` trên die, đây là một file ELF64

![](https://github.com/noobmannn/kcscrecruitment2023/blob/c61cd64fd259bbd9c850feda7eb855fc251cdfbc/password%20checker/Image/1.png)

Mở file ```hash.so``` trên IDA và đọc thông tin, đây là file Shared object (.so). Về cơ bản file này là 1 file thường được dùng để chứa các hàm và tài nguyên mà các ứng dụng khác có thể sử dụng được, vai trò giống với file .dll. Chỉ khác ở chỗ file này thường hoạt động trên linux, còn .dll thường hoạt động trên Windows

![](https://github.com/noobmannn/kcscrecruitment2023/blob/c61cd64fd259bbd9c850feda7eb855fc251cdfbc/password%20checker/Image/2.png)

Mở file ```passwordChecker``` và đọc qua Pseudocode của hàm ```main```, đầu tiên là hàm ```numberOfAttemptsRemaining``` để kiểm tra số lần được thực thi của chương trình, sau đó đến chương trình yêu cầu nhập input. Input sau đó được chạy qua hàm động v18

![](https://github.com/noobmannn/kcscrecruitment2023/blob/c61cd64fd259bbd9c850feda7eb855fc251cdfbc/password%20checker/Image/3.png)

Mình thử debug và trace đến hàm v18. Hàm này có vẻ được gọi từ ```hash.so```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/c61cd64fd259bbd9c850feda7eb855fc251cdfbc/password%20checker/Image/4.png)

Nhấp chuột trái chọn Create function sau đó chuyển sang chế độ Pseudocode để hàm trông dễ nhìn hơn

![](https://github.com/noobmannn/kcscrecruitment2023/blob/c61cd64fd259bbd9c850feda7eb855fc251cdfbc/password%20checker/Image/5.png)

Sau khi kiểm tra kĩ mình nhận ra hàm này chính là hàm ```hash``` bên file ```hash.so```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/c61cd64fd259bbd9c850feda7eb855fc251cdfbc/password%20checker/Image/6.png)

Từ đây rút ra được cách chương trình hoạt động: Đầu tiên kiểm tra số lần được thực thi, nếu còn hợp lệ thì chương trình sẽ yêu câu nhập password, sau đó gọi hàm hash từ ```hash.so``` để encrypt password, sau đó so sánh kết quả với giá trị được trỏ tới bởi ```target[abi:cxx11]```, nếu đúng thì generate flag và in ra màn hình, còn nếu sai thì in ra ```Wrong password bro ~~~```. Sau đó giảm số lần được thực thi của chương trình xuống. Nếu ngay từ đầu mà số lần thực thi đã bằng không thì in ra ```No more attempts remaining! Bye!``` rồi xoá đi hai file ```passwordchecker``` và ```hash.so```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/c61cd64fd259bbd9c850feda7eb855fc251cdfbc/password%20checker/Image/7.png)

Ở file ```hash.so```, mình vào hàm ```hash```, có một đoạn gọi đến hàm ```enc_init```, ở đây khai báo một vài giá trị đặc biệt ```0x67452301```, ```0xEFCDAB89```, ... mình đã nghĩ đây là thuật toán mã hoá hash md5

Tuy nhiên sau khi kiểm tra và thử lại một số trường hợp, mình nhận ra đây là một thuật toán hoàn toàn khác

Vì thế để giải quyết bài này, mình sẽ viết một script để thử tất cả mật khẩu có trong file ```wordlist.txt```.

Ở file ```passwordChecker```, mình debug để lấy giá trị được trỏ tới bởi ```target[abi:cxx11]```, đó là chuỗi ```b99aff88d8e71fba4bce610f4d3cbc8d```

Mình sẽ cho từng password trong file  ```wordlist.txt``` chạy qua hàm ```hash``` của ```hash.so```, rồi so sánh kết quả thu được với ```b99aff88d8e71fba4bce610f4d3cbc8d```, nếu đúng thì in password ra màn hình

```python
import ctypes

def check_text(text):
    hash_lib = ctypes.CDLL('./hash.so')
    hash_lib.hash.restype = ctypes.c_void_p
    hash_array = (ctypes.c_int * 32)()
    hash_lib.hash(hash_array, text.encode('utf-8'))
    result_address = hash_array[0]
    result_string = ctypes.cast(result_address, ctypes.c_char_p).value.decode('utf-8')
    return result_string == 'b99aff88d8e71fba4bce610f4d3cbc8d'

with open('wordlist.txt', 'r', encoding='utf-8', errors='ignore') as file:
    passwords = [line.strip() for line in file]
for password in passwords:
    if check_text(password):
        print(f"Password found: {password}")
        break
```

![](https://github.com/noobmannn/kcscrecruitment2023/blob/c61cd64fd259bbd9c850feda7eb855fc251cdfbc/password%20checker/Image/8.png)

Thử run file ```passwordChecker``` với mật khẩu lấy được, ta thu được flag của bài

![](https://github.com/noobmannn/kcscrecruitment2023/blob/c61cd64fd259bbd9c850feda7eb855fc251cdfbc/password%20checker/Image/9.png)

# Flag

```KCSC{tuyet_voi!!!qua_dep_trai_roi<3<3}```
