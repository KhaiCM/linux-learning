# The Linux Booting Process

## BIOS/UEFI Firmware Initialization

Khi nhấn nút nguồn, mã BIOS ROM sẽ được thực thi, đánh giá mức độ sẵn sàng của phần cứng và tìm kiếm mã khởi động trên các ổ lưu trữ được chỉ định theo cấu hình firmware. Điều này làm cho BIOS trở nên đặc thù với từng nền tảng và nhà cung cấp.

Các hệ thống hiện đại sử dụng một triển khai tiên tiến hơn gọi là UEFI (Giao diện Phần mềm Mở rộng Hợp nhất) chạy ở chế độ bảo vệ 32-bit hoặc 64-bit trên phần cứng. UEFI cung cấp thời gian khởi động nhanh hơn, khả năng khởi động an toàn và giao diện thiết lập đồ họa.

### Key Takeaways
- BIOS kế thừa thực thi ở chế độ thực 16-bit trực tiếp trên phần cứng
- UEFI chạy ở chế độ bảo vệ 32/64-bit trên phần cứng
- Hơn 492 triệu hệ thống UEFI đã được xuất xưởng vào năm ngoái
- Sử dụng UEFI thay vì BIOS cũ, với tính năng khởi động an toàn được bật để bảo mật

##  Bootloader Initialization via MBR or GPT

Sau khi khởi tạo phần cứng, firmware hệ thống cần xác định và thực thi chương trình bootloader chính để tiếp tục quá trình khởi động. Cách thức hoạt động tùy thuộc vào việc sử dụng chế độ firmware BIOS cũ hay UEFI.

### Master Boot Record (MBR)

- Trong các hệ thống BIOS cũ, Bản ghi khởi động chính (MBR) trong 512 byte đầu tiên của đĩa khởi động chứa mã khởi động có thể thực thi và bảng phân vùng xác định các phân vùng đĩa.

- BIOS đọc MBR vào bộ nhớ và thực thi bộ nạp khởi động Giai đoạn 1 từ mã được nhúng trong đó. Điều này cho phép tải bộ nạp khởi động thứ cấp giai đoạn tiếp theo (Giai đoạn 2).

### GUID Partition Table (GPT)

Thay vào đó, phần mềm UEFI sử dụng phương pháp Bảng phân vùng GUID (GPT) mới hơn, hỗ trợ kích thước ổ đĩa lên đến 9,4 ZB (zettabyte)! Khi khởi động, UEFI sẽ đọc thông tin phân vùng từ header GPT nằm trong sector LBA 1 của đĩa.

Phân vùng GPT sử dụng MBR bảo vệ và tiêu đề phân vùng thứ cấp, cung cấp khả năng dự phòng và sao lưu dữ liệu trong trường hợp hỏng hóc. Hơn nữa, GPT cho phép thiết lập Phân vùng Hệ thống UEFI (ESP) chuyên dụng cho các tệp bộ nạp khởi động như một phần của thông số kỹ thuật UEFI.

Việc phân tách thông tin phân vùng đĩa, dự phòng bảo vệ và lưu trữ bộ nạp khởi động rõ ràng hơn mang lại cho phân vùng GPT một lợi thế mạnh mẽ so với MBR lỗi thời.

### Key Takeaways

- MBR cũ giữ bộ nạp khởi động 512 byte một sector
- Giới hạn MBR ở kích thước đĩa 2 TB
- GPT hỗ trợ đĩa 9.4ZB khổng lồ
- GPT cung cấp khả năng dự phòng và tương thích với UEFI

## Bootloader – Explaining GRUB vs Systemd-Boot

Sau khi khởi tạo chương trình cơ sở xong, quá trình chuyển giao sẽ được chuyển đến bộ nạp khởi động – phần mềm chịu trách nhiệm tải hạt nhân Linux và ramdisk ban đầu vào bộ nhớ.

Bộ nạp khởi động cổ điển vẫn là GRUB (GRand Unified Bootloader), được sử dụng trong hơn 96% bản cài đặt Linux hiện nay. Tuy nhiên, trên các hệ thống UEFI, các bộ nạp khởi động được phân phối như systemd-boot cũng đã được áp dụng rộng rãi.

### GRUB Bootloader

GRUB chứa trình điều khiển thiết bị, ảnh kernel, cấu hình và khả năng viết lệnh/kịch bản hỗ trợ nhiều môi trường khác nhau. Sau khi đọc tệp cấu hình – thường là /boot/grub/grub.cfg – GRUB sẽ hiển thị menu khởi động để chọn một trong các kernel đã được cấu hình:

### systemd-boot on UEFI systems

`systemd-boot` tích hợp chặt chẽ với các hệ thống Linux chạy systemd init. Nó đọc các tùy chọn kernel từ /boot hoặc phân vùng UEFI theo các quy ước systemd-boot thay vì cần cấu hình riêng. Ưu điểm so với GRUB bao gồm:

- Tích hợp UEFI gốc và đơn giản
- Các mục khởi động UEFI tự động
- Hỗ trợ UEFI Secure Boot
- Hoạt động nhanh mà không cần trình nền

## Kernel Initialization

Khi bootloader sắp hoàn tất, đã đến lúc nhân Linux tự tiếp quản! Phần này tập trung vào các tác vụ ban đầu quan trọng mà nó phải thực hiện.

The Linux kernel is the core cung cấp các khả năng thiết yếu như:

- Memory, process, and task management
- Hardware interfaces for devices/drivers
- Filesystem mounting and accessing
- Networking, IPC, synchronization primitives
- System call APIs for user programs

Sau  khi được load vào memory, kernel phải khởi tạo các hệ thống con thông qua module code driver, mount the root filesystem, and prepare the userspace environment.

- Thiết lập chế độ bộ xử lý/vòng bảo vệ
- Khởi tạo lớp trừu tượng phần cứng
- Sự nghiêm ngặt trong xử lý ngắt
- Các hệ thống con của hạt nhân như IPC, đồng bộ hóa, v.v.
- Gắn hệ thống tập tin gốc.
- Xử lý đĩa RAM ban đầu nếu cần.
- Thay đổi thư mục gốc thông qua chroot
- /dev và /sys devtmpfs gắn kết
- Mô-đun hạt nhân tải dựa trên phần cứng