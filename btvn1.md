# BÀI 1: HỆ THỐNG GIÁM SÁT SỰ CỐ WEBSITE DOANH NGHIỆP

## PHẦN 1: PHÂN TÍCH

### 1. Chỉ ra lỗi sai trong lập luận của Nam về DNS và mã trạng thái 500

**Lỗi sai 1: Nhầm lẫn giữa lỗi DNS và lỗi HTTP 500**

- DNS Lookup có nhiệm vụ phân giải tên miền `lms.ptit.edu.vn` thành địa chỉ IP của máy chủ.
- Nếu DNS không tìm thấy địa chỉ IP, trình duyệt thường hiển thị lỗi không phân giải được tên miền (DNS_PROBE_FINISHED_NXDOMAIN), chứ không phải lỗi HTTP 500.
- Vì Nam nhận được thông báo `500 Internal Server Error`, điều đó cho thấy trình duyệt đã kết nối được với máy chủ và nhận được phản hồi HTTP.

**Lỗi sai 2: Cho rằng đổi DNS sang 8.8.8.8 sẽ khắc phục lỗi 500**

- DNS Google `8.8.8.8` là một máy chủ DNS công cộng, có nhiệm vụ phân giải tên miền.
- Tuy nhiên, lỗi 500 thường xuất phát từ quá trình xử lý yêu cầu phía Server.
- Việc đổi DNS không giải quyết được lỗi trong mã nguồn, Backend hoặc cơ sở dữ liệu của website.

---

## PHẦN 2: HOÀN THIỆN

### 2. Mô tả sơ đồ luồng dữ liệu gói tin từ DNS Lookup đến Server Response

**Sơ đồ luồng hoạt động:**

```text
[1. Người dùng nhập URL]
       |
       v
[lms.ptit.edu.vn]
       |
       v
[2. DNS Lookup]
       |
       v
[DNS phân giải tên miền thành IP]
       |
       v
[3. TCP/TLS Handshake]
       |
       v
[Thiết lập kết nối HTTPS an toàn]
       |
       v
[4. HTTP Request]
       |
       v
[Web Server tiếp nhận yêu cầu]
       |
       v
[Backend xử lý yêu cầu]
       |
       v
[Truy vấn Database]
       |
       v
[Backend gặp lỗi nội bộ]
       |
       v
[5. HTTP Response: 500 Internal Server Error]
       |
       v
[6. Trình duyệt hiển thị thông báo lỗi]