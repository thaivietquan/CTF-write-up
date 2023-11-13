# New Caesar:
>Description
We found a brand new type of encryption, can you break the secret code? (Wrap with picoCTF{}) <br>lkmjkemjmkiekeijiiigljlhilihliikiliginliljimiklligljiflhiniiiniiihlhilimlhijil [new_caesar.py](https://mercury.picoctf.net/static/c9043977604318594ab73d126a01d0b1/new_caesar.py)

## Giải thích cách mã hóa:
+ mã hóa này sử dụng key là 1 chữ cái trong 16 chữ `abcdefghijklmnop
` <br>---> ta sẽ giải mã nó bằng bruteforce
- ban đầu FLAG  được lặp qua  từng phần tử và chuyển nó thành chuỗi bytes gồm 8 bit <br>
```python
def b16_encode(plain):
	enc = ""
	for c in plain:
		binary = "{0:08b}".format(ord(c))
		enc += ALPHABET[int(binary[:4], 2)] + ALPHABET[int(binary[4:], 2)]
	return enc
```
* 8 bit này được chia ra làm đôi và đưa về lại số nguyên sau đó lấy giá trị tương ứng với 2 số đó trong bảng ALPHABET. tạo thành 1 chuỗi b16
* chuỗi b16 này tiếp tục được đưa vào hàm shift(c, k). 
```python
def shift(c, k):
	t1 = ord(c) - LOWERCASE_OFFSET
	t2 = ord(k) - LOWERCASE_OFFSET
	return ALPHABET[(t1 + t2) % len(ALPHABET)]
```
## Cách giải:
- Ta sẽ bruteforce cho từng key trong `abcdefghijklmnop`
- Ta sẽ tìm chuỗi ký tự b16 bằng hàm shift như cũ:
> ta nhận thấy các ký tự của chuỗi enc là flag sau khi bị mã hóa đều được lấy tù bảng ALPHABET đã cung cấp lúc đầu. tôi nghĩa là sử dụng phép tính đồng dư tôi sẽ cập nhật chi tieét tại sao tôi có thể làm được như vậy. <br>
![alt](https://cdn.discordapp.com/emojis/1092284790578876567.webp?size=128&quality=lossless)

```python
def unshift(c, k):
	t1 = ord(c) - LOWERCASE_OFFSET
	t2 = ord(k) - LOWERCASE_OFFSET
	return ALPHABET[(t1 + t2) % len(ALPHABET)]
```

- khi đã tìm được đoạn b16:
- ta lặp qua từng phần tử chuyển nó hệ nhị phân và ghép chúng lại 2 cái một tại vì ban đầu chúng bị tách ra làm đôi.
```python
def b16_decode(plain):
	dec = ""
	for i in range(0, len(plain), 2):
		a = "{0:04b}".format(int(ALPHABET.index(plain[i])))
		b = "{0:04b}".format(int(ALPHABET.index(plain[i+1])))
		
		dec += chr(int(a + b, 2)) 
	print(dec)
```
Và đây là script của tôi
```python
import string

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]

enc = "lkmjkemjmkiekeijiiigljlhilihliikiliginliljimiklligljiflhiniiiniiihlhilimlhijil"

def b16_decode(plain):
	dec = ""
	for i in range(0, len(plain), 2):
		a = "{0:04b}".format(int(ALPHABET.index(plain[i])))
		b = "{0:04b}".format(int(ALPHABET.index(plain[i+1])))
		
		dec += chr(int(a + b, 2)) 
	print(dec)


def unshift(c, k):
	t1 = ord(c) - LOWERCASE_OFFSET
	t2 = ord(k) - LOWERCASE_OFFSET
	return ALPHABET[(t1 + t2) % len(ALPHABET)]

		
for i in ALPHABET:
    b16 = ''	
    for j in enc:
        b16 += unshift(j, i)
    b16_decode(b16)
	
```
kết quả tôi nhận được rất nhiều nhưng có một dòng nó có vẻ là tôi hiểu là ngôn ngữ cho con người hiểu: ![alt](https://cdn.discordapp.com/emojis/906464497168969758.gif?size=128&quality=lossless)  <br> <br>
`et_tu?_431db62c5618cd75f1d0b83832b67b46`<br>
thêm form flag và submit thôi: `picoCTF{et_tu?_431db62c5618cd75f1d0b83832b67b46}`

Ohshit cả 1 buổi tối của tôi Crypto thật khó nhai