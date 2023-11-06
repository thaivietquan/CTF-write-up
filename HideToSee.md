## vấn đề:
How about some hide and seek heh?
Look at this image [here](https://artifacts.picoctf.net/c/240/atbash.jpg)

khi tải file và mở ảnh lên tôi thấy 2 vòng tròn lồng nhau:
* chữ A tương ứng với z; chữ B tương ứng với y, .... cho đến hết bảng chữ cái alphabet
* điều này có nghĩa chữ cái của cipher text sẽ được thay bằng các ký tự tương ứng.
nhưng ở đây chẳng có đoạn mã nào cả

#Giải:
- Tôi sử dụng lệnh `strings atbash.jpg` nhưng cũng chẳng có đoạn mã nào đặc biệt
- dựa vào gợi ý "Download the image and try to `extract` it."
và tiêu đề của bài là HidetoSee thì chắc chắn đoạn mã đã được giấu
- tôi sử dụng công cụ `steghide` với cú pháp `steghide extract -sf atbash.jpg` dữ liệu ẩn được ghi vào file text 
- đoạn mã là: `krxlXGU{zgyzhs_xizxp_xz00558y}`

như đã phân tích ở trên tôi sẽ giải nó bằng code python
```python
import string
import re
cipher = re.findall(r'.', "krxlXGU{zgyzhs_xizxp_xz00558y}") # lấy tất cả các ký tự
alphabet_low = re.findall(r'\w', string.ascii_lowercase)   
alphabet_up = re.findall(r'\w', string.ascii_uppercase)
alphabet = re.findall(r'\w', string.printable[10:62]) 
print(cipher)
def check(a):
    if a in alphabet_low:
        return True
    else:
        return False
for i in cipher:
    if i in alphabet:
        if check(i):
            print(alphabet_up[25 - alphabet_low.index(i)], end = '')
        else:
            print(alphabet_low[25 - alphabet_up.index(i)], end = '')
    else:
        print(i, end = '')

```
tôi được flag là: `PICOctf{ATBASH_CRACK_CA00558B}`
sửa đổi cho đúng forrm flag và submit
