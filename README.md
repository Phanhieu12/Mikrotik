Hướng Dẫn Lấy Thông Tin Cấu Hình và Trạng Thái MikroTik RouterOS Qua SSH
Hướng dẫn này cung cấp các lệnh chi tiết để lấy toàn bộ thông tin cấu hình và trạng thái trên hệ điều hành MikroTik RouterOS (ROS) thông qua giao thức SSH. Các lệnh được tổ chức theo từng danh mục để dễ dàng tra cứu và sử dụng. File này phù hợp cho quản trị viên mạng hoặc kỹ thuật viên MikroTik muốn thu thập thông tin hệ thống một cách có tổ chức.
Mục lục

Yêu cầu trước khi bắt đầu
Thông tin hệ thống
Cấu hình mạng
Cấu hình Firewall
Cấu hình Wireless
Cấu hình VPN
Quản lý người dùng
Cấu hình QoS
Cấu hình Routing
Cấu hình System Services
Giám sát và Log
Cấu hình Scripts và Scheduler
Cấu hình khác
Lưu toàn bộ cấu hình
Kiểm tra trạng thái hoạt động
Lấy thông tin theo thời gian thực
Script tổng hợp thông tin
Tài liệu tham khảo

Yêu cầu trước khi bắt đầu

Kết nối SSH: Sử dụng công cụ như PuTTY hoặc terminal với lệnh:ssh admin@<IP_ADDRESS>

(Thay <IP_ADDRESS> bằng địa chỉ IP của RouterOS).
Quyền truy cập: Tài khoản phải có quyền admin.
Lưu đầu ra: Sử dụng lệnh export để lưu cấu hình:/export file=configuration

Tải file về qua SCP:scp admin@<IP_ADDRESS>:/configuration.rsc .


Tham số chi tiết: Sử dụng print detail để lấy thông tin đầy đủ.


1. Thông tin hệ thống (System Information)
Thu thập thông tin về phần cứng, giấy phép, và trạng thái hệ thống.

Thông tin phần cứng:
/system resource print

Hiển thị CPU, RAM, uptime, phiên bản ROS, v.v.

Thông tin bo mạch:
/system routerboard print

Hiển thị model, số serial, firmware, v.v.

Giấy phép:
/system license print


Trạng thái hệ thống:
/system health print

Hiển thị nhiệt độ, điện áp, tốc độ quạt (nếu hỗ trợ).

Cấu hình hệ thống:
/system identity print

Hiển thị tên thiết bị.

Thời gian hệ thống:
/system clock print
/system ntp client print


Lịch sử hoạt động:
/system history print


Gói phần mềm đã cài:
/system package print



2. Cấu hình mạng (Network Configuration)
Thu thập thông tin về interface, IP, VLAN, bridge, v.v.

Danh sách interface:
/interface print detail
/interface monitor-traffic [find]


Cấu hình IP:
/ip address print detail
/ip route print detail
/ip arp print
/ip dns print
/ip dhcp-client print detail
/ip dhcp-server print detail
/ip dhcp-server lease print


Cấu hình IPv6 (nếu sử dụng):
/ipv6 address print detail
/ipv6 route print detail
/ipv6 nd print


VLAN:
/interface vlan print detail


Bridge:
/interface bridge print detail
/interface bridge port print
/interface bridge nat print


PPPoE, VPN, Tunnel:
/interface pppoe-client print detail
/interface pppoe-server server print
/interface ovpn-client print detail
/interface ovpn-server server print
/ip ipsec policy print
/ip ipsec peer print



3. Cấu hình Firewall

Firewall Filter:
/ip firewall filter print detail


NAT:
/ip firewall nat print detail


Mangle:
/ip firewall mangle print detail


Raw:
/ip firewall raw print detail


Service Ports:
/ip firewall service-port print



4. Cấu hình Wireless (Nếu thiết bị hỗ trợ)

Thông tin Wireless:
/interface wireless print detail
/interface wireless registration-table print


Access List:
/interface wireless access-list print


Security Profiles:
/interface wireless security-profiles print detail



5. Cấu hình VPN

PPTP:
/interface pptp-server server print
/interface pptp-client print detail


L2TP:
/interface l2tp-server server print
/interface l2tp-client print detail


IPSec:
/ip ipsec installed-sa print
/ip ipsec policy print
/ip ipsec peer print



6. Quản lý người dùng (User Management)

Danh sách người dùng:
/user print detail
/user active print


AAA (RADIUS):
/radius print
/user aaa print



7. Cấu hình QoS (Quality of Service)

Queue Simple:
/queue simple print detail


Queue Tree:
/queue tree print detail


Queue Types:
/queue type print



8. Cấu hình Routing

Static Routes:
/ip route print detail


Dynamic Routing (OSPF, BGP, RIP):
/routing ospf instance print detail
/routing ospf neighbor print
/routing bgp peer print detail
/routing rip print



9. Cấu hình System Services

Dịch vụ:
/ip service print


Proxy:
/ip proxy print
/ip proxy access print


Socks:
/ip socks print


UPnP:
/ip upnp print
/ip upnp interfaces print



10. Giám sát và Log

Log hệ thống:
/log print


Netwatch:
/tool netwatch print


Traffic Monitor:
/tool traffic-monitor print


Packet Sniffer:
/tool sniffer quick



11. Cấu hình Scripts và Scheduler

Scripts:
/system script print detail


Scheduler:
/system scheduler print detail



12. Cấu hình khác

SNMP:
/snmp print
/snmp community print


NTP:
/system ntp client print
/system ntp server print


Watchdog:
/system watchdog print



13. Lưu toàn bộ cấu hình
Xuất toàn bộ cấu hình vào file:
/export verbose file=full-config

Tải file về qua SCP:
scp admin@<IP_ADDRESS>:/full-config.rsc .

14. Kiểm tra trạng thái hoạt động

Ping và Traceroute:
/ping <IP_ADDRESS> count=5
/tool traceroute <IP_ADDRESS>


Kiểm tra băng thông:
/tool bandwidth-test <IP_ADDRESS>


Kiểm tra tài nguyên:
/system resource monitor



15. Lấy thông tin theo thời gian thực

Theo dõi lưu lượng:
/interface monitor-traffic [find]


Theo dõi kết nối:
/ip firewall connection print



16. Script tổng hợp thông tin
Script mẫu để thu thập thông tin và lưu vào file:
/system script
add name=collect-system-info source={
  :local output "";
  :set output ($output . [/system identity get name] . "\n");
  :set output ($output . [/system resource print as-value] . "\n");
  :set output ($output . [/ip address print as-value] . "\n");
  :set output ($output . [/interface print detail as-value] . "\n");
  :set output ($output . [/ip firewall filter print as-value] . "\n");
  :set output ($output . [/ip route print as-value] . "\n");
  /file print file=system-info.txt value=$output;
}

Chạy script:
/system script run collect-system-info

17. Tài liệu tham khảo

MikroTik Wiki
MikroTik Forum
Hướng dẫn CLI trong ROS: /help


Lưu ý: 

Xóa thông tin nhạy cảm (mật khẩu, khóa VPN, v.v.) trước khi chia sẻ file cấu hình.
Sử dụng lệnh /export compact để xuất cấu hình ngắn gọn, không bao gồm các giá trị mặc định.
Nếu cần tự động hóa, xem xét sử dụng API MikroTik hoặc script.

