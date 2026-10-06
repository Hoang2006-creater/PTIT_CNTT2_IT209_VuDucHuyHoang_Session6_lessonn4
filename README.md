# Bài 4: Quản lý tiến trình nền với nohup và tín hiệu Kill

## 1. Tạo shell script

File `loop-monitor.sh`:

```bash
#!/bin/bash

while true; do
    echo "System time: $(date)" >> /tmp/monitor.log
    sleep 5
done
```

Cấp quyền thực thi:

```bash
chmod +x loop-monitor.sh
```

## 2. Khởi chạy tiến trình nền

Sử dụng `nohup` và `&`:

```bash
nohup ./loop-monitor.sh > /dev/null 2>&1 &
```

PID của tiến trình:

```bash
pgrep -f loop-monitor.sh
```

Kết quả:

```text
<PID_CUA_TIEN_TRINH>
```

## 3. Kiểm tra tiến trình

```bash
ps aux | grep loop-monitor.sh
```

Tiến trình `loop-monitor.sh` đang hoạt động dưới nền.

## 4. Kiểm tra file log

```bash
tail -n 10 /tmp/monitor.log
```

Kết quả:

```text
System time: Tue Oct  6 20:40:01 +07 2026
System time: Tue Oct  6 20:40:06 +07 2026
System time: Tue Oct  6 20:40:11 +07 2026
System time: Tue Oct  6 20:40:16 +07 2026
```

Log được ghi tự động khoảng 5 giây một lần.

## 5. Kiểm tra sau khi đăng nhập SSH lại

Sau khi thoát SSH và đăng nhập lại:

```bash
pgrep -af loop-monitor.sh
```

Tiến trình vẫn hoạt động.

Kiểm tra log:

```bash
tail -n 10 /tmp/monitor.log
```

File log tiếp tục được ghi, chứng minh tiến trình chạy độc lập với phiên Terminal nhờ `nohup`.

## 6. Tắt tiến trình bằng SIGTERM

Sử dụng PID đã tìm được:

```bash
kill -15 <PID_CUA_TIEN_TRINH>
```

Kiểm tra lại:

```bash
ps aux | grep loop-monitor.sh
```

Hoặc:

```bash
pgrep -af loop-monitor.sh
```

Tiến trình `loop-monitor.sh` đã được tắt.

## 7. SIGKILL

Nếu `kill -15` không thể tắt tiến trình, có thể sử dụng:

```bash
kill -9 <PID_CUA_TIEN_TRINH>
```

Tuy nhiên trong bài thực hành, tiến trình đã được tắt thành công bằng `SIGTERM (15)` nên không cần sử dụng `SIGKILL (9)`.
