## Bài đầu tiên về web:
> link: https://play.picoctf.org/practice/challenge/274?category=1&originalEvent=70&page=1

## bài này cho ta 1 trang web:
> http://saturn.picoctf.net:58519/

## Giải:
- sau khi truy cập có những đoạn text cũng chả hiểu nó nói gì
- phí dưới có 1 'button' "Say Hello" bấm vào thì màn hình hiển thị 'This code is in a separate file!'
- vậy có thể flag bị tách ra và nẳm ở các file source code
- kiểm tra thì thấy có đoạn command nằm ở file "style.css" và  file "scrip.js" chính là flag
- ghép nó lại thôi:   'picoCTF{1nclu51v17y_1of2_f7w_2of2_b8f4b022}'