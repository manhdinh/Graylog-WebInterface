###Một số kỹ thuật làm việc với WebInterface của Graylog
##Mục luc

*	[1.	Giới thiệu chung]
*	[2. Search]
*	[3. Stream]
*	[4. Dashboard]
*	[5. Source]
*	[6. System]

###1. Giới thiệu chung

Đăng nhập Web-Interface trên Web-Browser với cú pháp :
http://IP-GraylogServer:9000
<img src="http://i.imgur.com/N88Guvw.png">

Trên dashboard của Web-Interface có 5 mục chính :
•	Search 
•	Stream
•	Dashboard
•	Sources
•	System

<img src="http://i.imgur.com/buQz6VM.png">

Kịch bản ở đây là : Chúng ta đã có sẵn 1 máy Graylog-Server hoàn chỉnh, và đã có sẵn những Input được đẩy về (Cách cài đặt Graylog-server đã được giới thiệu ở bài trước, và các kiểu cài đặt và cấu hình Input cụ thể sẽ được giới thiệu ở phần System => Input)

###2.	Search
####2.1 Định nghĩa
 Dùng tìm kiếm các bản tin liên quan đến từ khóa nhập vào. Việc search do Elasticsearch đảm nhận toàn bộ. Để tham khao thêm vào các cú pháp tìm kiếm, tham khảo
link sau : http://docs.graylog.org/en/1.2/pages/queries.html. 

    Ví dụ 1 : Search tất cả các bản tin chứa ssh :
	<img src="http://i.imgur.com/AzX8xVh.png">
	
1 : Tùy chỉnh thời gian search ( search các bản tin trong vòng 5, 15, 30 phút, 1h, 2h… 1 ngày, 2 ngày… ).

2 : Ô tìm kiếm, nơi ta nhập term muốn tìm kiếm ( trong trường hợp này là ssh ).

3 : Phần Message sẽ hiện các bản tin chứa chính xác những term ta tìm kiếm.

####2.2 Công dụng
	Giúp việc tìm kiếm thông tin được nhanh chóng và chính xác.

####2.3 Thành phần

<img src="http://i.imgur.com/W5gEQQF.png">

1 : Hiện kết quả tìm kiếm, ví dụ kết quả ở đây báo là tìm kiếm thấy 1 bản tin chưa term “ ssh”, và chỉ tìm kiếm trên 1 index.

2 : Biểu đồ kết quả tìm kiếm. Graylog sẽ tạo 1 biểu đồ hiển thị số bản tin tìm kiếm theo thời gian ( Có thể tùy chỉnh thời gian tìm kiếm )

3 : Message : Hiển thị nội dung bản tin. Bản tin nhận về sẽ chưa tất cả những thông tin có liên quan trong bản tin log chưa term “ssh”

4 : Fields : Hiển thị những trường lọc được từ bản tin, giúp việc đọc bản tin dễ dàng hơn, và có thể tạo thông số và biểu đồ để thêm vào dashboard. Ta cũng có thể tạo thêm những field này với kỹ thuật tạo regex ( Regular Expression ) sẽ được giới thiệu ở phần sau. Ví dụ ở hình dưới ta show ra 2 nội dung là Statistics và Quick value cho 2 trường message và source, trên giao diện sẽ hiển thị như sau :

<img src="http://i.imgur.com/gvVZtV0.png">

Các bảng và biểu đồ này ta có thể thêm vào dashboard, giúp việc đọc dữ liệu dễ dàng hơn.

### 3. Stream
####3.1 Định nghĩa
 Là kỹ thuật định tuyến bản tin tới một chỉ mục nhất định. Ví dụ, tôi chỉ muốn đọc các bản tin liên quan ssh thì sẽ dùng Stream để lọc các bản tin về ssh được đẩy về.
####3.2 Công dụng
 -	Phân luồng thông tin.
 -	Có thể đẩy các bản tin trong Stream qua một output khác để xử lý.
 -	Dùng cho việc cảnh báo ( qua Email, Slack)
####3.3 Tạo và kiểm soát Stream

Stream dùng những rule riêng để phân luồng và lấy về các bản tin cần thiết. 

Trong mục Stream, ta click vài Create Stream, sau đó điền Title và Description cho Stream.
<img src ="http://i.imgur.com/xXIs7Nk.png">
Click vào Edit Rule, sau đó thêm Rule mới cho Stream. Ví dụ ở đây ta sẽ tạo rule để thu thập các bản tin chứa regex là : Failed password for .+ from .+
<img src="http://i.imgur.com/lk3wFrz.png">
<img src="http://i.imgur.com/3kPHnOo.png">
Stream SSH Fail sẽ bắt tất cả những bản tin chứa chuỗi :Failed password for… from …
<img src="http://i.imgur.com/nJ7TbJ7.png">
####3.4Cảnh báo với Stream

### 4. Dashboard
####4.1 Định nghĩa
 Là nơi để hiển thị biểu đồ, số liệu, bảng thống kê... được tạo trong mục Search 
 <img src="http://i.imgur.com/tS2djC5.png">
####4.2 Công dụng
Giúp người dùng dễ dàng trong việc thống kê, tìm kiếm các thông tin được lọc từ bản tin log.
###4.3 Tạo và kiểm soát Dashboard 
Trên Dashboard, tạo một Dashboard mới, thêm Title và Description cho Dashboard
<img src="http://i.imgur.com/3OztHjw.png">
Sau khi tạo xong, Dashboard hiện giờ vẫn đang rỗng, ta cần thêm thông số vào Dashboard thông qua Search hoặc Stream.

Ví dụ : Dashboard thống kê số lần đăng nhập SSH thành công, thất bại, và tổng số đăng nhập trong 30 phút
<img src="http://i.imgur.com/edGkJIc.png">
1 : Nhập term tìm kiếm ở mục search, biểu thức tìm kiếm ở mục 1 có ý nghĩa là : Tìm kiếm các bản tin các chính xác 2 cụm từ : Failed Password hoặc Accepted Password. 

2 : Hiện lên thông số và biểu đồ thống kê cho từng field, ví dụ ở đây khi chọn filed là source và chọn mục Quick Value, sẽ hiện biểu đồ và bảng thống kê những source nào có xuất hiện các bản tin trên. 

3 : Chọn các mục muốn thêm vào Dashboard, ví dụ muốn thêm tổng số lần đăng nhập SSH, ta thêm theo 3.1
<img src="http://i.imgur.com/Ve4COKD.png">
Làm tương tự với 3.2. Ta có Dashboard sau 
<img src="http://i.imgur.com/lqtjzSw.png">
### 5. Source
####5.1 Định nghĩa 
Sources thống kê số bản tin nhận về theo thời gian dưới dạng biểu đồ, và ip đang đẩy log về Graylog
<img src="http://i.imgur.com/RtiGGSB.png">
### 6. System

Trong phần này tôi sẽ giới thiệu 2 mục quan trọng đó là Input và kỹ thuật Regex để tạo ra các Extractor.
####6.1 Input 
#####Định nghĩa
Input giống như một địa chỉ nhà, các bản tin từ máy client sẽ được cung cấp thông tin về địa chỉ đó để có thể đẩy được log về cho Graylog Server.

Có rất nhiều dạng Input nhưng ở đây chúng ta sẽ chỉ đề cập đến 2 loại input là GELF TCP và Syslog để sử dụng với 2 kỹ thuật đẩy log là đẩy bằng Syslog thuần túy và đẩy bằng Graylog Collector.

#####6.1.1.	GELF TCP
GELF TCP là input chuyên dùng cho Graylog collector, xem hướng dẫn về Graylog-Collector theo link sau :
https://github.com/hocchudong/ghichep-graylog/tree/master/graylog-collector

<img src="http://i.imgur.com/iTUv0aj.png">
Sau khi launch input mới, ta nhập các thông tin cần thiết vào bảng
<img src="http://i.imgur.com/1xxxatn.png">
<img src="http://i.imgur.com/WvRdRou.png">
Một số mục cần lưu ý khi nhập thông tin :
•	Bind IP : Nhập IP của Graylog-Server  hoặc 0.0.0.0 ( Nếu đặt 0.0.0.0 Graylog server sẽ lắng nghe tất cả các bản tin trả về, chỉ đặt nếu đã thiết lập IPTables)
•	Port : Chú ý đặt trùng với port thiết lập trong file config của máy Collector Client ( mặc định của cả 2 là 12201 )
•	Bạn có thể tìm hiểu thêm về cơ chế đẩy log với TLS để sử dụng TLS với Graylog-Colector để bảo mật tốt hơn khi truyền các bản tin log.

Sau khi launch xong input, cần có 2 phần của Input cần lưu ý
<img src="http://i.imgur.com/hkwHTEE.png">
1 : Kiểm tra xem các bản tin log đã được đẩy về hay chưa. ( Các bản tin sẽ bắt đầu đẩy về sau khi graylog-collector service khởi động )

2 : Quản lý các extractor được tạo ở phần searching. 

####6.1.2.	Syslog UDP

Nhận những bản tin được đẩy bằng syslog thuần túy.
Các thông số cũng giống với input GELF
<img src="http://i.imgur.com/JAeFGgr.png">
