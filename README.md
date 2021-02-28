# Spark_2
## Phần 1: Tìm hiểu Spark
### A: Spark properties
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Spark properties kiểm soát hầu hết các cài đặt ứng dụng và được cấu hình riêng cho từng ứng dụng, được đặt trực tiếp trên SparkConf (cho phép định cấu hình một số thuộc tính phổ biến như URL chính và tên ứng dụng) được chuyển tới SparkContext. Các key-value thông qua phương thức set(). 
Ví dụ: khởi tạo ứng dụng chạy trong ngữ cảnh phân tán với 2 luồng giá trị.
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374874-92ec6e80-63b9-11eb-92eb-405234070018.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Spark Streaming có thể yêu cầu nhiều hơn 1 luồng để ngăn chặn bất kỳ vấn đề chết đói nào.
Các định dạng khoảng thời gian được chấp nhận:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374942-3d649180-63ba-11eb-8d1c-cfe2c04f9906.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các định dạng kích thước được chấp nhận:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374971-8a486800-63ba-11eb-919f-ff3ad8e7aa8f.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các số không có đơn vị thông thường sẽ được hiểu là byte.
	Mã hóa cứng các cấu hình nhất định trong Spark. Cụ thể là tạo 1 conf trống trong Spark để chạy ứng dụng với các bản gốc khác nhau hoặc số lượng bộ nhớ khác nhau.
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374973-8caac200-63ba-11eb-9ffa-2986856fc2a2.png" width="50%"/>
  
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Cung cấp cấu hình trong thời gian chạy:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374974-8f0d1c00-63ba-11eb-8bb2-a157e19bae3f.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; spark-submit : tải cấu hình động, chấp nhận bất kỳ thuộc tính nào nếu dùng cờ --conf/-c, sử dụng các cờ đặt biệt( dùng ./bin/spark-submit –help để hiện thị tất cả các tùy chọn) cho các lệnh khởi động 
spark--master : hiển thị ở trên
Ví dụ: 
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374975-903e4900-63ba-11eb-9ec0-ce11ed674ad7.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các thuộc tính đặt trực tiếp trên SparkConf có độ ưu tiên cao nhất sau đó là spark-submit hoặc spark-shell sau đó là spark-defaults.conf. Ở các phiên bản mới thì các tên cũ vẩn được chấp nhận nhưng độ ưu tiên sẽ thấp hơn.
Các thuộc tính của Spark được chia làm 2 loại: 
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Liên quan đến triển khai như spark.driver.memory, spark.executor.instances
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Liên quan đến thời gian chạy Spark như spark.task.maxFailures
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Trường hợp ứng dụng:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106383525-31e48b00-63f9-11eb-9a45-1767b9cc35ca.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Giao diện người dùng: 
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106383555-5a6c8500-63f9-11eb-89e8-146f74ba3f2b.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Compression and Serialization: spark.rdd.compress 
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Có nén các phân vùng tuần tự
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Ngoài các loại thuộc tính trên Spark còn hỗ trợ nhiều loại thuộc tính khác nhau: môi trường thực thi (Runtime Environment), quản lý bộ nhớ (Memory Management), hành vi thực thi (Execution Behavior), chỉ số thực thi (Executor Metrics), kết nối mạng (Networking), lập lịch (Scheduling), chế độ thực thi rào cản (Barrier Execution Mode), phân bố động (Dynamic Allocation), cấu hình Thread (Thread Configurations), bảo mật (Security)
	
### B: Spark RDD
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Tập dữ liệu phân tán có khả năng phục hồi (RDD) là một cấu trúc dữ liệu cơ bản của Spark là tập hợp các đối tượng được phân phối bất biến, dữ liệu được chia thành các vùng logic được tính toán trên các nút khác nhau của cụm, chứa các đối tượng của Python, Java, Scala gồm cả các lớp do người dùng định nghĩa. Về hình thức thì RDD là tập hợp các bản ghi được phân vùng và chỉ để đọc được tạo thông qua hoạt động xác đĩnh trên dữ liệu bộ lưu trữ ổn định hoặc các RDD khác. RDD chịu được lỗi có thể hoạt động song song. Có 2 các để tạo RDD: song song và tham chiếu dữ liệu.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Spark sử dụng khái niệm RDD để đạt được các hoạt động MapReduce nhanh hơn và hiệu quả hơn.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD được sinh ra để khắc phục chia sẻ dữ liệu chậm trong MapReduce do sao chép, tuần tự hóa và IO đĩa, hầu hết các ứng dụng Hadoop dành hơn 90% thời gian để thực hiện các thao tác đọc-ghi HDFS. RDD lưu trữ bộ nhớ như một đối tượng trên các công việc và đối tượng có thể chia sẻ giữa các công việc đó. Chia sẻ dữ liệu trong bộ nhớ nhanh hơn mạng và Đĩa từ 10 đến 100 lần.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hoạt động lặp lại trên Spark RDD: lưu trữ các kết quả trung gian trong một bộ nhớ phân tán thay vì Ổ lưu trữ ổn định (Disk) và làm cho hệ thống nhanh hơn.
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374976-92080c80-63ba-11eb-8362-0951029296ae.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hoạt động tương tác trên Spark RDD: nếu các truy vấn khác nhau được chạy lặp lại trên cùng một tập dữ liệu, thì dữ liệu cụ thể này có thể được lưu trong bộ nhớ để có thời gian thực thi tốt hơn.
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374977-93393980-63ba-11eb-8b0a-552172ec3246.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các đặc điểm của Spark RDD: tính toán trong bộ nhớ, lazy evaluations, khả năng chịu lỗi, tính bất biến, phân vùng, sự bền bỉ, hoạt động chi tiết thô, vị trí-độ dính
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD trong Apache Spark hỗ trợ 2 hoạt động: Transformation, Actions.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Transformation
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; o	Spark RDD Transformations là các hàm sử dụng một RDD làm đầu vào và tạo ra một hoặc nhiều RDD làm đầu ra. Chúng ta không thay đổi RDD đầu vào (vì RDD là bất biến và do đó người ta không thể thay đổi nó), nhưng luôn tạo ra một hoặc nhiều RDD mới bằng cách áp dụng các tính toán mà nó đại diện
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; o	Các phép biến đổi là các hoạt động lười biếng trên RDD trong Apache Spark. Nó tạo ra một hoặc nhiều RDD mới, thực thi khi một Action xảy ra. Do đó, Transformation tạo ra một tập dữ liệu mới từ tập dữ liệu hiện có.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; o	Một số phép biến đổi nhất định có thể được pipelined, đây là một phương pháp tối ưu hóa mà Spark sử dụng để cải thiện hiệu suất của các phép tính. Có hai loại phép biến hình: phép biến hình hẹp (narrow transformation), phép biến hình rộng(wide transformation).
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Actions
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; o	Action trong Spark trả về kết quả cuối cùng của các tính toán RDD. Nó kích hoạt thực thi bằng cách sử dụng đồ thị dòng để tải dữ liệu vào RDD ban đầu, thực hiện tất cả các phép biến đổi trung gian và trả về kết quả cuối cùng cho chương trình Driver hoặc ghi nó ra hệ thống tệp. Đồ thị tuyến tính là đồ thị phụ thuộc của tất cả các RDD song song của RDD.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; o	Các Actions là các hoạt động RDD tạo ra các giá trị không phải RDD. Chúng hiện thực hóa một giá trị trong chương trình Spark. Actions là một trong những cách để gửi kết quả từ người thực thi đến driver. First(), take(), Reduce(), collect(), count() là một số Action trong Spark.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; o	Sử dụng các phép biến đổi (Transformations), người ta có thể tạo RDD từ biến hiện có. Nhưng khi chúng ta muốn làm việc với tập dữ liệu thực tế, tại thời điểm đó chúng ta sử dụng Action. Khi Hành động xảy ra, nó không tạo ra RDD mới, không giống như sự chuyển đổi. Do đó, Actions là các hoạt động RDD không cung cấp giá trị RDD. Actions lưu trữ giá trị của nó đối với driver hoặc hệ thống lưu trữ bên ngoài. Nó đưa sự lười biếng (lazy) của RDD vào chuyển động
	
### C: Spark DataFrame
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Spark DataFrame là một tập hợp dữ liệu phân tán được tổ chức thành các cột được đặt tên và cũng được sử dụng để cung cấp các hoạt động như lọc, tính toán tổng hợp, phân nhóm và cũng có thể được sử dụng với Spark SQL. Khung dữ liệu có thể được tạo bằng cách sử dụng các tệp dữ liệu có cấu trúc, cùng với các RDD hiện có, cơ sở dữ liệu bên ngoài và bảng Hive. Về cơ bản, nó được gọi là một lớp trừu tượng được xây dựng trên RDD và cũng được theo sau bởi API tập dữ liệu đã được giới thiệu trong các phiên bản sau của Spark (2.0 +). Hơn nữa, các bộ dữ liệu không được giới thiệu trong Pyspark mà chỉ ở Scala với Spark nhưng đây không phải là trường hợp của Dataframe. Khung dữ liệu phổ biến được gọi là DF là định dạng cột hợp lý giúp làm việc với RDD dễ dàng và thuận tiện hơn, cũng sử dụng các chức năng tương tự như RDD theo cách tương tự. Nếu nói nhiều hơn ở mức độ khái niệm thì nó tương đương với các bảng quan hệ cùng với các tính năng và kỹ thuật tối ưu hóa tốt.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Cách tạo DataFrame: có thể được tạo ra bằng cách sử dụng bảng Hive, cơ sở dữ liệu bên ngoài, tệp dữ liệu có cấu trúc hoặc thậm chí trong trường hợp RDD hiện có. Tất cả các cách này đều có thể tạo các cột được đặt tên này được gọi là Dataframe được sử dụng để xử lý Apache Spark . Bằng cách sử dụng các ứng dụng SQLContext hoặc SparkSession có thể được sử dụng để tạo Dataframe.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hoạt động Spark DataFrames: Trong Spark, khung dữ liệu là sự phân phối và thu thập dạng dữ liệu có tổ chức thành các cột được đặt tên tương đương với cơ sở dữ liệu quan hệ hoặc lược đồ hoặc khung dữ liệu bằng ngôn ngữ như R hoặc python nhưng cùng với mức độ tối ưu hóa phong phú hơn được sử dụng. Nó được sử dụng để cung cấp một loại miền cụ thể của ngôn ngữ có thể được sử dụng để thao tác dữ liệu có cấu trúc.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; DataFrame được phân phối trong tự nhiên, làm cho nó trở thành một cấu trúc dữ liệu có khả năng chịu lỗi và có tính khả dụng cao.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Đánh giá lười biếng là một chiến lược đánh giá giữ việc đánh giá một biểu thức cho đến khi giá trị của nó là cần thiết. Nó tránh đánh giá lặp lại. Đánh giá lười biếng trong Spark có nghĩa là quá trình thực thi sẽ không bắt đầu cho đến khi một hành động được kích hoạt. Trong Spark, bức tranh về sự lười biếng xuất hiện khi các phép biến đổi Spark xảy ra
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; DataFrame là bất biến trong tự nhiên. Bởi bất biến, ý tôi là nó là một đối tượng có trạng thái không thể sửa đổi sau khi nó được tạo. Nhưng chúng ta có thể biến đổi các giá trị của nó bằng cách áp dụng một phép biến đổi nhất định, như trong RDD.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Đọc dữ liệu:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374978-9502fd00-63ba-11eb-8012-5913e78152d9.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Hiển thị dữ liệu:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374979-96342a00-63ba-11eb-806f-960705d14111.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Sử dụng phương thức printSchema:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374982-97fded80-63ba-11eb-85ac-5059de32b6f6.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Sử dụng phương thức select:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374984-992f1a80-63ba-11eb-886c-45fc6e8317e4.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Sử dụng bộ lọc tuổi:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374985-9a604780-63ba-11eb-8ad4-6aba2ccdee1f.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Sử dụng phương pháp groupBy:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374986-9c2a0b00-63ba-11eb-9799-f0b7f43dab0a.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Sử dụng hàm SQL trên SparkSession:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374987-9d5b3800-63ba-11eb-8298-27cb3d1ec89a.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Sử dụng hàm SQL trên một phiên Spark cho chế độ xem tạm thời Toàn cầu:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106374988-9e8c6500-63ba-11eb-9b5f-745a2dfc097e.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Ưu điểm của Spark DataFrame:
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Khung dữ liệu là tập hợp phân tán của Dữ liệu và do đó dữ liệu được tổ chức theo kiểu cột được đặt tên.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Chúng ít nhiều giống với bảng trong trường hợp cơ sở dữ liệu quan hệ và có một tập hợp tối ưu hóa phong phú.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Khung dữ liệu được sử dụng để trao quyền cho các truy vấn được viết bằng SQL và cả API khung dữ liệu
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Nó có thể được sử dụng để xử lý cả loại dữ liệu có cấu trúc và không có cấu trúc.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Việc sử dụng trình tối ưu hóa chất xúc tác giúp tối ưu hóa dễ dàng và hiệu quả.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Các thư viện hiện diện bằng nhiều ngôn ngữ như Python, Scala, Java và R.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Điều này được sử dụng để cung cấp khả năng tương thích mạnh mẽ với Hive được sử dụng để chạy các truy vấn Hive không sửa đổi trên kho tổ ong đã có sẵn.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Nó có thể mở rộng quy mô rất tốt ngay từ một vài kbs trên hệ thống cá nhân đến nhiều petabyte trên các cụm lớn.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Nó được sử dụng để cung cấp mức độ tích hợp dễ dàng với các công nghệ và khuôn khổ dữ liệu lớn khác.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; •	Tính trừu tượng mà họ cung cấp cho RDD hiệu quả và giúp xử lý nhanh hơn.
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Nguồn dữ liệu PySpark: Dữ liệu có thể được tải vào thông qua tệp CSV, JSON, XML hoặc tệp Parquet. Nó cũng có thể được tạo bằng cách sử dụng RDD hiện có và thông qua bất kỳ cơ sở dữ liệu nào khác, như Hive hoặc Cassandra . Nó cũng có thể lấy dữ liệu từ HDFS hoặc hệ thống tệp cục bộ.

## Phần 2: Code minh họa
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hàm count() cho biết số phần tử có trong RDD:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106383803-77558800-63fa-11eb-834d-5c6633323006.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hàm collect() trả về tất cả các phần tử ở trong RDD:
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106383807-7886b500-63fa-11eb-8976-009a66948734.png" width="50%"/>
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hàm filler():
<p align="center"> <img src ="https://user-images.githubusercontent.com/77925421/106383808-7a507880-63fa-11eb-90e6-5d10e747262a.png" width="50%"/>
	
## Phần 3: Tài liệu tham khảo
	
&nbsp;&nbsp;&nbsp;&nbsp; 1. https://spark.apache.org/docs/latest/configuration.html

&nbsp;&nbsp;&nbsp;&nbsp; 2. https://www.tutorialspoint.com/apache_spark/apache_spark_rdd.htm

&nbsp;&nbsp;&nbsp;&nbsp; 3. https://www.educba.com/spark-dataframe/#:~:text=%20Advantages%20of%20Spark%20DataFrame%20%201%20The,and%20also%20the%20data%20frame%20API%20More%20
