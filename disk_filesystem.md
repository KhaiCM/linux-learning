# Inodes

filesystem bao gồm tất cả các tệp thực tế và một cơ sở dữ liệu quản lý các tệp này. Cơ sở dữ liệu này được gọi là inode table.

## What is an inode?

Một inode (index node) là một mục trong table này, và có một mục cho mỗi file. Nó mô tả mọi thứ về tệp, chẳng hạn như:

- File type - regular file, directory, character device, etc.
- Owner
- Group
- Access permissions
Timestamps - mtime (time of last file modification), ctime (time of last - attribute change), atime (time of last access)
- Number of hardlinks to the file
- Size of the file
- Number of blocks allocated to the file
- Pointers to the data blocks of the file - most important!

## When are inodes created?

Khi một hệ thống tệp được tạo, không gian lưu trữ inode cũng được phân bổ. Các thuật toán sẽ xác định dung lượng inode bạn cần tùy thuộc vào dung lượng ổ đĩa và nhiều yếu tố khác. Có lẽ bạn đã từng gặp lỗi do hết dung lượng ổ đĩa. Điều tương tự cũng có thể xảy ra với inode (mặc dù ít phổ biến hơn); bạn có thể hết inode và do đó không thể tạo thêm tệp. Hãy nhớ rằng, lưu trữ dữ liệu phụ thuộc vào cả dữ liệu và cơ sở dữ liệu (inode).

Để xem còn bao nhiêu inode trên hệ thống của bạn, hãy sử dụng lệnh `df -i`.

## Inode information
Các inode được xác định bằng số. Khi một tệp được tạo, nó sẽ được gán một số inode, và số này được gán theo thứ tự tuần tự. Tuy nhiên, đôi khi bạn có thể nhận thấy khi tạo một tệp mới, nó sẽ nhận được số inode thấp hơn các tệp khác. Điều này là do một khi các inode bị xóa, chúng có thể được các tệp khác sử dụng lại. Để xem số inode, hãy chạy ls -li:

```
$ ls -li
140 drwxr-xr-x 2 pete pete 6 Jan 20 20:13 Desktop
141 drwxr-xr-x 2 pete pete 6 Jan 20 20:01 Documents
```


# Filesystem Types

## Common Desktop Filesystem Types

- ext4 - Đây là phiên bản mới nhất của hệ thống tệp Linux gốc. Nó tương thích với các phiên bản ext2 và ext3 cũ hơn. Nó hỗ trợ dung lượng ổ đĩa lên đến 1 exabyte và kích thước tệp lên đến 16 terabyte và nhiều hơn nữa. Đây là lựa chọn tiêu chuẩn cho hệ thống tệp Linux.
- Btrfs - "Better or Butter FS" là một hệ thống tập tin mới dành cho Linux, tích hợp tính năng snapshot, sao lưu gia tăng, tăng hiệu suất và nhiều tính năng khác. Hệ thống này được sử dụng rộng rãi, nhưng vẫn chưa hoàn toàn ổn định và tương thích.
- XFS - Hệ thống tệp nhật ký hiệu suất cao, tuyệt vời cho hệ thống có các tệp lớn như máy chủ phương tiện.
- NTFS và FAT - Hệ thống tập tin Windows
- HFS+ - Hệ thống tập tin Macintosh

## Anatomy of a Disk

Ổ cứng có thể được chia thành nhiều phân vùng, về cơ bản là tạo ra nhiều thiết bị khối. Hãy nhớ lại các ví dụ như, `/dev/sda1` và `/dev/sda2`. `/dev/sda` là toàn bộ đĩa, nhưng `/dev/sda1` là phân vùng đầu tiên trên đĩa đó. Phân vùng cực kỳ hữu ích để phân tách dữ liệu, và nếu bạn cần một hệ thống tệp cụ thể, bạn có thể dễ dàng tạo một phân vùng thay vì biến toàn bộ đĩa thành một loại hệ thống tệp.

## Partition

Disks được  tạo  thành từ các partitions giúp chúng ta tổ chức data. Bạn có thể có nhiều phân vùng trên một đĩa và chúng không thể chồng lấn lên nhau. Nếu có không gian chưa được phân bổ cho một phân vùng, thì đó được gọi là không gian trống. Các loại phân vùng phụ thuộc vào bảng phân vùng của bạn. Bên trong một phân vùng, bạn có thể có một hệ thống tệp hoặc dành một phân vùng cho những thứ khác như trao đổi (swap)

### MBR

- Bảng phân vùng truyền thống được sử dụng làm tiêu chuẩn
- Có thể có phân vùng chính, phân vùng mở rộng và phân vùng logic
- MBR có giới hạn bốn phân vùng chính
- Có thể tạo thêm phân vùng bằng cách chuyển phân vùng chính thành phân vùng mở rộng (mỗi đĩa chỉ có thể có một phân vùng mở rộng). Sau đó, bên trong phân vùng mở rộng, bạn thêm các phân vùng logic. Các phân vùng logic được sử dụng giống như bất kỳ phân vùng nào khác. Thật ngớ - ngẩn, tôi biết.
- Hỗ trợ ổ đĩa lên đến 2 terabyte

### GPT

- Bảng phân vùng GUID (GPT) đang trở thành tiêu chuẩn mới cho phân vùng đĩa
- Chỉ có một loại phân vùng và bạn có thể tạo nhiều loại phân vùng
- Mỗi phân vùng có một ID duy nhất toàn cầu (GUID)
- Được sử dụng chủ yếu kết hợp với khởi động dựa trên UEFI

## Creating Filesystems

>  sudo mkfs -t ext4 /dev/sdb2

## mount and umount

Trước khi có thể xem nội dung hệ thống tập tin, bạn sẽ phải mount nó. Để làm điều đó, cần vị trí thiết bị, loại hệ thống tập tin và điểm gắn kết

> sudo mount -t ext4 /dev/sdb2 /mydrive

Bây giờ, khi truy cập /mydrive, chúng ta có thể xem nội dung hệ thống tệp. Tham số -t chỉ định loại hệ thống tệp.

Để ngắt kết nối thiết bị khỏi điểm gắn kết:

> sudo umount /mydrive

## swap

Swap là thứ chúng ta dùng để phân bổ bộ nhớ ảo cho hệ thống. Nếu bộ nhớ của bạn thiếu, hệ thống sẽ sử dụng phân vùng này để "hoán đổi" các phần bộ nhớ của các tiến trình nhàn rỗi sang ổ đĩa, giúp bạn không bị quá tải bộ nhớ.

## Disk Usage

> df -h

Hiển thị mức sử dụng của các hệ thống tệp hiện đang được gắn kết. -hCờ này cung cấp cho bạn định dạng dễ đọc. Bạn có thể xem thiết bị là gì và dung lượng đã sử dụng và khả dụng là bao nhiêu.
