## Linux basic folder
- /: Root directory, the top level of the file system.
- /home: User home directories.
- /bin: Essential binary executables.
- /sbin: System administration binaries.
- /etc: Configuration files.
- /var: Variable data (logs, spool files).
- /usr: User programs and data.
- /lib: Shared libraries.
- /tmp: Temporary files.
- /opt: Third-party applications.

Trong Linux, các file descriptor (FD) cơ bản

Mỗi tiến trình khi chạy có sẵn 3 kênh I/O (Input/Output):

| FD |	Tên	Ý nghĩa |	Mặc định đi đâu|
|---| -------------| ----------------- |
| 0 |	stdin |	Standard Input (dữ liệu nhập vào) |	Bàn phím |
| 1 |	stdout |	Standard Output (kết quả bình thường) |	Màn hình (terminal) |
| 2 |	stderr |	Standard Error (thông báo lỗi) |	Màn hình (terminal) |

## stdin

The standard input is the default source of input for programs and typically originates from the keyboard

> Ex: ./your_script.sh < input_file.txt

## stdout

The standard output stream is used to output data that is processed by the program. By default, this is the screen

> Ex: echo "This will be written to file.txt" > file.txt

## stderr

The standard error stream is used by programs to output error messages. By default, stderr is also displayed on the screen.

### Ý nghĩa của 2>&1

- 2 = stderr
- 1 = stdout
- 2>&1 = "chuyển hướng stderr (2) tới cùng nơi mà stdout (1) đang đi".

=> Tức là: gộp chung output và error vào cùng một chỗ.

> Ex: ls /etc /notfound > out.txt 2>&1

Phân biệt sự khác nhau giữa
> out.txt 2>&1 ≠ 2>&1 > out.txt

`> out.txt 2>&1:` stdout → out.txt, rồi stderr → chỗ stdout (out.txt). ✅ gộp cả 2 vào file.

`2>&1 > out.txt:` stderr → stdout (màn hình), rồi stdout → out.txt. ❌ lỗi vẫn hiện ra màn hình, chỉ output vào file.

## Tail Command

The tail command displays the last part (10 lines by default) of one or more files or piped data. It can be also used to monitor the file changes in real time.

> tail [OPTION]... [FILE]...

## Split

Split trong Linux là một công cụ mạnh mẽ được sử dụng để chia các tệp lớn thành các tệp nhỏ hơn, dễ quản lý hơn. 

> split [option] [desired_file_size] original_file.

Option:

|Flag | Description | Example |
|-----|-------------|---------|
|-b | Chỉ định kích thước của mỗi tệp đầu ra. | split -b 100M largefile |
| -l |Chỉ định số dòng mà mỗi tệp đầu ra phải chứa. | split -l 500 largefile |

## Pipes

pipe cho phép bạn sử dụng đầu ra của một lệnh làm đầu vào cho một lệnh khác. Pipe được biểu thị bằng ký tự thanh dọc |.

> command1 | command2

> Ex: ls -l /usr/bin | grep python

## awk

AWK là một công cụ mạnh mẽ để xử lý dữ liệu văn bản có cấu trúc trên Linux, cho phép bạn trích xuất, lọc và chuyển đổi thông tin một cách hiệu quả.

> awk options 'selection _criteria {action }' input-file > output-file

> Ex: awk '{print $3}' server_logs.txt | head -n 5

- awk '{print $3}': AWK in ra trường thứ ba của mỗi dòng
- Chúng ta thêm pipes (|) và lệnh `head -n 5` để giới hạn hiển thị ở 5 dòng đầu tiên.

## File permissions in Linux

### Permissions for files and folder

- Read – Can list all files and copy the files from directory
- Write – Can add or delete files into directory (needs execute permission as well)
- Execute – Can enter the directory

### Understanding file permissions and ownership in Linux

> -rwxrw-r-- 1 abhi itsfoss adsf 457 Aug 10 11:55 agatha.txt

- File type: Denotes the type of file. d means directory, – means regular file, l means a symbolic link.
- Permissions: This field shows the permission set on a file. I’ll explain it in detail in the next section.
- Hard link count: Shows if the file has hard links. Default count is one.
- User: The user who owns the files.
- Group: The group that has access to this file. Only one group can be the owner of a file at a time.
- File size: Size of the file in bytes.
- Modification time: The date and time the file was last modified.
- Filename: Obviously, the name of the file or directory.

> rwxrw-r--

- r : Read permissio
- w : Write permissio
- x : Execute permissio
- – : No permission se

### Change file permissions in Linux

There are two ways to use the chmod command:

- Absolute mode
- Symbolic mode

#### absolute mode

- r (read) = 4
- w (write) = 2
- x (execute) = 1
- – (no permission) = 0

#### symbolic mode

- u = user owner
- g = group owner
- o = other
- a = all (user + group + other)

Operators to perform the permission changes:

- + for adding permissions
- – for removing permissions
- = for overriding existing permissions with new value

> Ex: chmod o-rw+x,u+x agatha.txt

### Change file ownership in Linux

> chown <new_user_name>:<new_user_group> <filename>

If you just want to change the group:

> chown :<new_user_group> <filename>

Or

> chgrp <new_user_group> <filename>


## Daemon

Chương trình chạy nền (background process) — không có giao diện người dùng, không kết thúc khi đóng terminal.

- sshd → daemon của SSH.
- nginx → daemon của web server.
- mysqld → daemon của MySQL.

Daemon thường bắt đầu khi hệ thống khởi động và chạy mãi cho đến khi tắt máy.

## Systemd

Hệ thống init hiện đại trong Linux — nó chịu trách nhiệm khởi động toàn bộ hệ thống (bắt đầu từ PID 1).

- Khi bạn bật máy, kernel → nạp systemd → systemd lần lượt khởi động các daemon, dịch vụ, socket, timer, mount… theo cấu hình.
- Systemd quản lý mọi daemon, theo dõi trạng thái, restart nếu crash, ghi log, thiết lập dependency,...

`systemd` = trình quản lý toàn bộ daemon & service trong hệ thống Linux.

## Systemctl

systemctl là công cụ dòng lệnh dùng để điều khiển systemd.

- Không tương tác trực tiếp với systemd, mà thông qua lệnh systemctl.

## Service
“Service” là một loại daemon cụ thể, thường cung cấp chức năng nào đó (web, database, mail...).

Trong Linux cũ (SysVinit), người ta dùng lệnh service để quản lý các service:

> sudo service nginx start

Trên Linux mới (dùng systemd), lệnh service chỉ là wrapper, thực chất gọi systemctl start nginx.

### Relation

```
[systemctl]  ← (bạn gõ lệnh)
     ↓
[systemd]    ← (hệ thống init, quản lý dịch vụ)
     ↓
[daemon/service] ← (chạy thật, ví dụ nginx, sshd,...)
```
