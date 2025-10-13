## TCP/IP Commands for Linux

IP networking là trung tâm của cách các thiết bị thông minh ngày nay giao tiếp với nhau qua mạng, bao gồm cả internet.

IP là viết tắt của Internet Protocol và là một phần của bộ giao thức TCP/IP xác định các tiêu chuẩn chung cho mạng máy tính để trao đổi dữ liệu.

Cụ thể, IP điều chỉnh cách gán địa chỉ duy nhất cho từng thiết bị và cách định tuyến các gói dữ liệu giữa nguồn và đích. Linux và các hệ điều hành khác bao gồm các lệnh IP, là các công cụ chuyên dụng để cấu hình và quản lý ngăn xếp mạng TCP/IP.

## What are IP Commands?

IP Commands cung cấp quyền truy cập vào các tham số mạng và cho phép quản trị viên kiểm soát cách thức triển khai TCP/IP trong hệ điều hành

Các lệnh IP cho phép thực hiện các chức năng quan trọng, bao gồm:  

- Assigning IP addresses
- Bringing network interfaces up or down
- Defining routes between networks
- Configuring firewall policies

## Working with Network Interfaces

Một khái niệm quan trọng trong mạng IP là ý tưởng về giao diện mạng. Các kết nối ảo này đại diện cho các cổng phần cứng vật lý cho phép một thiết bị giao tiếp với một hoặc nhiều mạng.

Ví dụ, một máy chủ có thể có các giao diện Ethernet vật lý được gọi là eth0, eth1, v.v. Mặt khác, một máy ảo sẽ có giao diện tun hoặc tap . Các lệnh IP được sử dụng để cấu hình các tham số trên các giao diện này và xác định cách chúng kết nối với mạng. 

Các lệnh TCP/IP có thể được đưa ra để cho phép lập trình viên:

- View status information like MAC address, speed, IP addresses
- Enable/disable interfaces or set to modes like promiscuous
- Assign IP addresses and subnet masks to interfaces
- Define gateway and DNS settings used by interfaces
- Bring interfaces up or down as needed

## Controlling IP Addresses

Địa chỉ IP đóng vai trò là mã định danh duy nhất cho các thiết bị trên mạng TCP/IP. Các gói dữ liệu được định tuyến giữa các nguồn và đích dựa trên các địa chỉ IP này. Cả hệ thống Linux và Windows đều sử dụng lệnh IP để quản lý việc gán địa chỉ. 

Một số tác vụ phổ biến:

- Add or remove static IP addresses from interfaces
- Define primary and secondary IP addresses on an interface
- Display currently assigned addresses and interface properties
- Leverage DHCP for dynamic address allocation
- Manage Address Resolution Protocol (ARP) mappings

## Managing Routes and Traffic Flow

Định tuyến mạng chỉ đạo đường dẫn lưu lượng giữa các mạng và thiết bị. 

Lệnh IP cho phép quản trị viên xác định chính sách định tuyến và bảng quyết định cách chuyển tiếp các gói tin dựa trên IP đích. Điều này bao gồm:

- Setting the default gateway used for external traffic
- Adding or removing static routes for specific IP
- Listing and modifying dynamic routes learned through routing protocols
- Changing route metrics and priorities to influence traffic
- Optimizing performance by adapting routes based on network conditions

## Firewalls and Access Control

Chính sách tường lửa lọc lưu lượng và kết nối dựa trên các quy tắc IP. 

Hệ thống Linux sử dụng lệnh iptables để xác định cài đặt tường lửa. Điều này có thể bao gồm:

- Adding and removing rules to accept or reject certain packets
- Allowing or blocking traffic based on IP addresses, protocols, ports
- Using rules to limit access and implement access control
- Leveraging firewall capabilities provided by IP commands