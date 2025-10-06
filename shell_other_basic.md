## PATH

Khi bạn nhập lệnh vào terminal Linux, điều thực sự xảy ra là một chương trình đang được thực thi. Thông thường, để thực thi một chương trình hoặc tập lệnh tùy chỉnh, chúng ta cần sử dụng đường dẫn đầy đủ của nó, chẳng hạn như /path/to/script.shhoặc ./script.shnếu chúng ta đã ở trong thư mục lưu trữ của nó. Ngoài ra, chúng ta có thể thực thi nhiều lệnh mà không cần chỉ định đường dẫn, chỉ bằng cách nhập lệnh như uptimehoặc date, v.v.

Lý do chúng ta không cần chỉ định đường dẫn cho một số lệnh là nhờ $PATHbiến $PATH. Đây là một biến có thể được cấu hình để cho hệ thống Linux biết nơi tìm kiếm các chương trình nhất định. Bằng cách đó, khi nhập lệnh vào terminal, Linux sẽ kiểm tra biến $PATH để xem danh sách các thư mục cần tìm chương trình.

List PATH in linux:

> echo $PATH

Thêm thư mục vào biến PATH sẽ cho phép bạn gọi chương trình hoặc tập lệnh của mình từ bất kỳ đâu trong hệ thống mà không cần phải chỉ định đường dẫn đến nơi bạn lưu trữ chúng.

Chúng ta có thể thêm tạm thời đường dẫn vào $PATH
> Ex: export PATH="/bin/myscripts:$PATH"
Hoặc lưu trữ lâu dài bằng việc thêm vào 3 file sau 

- `~/.bashrc`: Các biến được lưu trữ tại đây sẽ nằm trong thư mục home của người dùng và chỉ người dùng đó mới có thể truy cập. Các biến sẽ được tải bất cứ khi nào một shell mới được mở.
- `/etc/profile`: Các biến được lưu trữ ở đây sẽ có thể được tất cả người dùng truy cập và được tải bất cứ khi nào một shell mới được mở.
- `/etc/environment`: Các biến được lưu trữ ở đây có thể truy cập được trên toàn hệ thống.