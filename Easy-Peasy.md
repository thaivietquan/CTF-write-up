## Vấn đề
Description
>A one-time pad is unbreakable, but can you manage to recover the flag? (Wrap with picoCTF{}) `nc mercury.picoctf.net 58913`
```python
#!/usr/bin/python3 -u
import os.path

KEY_FILE = "key"
KEY_LEN = 50000
FLAG_FILE = "flag"


def startup(key_location):
	flag = open(FLAG_FILE).read()
	kf = open(KEY_FILE, "rb").read()

	start = key_location
	stop = key_location + len(flag)

	key = kf[start:stop]
	key_location = stop

	result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), flag, key))
	print("This is the encrypted flag!\n{}\n".format("".join(result)))

	return key_location

def encrypt(key_location):
	ui = input("What data would you like to encrypt? ").rstrip() # nhập vào loại dữ liệu muốn mã hóa
	if len(ui) == 0 or len(ui) > KEY_LEN: 
		return -1

	start = key_location
	stop = key_location + len(ui)

	kf = open(KEY_FILE, "rb").read()
	# đọc file key: chắc là chìa khóa nằm trong đây
	if stop >= KEY_LEN:
		stop = stop % KEY_LEN
		key = kf[start:] + kf[:stop]
	else:
		key = kf[start:stop]
	key_location = stop

	result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), ui, key)) 
	'''
	đoạn code này lặp qua từng phần tử của ui và key sau đó dùng cấu trúc lamda
	để thực hiện phép xor giữa các phần tử sau đó đổi kết quả trả về hệ hexa bằng:
	"{:02x}".format() ---1 kiến thức khá mới ở đây 
	'''

	print("Here ya go!\n{}\n".format("".join(result)))

	return key_location


print("******************Welcome to our OTP implementation!******************")
c = startup(0)
while c >= 0:
	c = encrypt(c)
```
khi truy cập lên sever thì nhận được This is the encrypted flag! `51124f4d194969633e4b52026f4c07513a6f4d05516e1e50536c4954066a1c57`
mã hóa FLAG người ta sử dụng phép toán XOR cho từng phần tử của key và flag sau đó chuyển nó về mã hex
vì xor từng phần tử cho nhau ---> key và flag cùng độ dài
tôi nhận thấy đoạn mã flag gửi ra là gồm 64 ký tự có nghĩa  key và flag cùng độ dài là 64 với kiểu dữ liệu là hex vì mã hex này được định dạng đầu ra là 2 ký tưj ---> flag và key có độ dài là 32
- HƯỚNG GIẢI:
* Phép toán xor: khi xor cho 0 thì bằng chính nó với cách này tôi sẽ in ra được key
```python
if stop >= KEY_LEN:
		stop = stop % KEY_LEN
		key = kf[start:] + kf[:stop]
	else:
		key = kf[start:stop]
	key_location = stop
```
* nếu như độ lài chuỗi lớn hơn hoặc bằng 50000 thì nó sẽ lấy phần dư với 50000 và lấy key và thêm kf[:stop]
* ta sẽ làm sao cho key của ta nhận giá trị là `000..0` có độ dài là 32
* stop ban đầu là len(flag)=32 sau đó cộng thêm độ dài chuỗi mà ta nhập vào
* để lấy được chuỗi key thôi thì cần stop lúc sau là 0
* vì thế ta sẽ nhập vào với độ dài chuỗi là (50000-32) sau đó nó sẽ cộng thêm 32 thì stop lúc sau là 0
* gặp 1 chút rắc rối là khi tôi gửi đến 1 đoạn mã rất nhiều số 0 như thế thì sever nhận không kịp 1 lúc nên đã thực hiện các lệnh phía sau.
* tôi đã tìm hiểu được cách để gửi từng đó ký tự trong 1 lượt
* `python3 -c "print('\x00'*(50000-32) +'\n' + '\x00'*32)" | nc mercury.picoctf.net 58913` 
>trong giao thức truyền thông TCP thì `\x00` đại diện có ký tự null (null byte) mà máy chủ sẽ nhận được và xem nó là số 0
* `\n` như đã biết là ký tự xuống dòng khi này máy chủ sẽ hiểu là `enter` và sau đó thì nhập thêm 32 ký tự không còn lại
* tôi đang phân vẫn ở chỗ là có thể gửi thằng số `0` hay không bởi khi tôi thực hiện flag mà tôi nhận không chính xác vì thế tôi cần tìm hiểu thêm về cách máy tính hiểu và và thực hiện với dữ liệu truyền vào như nào trong các chall tiếp theo
* sau khi thực hiện tôi được key là: `62272a2e7b7d5b505c7830365c783063595c7866325c7864625c7865655c7865`
* tôi thực hiện phép toán xor để được flag mong muốn

```python
from pwn import xor

key = '62272a2e7b7d5b505c7830365c783063595c7866325c7864625c7865655c7865'
flag = '51124f4d194969633e4b52026f4c07513a6f4d05516e1e50536c4954066a1c57'

key_bytes = bytes.fromhex(key)
flag_bytes = bytes.fromhex(flag)

result = xor(key_bytes, flag_bytes)
print(result)

```
- flag là: `35ecb423b3b43472c35cc2f41011c6d2`
