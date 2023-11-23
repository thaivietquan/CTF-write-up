## Đề: 
In RSA, a small e value can be problematic, but what about N? Can you decrypt this? [values](https://mercury.picoctf.net/static/12d820e355a7775a2c9129b2622a7eb6/values)
> hint: Bits are expensive, I used only a little bit over 100 to save money

- khi tải file về và mở nó lên tôi nhận được các thông tin như sau:<br>
`Decrypt my super sick RSA: 
c:843044897663847841476319711639772861390329326681532977209935413827620909782846667
n:1422450808944701344261903748621562998784243662042303391362692043823716783771691667
e: 65537%`

- để giải được bài này chắc chắn phải học qua mã RSA rồiii!

## Giải quyết:
Sau đây là công thức giải mã:
$c^{d} \equiv m \pmod{n}$ <br>
việc cần làm là đi tìm d: $de \equiv 1 \pmod{\phi(n)}$ <br>
---> lại phải tìm phi n: $\phi(n) = (p -1) * (q-1)$ <br>
Vì n khá bé (bé vãi) nên mình sẽ phân tích nó thành thừa số nguyên tố và tìm được ngay `q` và `p`
Chúng ta cần sử dụng thư viện <span style="color: red">FactorDB</span>
## Thư viện FactorDB:
>một thư viện Python cung cấp các phương thức để truy vấn cơ sở dữ liệu FactorDB, một cơ sở dữ liệu công khai chứa thông tin về phân tích thành thừa số nguyên tố của các số nguyên lớn. 
Thư viện này cho phép bạn truy vấn cơ sở dữ liệu FactorDB để tìm các thừa số nguyên tố của một số nguyên cụ thể.

đây là script của mình: <br>
```python
from factordb.factordb import FactorDB
from Crypto.Util.number import long_to_bytes
c = 843044897663847841476319711639772861390329326681532977209935413827620909782846667
n = 1422450808944701344261903748621562998784243662042303391362692043823716783771691667
e =  65537

# khởi tạo đối tượng factor_db
factordb = FactorDB(n)
factordb.connect()

q, p = factordb.get_factor_list()

phi = (p-1) * (q-1)
d = pow(e, -1, phi)
plain = pow(c, d, n)

print(plain)
byte_array = long_to_bytes(plain)
print("Flag: {}".format(byte_array.decode('utf-8')))
```
### không biết sao ubuntu của tôi lại không có thư viện Crypto

kết quả là: `13016382529449106065927291425342535437996222135352905256639555294957886055592061
Flag: picoCTF{sma11_N_n0_g0od_00264570}`

23/11/2023 by Quanda
