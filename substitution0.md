# substitution0
 | 100 points
Tags: 
AUTHOR: WILL HONG

Description
A message has come in but it seems to be all scrambled. Luckily it seems to have the key at the beginning. Can you crack this substitution cipher?
Download the message [here](https://artifacts.picoctf.net/c/152/message.txt).

* Khi nhìn vào đoạn text ta nhận thấy có 1 dòng ở đầu tiên gồm 26 chữ cái `DECKFMYIQJRWTZPXGNABUSOLVH` và không có chữ nào trùng nhau vì thế có vể đây là bảng chữ cái được thay đổi<br>
* A tương ứng với D, B với E, etc .. <br>
tương tự cho các chữ cái còn lại ta sẽ thay các chữ cái tương trong đoạn text với các chữ cái tương ứng

### Đây là script của tôi
```python
import string

with open('message.txt', 'r') as f:
    mes = f.read()
    
alphabet_change = 'DECKFMYIQJRWTZPXGNABUSOLVH' + 'DECKFMYIQJRWTZPXGNABUSOLVH'.lower()
alphabet = string.ascii_uppercase + string.ascii_lowercase

for i in mes:
    if i in alphabet:
        print(alphabet[alphabet_change.index(i)], end = '')
    else:
        print(i, end = '')
```

### Đây là kết quả:
>ABCDEFGHIJKLMNOPQRSTUVWXYZ <br> <br>
>Hereupon Legrand arose, with a grave and stately air, and brought me the beetle 
from a glass case in which it was enclosed. It was a beautiful scarabaeus, and, at
that time, unknown to naturalistsâ€”of course a great prize in a scientific point
of view. There were two round black spots near one extremity of the back, and a
long one near the other. The scales were exceedingly hard and glossy, with all the
appearance of burnished gold. The weight of the insect was very remarkable, and,
taking all things into consideration, I could hardly blame >Jupiter for his opinion
>respecting it.<br> <br>
>The flag is: picoCTF{5UB5717U710N_3V0LU710N_59533A2E}

Submit flag và nhận điểm thôi ![alt](https://cdn.discordapp.com/emojis/834973312240320562.webp?size=48&name=wrongnumberrapbattle&quality=lossless)