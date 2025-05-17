# Hướng Dẫn Lấy Thông Tin MikroTik RouterOS Qua SSH

Repository này cung cấp hướng dẫn toàn diện để lấy thông tin cấu hình và trạng thái từ MikroTik RouterOS (ROS) thông qua SSH. Các câu lệnh được sắp xếp theo danh mục, bao gồm thông tin hệ thống, cấu hình mạng, tường lửa, không dây, VPN, và nhiều mục khác. Hướng dẫn này dành cho các quản trị viên mạng và kỹ thuật viên MikroTik.

## Mục Lục
1. [Yêu Cầu Cần Thiết](#yêu-cầu-cần-thiết)
2. [Thông Tin Hệ Thống](#thông-tin-hệ-thống)
3. [Cấu Hình Mạng](#cấu-hình-mạng)
4. [Cấu Hình Tường Lửa](#cấu-hình-tường-lửa)
5. [Cấu Hình Không Dây](#cấu-hình-không-dây)
6. [Cấu Hình VPN](#cấu-hình-vpn)
7. [Quản Lý Người Dùng](#quản-lý-người-dùng)
8. [Chất Lượng Dịch Vụ (QoS)](#chất-lượng-dịch-vụ-qos)
9. [Cấu Hình Định Tuyến](#cấu-hình-định-tuyến)
10. [Dịch Vụ Hệ Thống](#dịch-vụ-hệ-thống)
11. [Giám Sát và Nhật Ký](#giám-sát-và-nhật-ký)
12. [Scripts và Lịch Trình](#scripts-và-lịch-trình)
13. [Cấu Hình Khác](#cấu-hình-khác)
14. [Xuất Toàn Bộ Cấu Hình](#xuất-toàn-bộ-cấu-hình)
15. [Giám Sát Thời Gian Thực](#giám-sát-thời-gian-thực)
16. [Script Mẫu Thu Thập Thông Tin](#script-mẫu-thu-thập-thông-tin)
17. [Lưu Ý](#lưu-ý)
18. [Tài Liệu Tham Khảo](#tài-liệu-tham-khảo)

## Yêu Cầu Cần Thiết
- **Kết nối SSH**: Sử dụng công cụ như PuTTY hoặc terminal để kết nối:
  ```bash
  ssh admin@<ĐỊA_CHỈ_IP>
  ```
  (Thay `<ĐỊA_CHỈ_IP>` bằng địa chỉ IP của thiết bị MikroTik).

- **Quyền Quản Trị**: Đảm bảo tài khoản có quyền admin để truy cập đầy đủ thông tin.

- **Lưu Đầu Ra**: Lưu kết quả lệnh vào file bằng `/export file=<tên_file>` và tải về qua SCP hoặc FTP:
  ```bash
  scp admin@<ĐỊA_CHỈ_IP>:/<tên_file>.rsc .
  ```

- **Ghi Chú**: Các lệnh `print detail` cung cấp thông tin chi tiết hơn. Sử dụng `verbose` trong `/export` để xuất toàn bộ cấu hình.

## Thông Tin Hệ Thống
Lấy thông tin về phần cứng, giấy phép, và trạng thái hệ thống:
```bash
/system resource print           # Thông tin CPU, RAM, thời gian hoạt động, phiên bản ROS
/system routerboard print        # Model, số serial, firmware
/system license print            # Thông tin giấy phép
/system health print             # Nhiệt độ, điện áp, tốc độ quạt (nếu hỗ trợ)
/system identity print           # Tên thiết bị
/system clock print              # Thời gian hệ thống
/system ntp client print         # Cấu hình NTP client
/system history print            # Lịch sử hoạt động
/system package print            # Danh sách gói phần mềm đã cài
```

## Cấu Hình Mạng
Thu thập thông tin về giao diện, địa chỉ IP, và cấu hình mạng:
```bash
/interface print detail                  # Danh sách giao diện
/interface monitor-traffic [find]        # Giám sát lưu lượng giao diện
/ip address print detail                 # Địa chỉ IP
/ip route print detail                   # Bảng định tuyến
/ip arp print                            # Bảng ARP
/ip dns print                            # Cấu hình DNS
/ip dhcp-client print detail             # DHCP client
/ip dhcp-server print detail             # DHCP server
/ip dhcp-server lease print              # Danh sách lease DHCP
/ipv6 address print detail               # Địa chỉ IPv6
/ipv6 route print detail                 # Bảng định tuyến IPv6
/ipv6 nd print                           # Neighbor Discovery IPv6
/interface vlan print detail             # Cấu hình VLAN
/interface bridge print detail           # Cấu hình Bridge
/interface bridge port print             # Cổng Bridge
/interface bridge nat print              # NAT trên Bridge
/interface pppoe-client print detail     # PPPoE client
/interface pppoe-server server print     # PPPoE server
/interface ovpn-client print detail      # OVPN client
/interface ovpn-server server print      # OVPN server
/ip ipsec policy print                   # Chính sách IPSec
/ip ipsec peer print                     # Peer IPSec
```

## Cấu Hình Tường Lửa
Kiểm tra các quy tắc tường lửa, NAT, và mangle:
```bash
/ip firewall filter print detail         # Quy tắc Filter
/ip firewall nat print detail            # Quy tắc NAT
/ip firewall mangle print detail         # Quy tắc Mangle
/ip firewall raw print detail            # Quy tắc Raw
/ip firewall service-port print          # Cổng dịch vụ
```

## Cấu Hình Không Dây
Dành cho thiết bị hỗ trợ không dây:
```bash
/interface wireless print detail                  # Cấu hình không dây
/interface wireless registration-table print      # Thiết bị kết nối
/interface wireless access-list print            # Danh sách truy cập
/interface wireless security-profiles print detail # Hồ sơ bảo mật
```

## Cấu Hình VPN
Lấy thông tin VPN:
```bash
/interface pptp-server server print      # PPTP server
/interface pptp-client print detail      # PPTP client
/interface l2tp-server server print      # L2TP server
/interface l2tp-client print detail      # L2TP client
/ip ipsec installed-sa print             # SA đã cài đặt
/ip ipsec policy print                   # Chính sách IPSec
/ip ipsec peer print                     # Peer IPSec
```

## Quản Lý Người Dùng
Danh sách người dùng và cài đặt xác thực:
```bash
/user print detail                       # Danh sách người dùng
/user active print                       # Người dùng đang hoạt động
/radius print                            # Cấu hình RADIUS
/user aaa print                          # Cấu hình AAA
```

## Chất Lượng Dịch Vụ (QoS)
Kiểm tra cài đặt quản lý băng thông:
```bash
/queue simple print detail               # Queue đơn giản
/queue tree print detail                 # Queue Tree
/queue type print                        # Loại Queue
```

## Cấu Hình Định Tuyến
Xem cấu hình định tuyến tĩnh và động:
```bash
/ip route print detail                   # Định tuyến tĩnh
/routing ospf instance print detail      # OSPF instance
/routing ospf neighbor print             # OSPF neighbor
/routing bgp peer print detail           # BGP peer
/routing rip print                       # RIP
```

## Dịch Vụ Hệ Thống
Kiểm tra các dịch vụ hệ thống:
```bash
/ip service print                        # Dịch vụ (SSH, WWW, FTP, v.v.)
/ip proxy print                          # Proxy
/ip proxy access print                   # Quy tắc truy cập Proxy
/ip socks print                          # SOCKS
/ip upnp print                           # UPnP
/ip upnp interfaces print                # Giao diện UPnP
```

## Giám Sát và Nhật Ký
Giám sát nhật ký và hoạt động mạng:
```bash
/log print                               # Nhật ký hệ thống
/tool netwatch print                     # Netwatch
/tool traffic-monitor print              # Giám sát lưu lượng
/tool sniffer quick                      # Packet Sniffer
```

## Scripts và Lịch Trình
Danh sách script và lịch trình:
```bash
/system script print detail              # Script
/system scheduler print detail           # Lịch trình
```

## Cấu Hình Khác
Các cài đặt hệ thống bổ sung:
```bash
/snmp print                              # SNMP
/snmp community print                    # Cộng đồng SNMP
/system ntp client print                 # NTP client
/system ntp server print                 # NTP server
/system watchdog print                   # Watchdog
```

## Xuất Toàn Bộ Cấu Hình
Xuất toàn bộ cấu hình vào file:
```bash
/export verbose file=full-config
```
Tải file về:
```bash
scp admin@<ĐỊA_CHỈ_IP>:/full-config.rsc .
```

## Giám Sát Thời Gian Thực
Giám sát lưu lượng và kết nối theo thời gian thực:
```bash
/interface monitor-traffic [find]        # Giám sát lưu lượng giao diện
/ip firewall connection print            # Kết nối tường lửa
/ping <ĐỊA_CHỈ_IP> count=5              # Ping
/tool traceroute <ĐỊA_CHỈ_IP>            # Traceroute
/tool bandwidth-test <ĐỊA_CHỈ_IP>        # Kiểm tra băng thông
/system resource monitor                 # Giám sát tài nguyên
```

## Script Mẫu Thu Thập Thông Tin
Script mẫu để thu thập và lưu thông tin hệ thống:
```bash
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
```
Chạy script:
```bash
/system script run collect-system-info
```

## Lưu Ý
- **Đầu Ra Chi Tiết**: Sử dụng `print detail` hoặc `/export verbose` để lấy thông tin đầy đủ.
- **Bảo Mật**: Xóa thông tin nhạy cảm (mật khẩu, khóa VPN) trước khi chia sẻ file cấu hình.
- **Tự Động Hóa**: Sử dụng API của MikroTik hoặc script để tự động thu thập dữ liệu.
- **Gửi Qua Email**: Gửi cấu hình qua email (yêu cầu cài đặt email server):
  ```bash
  /tool e-mail send to="your-email@example.com" subject="Cấu Hình RouterOS" file=full-config.rsc
  ```

## Tài Liệu Tham Khảo
- [MikroTik Wiki](https://wiki.mikrotik.com)
- [MikroTik Forum](https://forum.mikrotik.com)
- Chạy `/help` trong CLI của RouterOS để xem chi tiết lệnh.

---

**Đóng góp**: Chào mừng mọi đóng góp! Vui lòng gửi issue hoặc pull request để cải thiện hướng dẫn này.