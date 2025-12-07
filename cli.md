Dưới đây là **các lệnh Command Line để kiểm tra tình trạng SSD trên macOS** (áp dụng tốt khi mua MacBook cũ). Mình chia làm **3 nhóm: kiểm tra sức khỏe – kiểm tra mức độ hao mòn – test tốc độ đọc ghi**.

---

# 1. KIỂM TRA SSD CÒN “SỐNG” HAY SẮP CHẾT (SMART)

### Cách 1: diskutil

Open **Terminal** :

```bash
diskutil info disk0 | grep SMART

SMART Status: Verified # Good
SMART Status: Failing # Bad
```

---

# 2. KIỂM TRA ĐỘ HAO MÒN – TỔNG DỮ LIỆU ĐÃ GHI (QUAN TRỌNG NHẤT)

macOS mặc định **KHÔNG hiển thị chi tiết Wear Level**, nên bạn cần cài `smartmontools`.

```bash
# install brew (if not existed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
#install smartmontools
brew install smartmontools
## check info
sudo smartctl -a /dev/disk0
```

Bạn cần quan tâm mấy dòng sau:

| Thông số                    | Ý nghĩa              |
| --------------------------- | -------------------- |
| **Power_On_Hours**          | Tổng giờ SSD đã chạy |
| **Data Units Written**      | Tổng dữ liệu đã ghi  |
| **Wear Leveling Count**     | Mức độ hao mòn       |
| **Media_Wearout_Indicator** | % tuổi thọ SSD       |

---

### Đánh giá SSD còn tốt hay không:

| Data đã ghi | Đánh giá        |
| ----------- | --------------- |
| < 10 TB     | Gần như mới     |
| 10–50 TB    | Bình thường     |
| 50–150 TB   | Đã dùng nhiều   |
| > 150 TB    | ⚠️ SSD cày nặng |
| > 300 TB    | ❌ Không nên mua |

---

# 3. TEST speed read/write SSD (PHÁT HIỆN SSD YẾU, SSD LỖI)


```bash
# Test write:
time dd if=/dev/zero of=~/testfile bs=1024k count=4096
# Test read:
time dd if=~/testfile of=/dev/null bs=1024k
# Done remove file
rm ~/testfile
```

---

### Tốc độ bình thường của SSD MacBook:

| Loại SSD          | Ghi / Đọc chuẩn    |
| ----------------- | ------------------ |
| MacBook M1/M2/M3  | 2,500 – 3,500 MB/s |
| MacBook Intel SSD | 1,500 – 2,500 MB/s |
| SSD yếu / lỗi     | < 800 MB/s         |

---

# 4. THEO DÕI SSD KHI MÁY ĐANG CHẠY

Lệnh theo dõi realtime:

```bash
iostat -d 1
```

→ Xem:

* SSD có bị load cao bất thường không
* Có nghẽn ghi đọc không

---

# TÓM TẮT CỰC NGẮN (BẠN CHỈ CẦN 3 LỆNH NÀY)

```bash
diskutil info disk0 | grep SMART
sudo smartctl -a /dev/disk0
time dd if=/dev/zero of=~/testfile bs=1024k count=4096
```

#  Mac diagnostics from Terminal
```bash
sudo sysdiagnose -f ~/Desktop/
```

| **Mã lỗi** | **Chẩn đoán / Ý nghĩa**                                                          |
| ---------- | -------------------------------------------------------------------------------- |
| **ADP000** | Không phát hiện lỗi nào (No issues found).                                       |
| **VFD001** | Có thể có lỗi liên quan đến màn hình hoặc bộ xử lý đồ họa (GPU).                 |
| **VFD002** | Có thể có lỗi liên quan đến màn hình hoặc bộ xử lý đồ họa (GPU).                 |
| **VFD003** | Có thể có lỗi liên quan đến màn hình hoặc bộ xử lý đồ họa (GPU).                 |
| **VFD004** | Có thể có lỗi liên quan đến màn hình hoặc bộ xử lý đồ họa (GPU).                 |
| **NDC001** | Có thể có lỗi với camera. Apple Diagnostics không thể truy cập camera tích hợp.  |
| **NDD001** | Có thể có lỗi phần cứng USB, ảnh hưởng đến thiết bị kết nối qua cổng USB.        |
| **PFR001** | Có thể có lỗi firmware. Nếu máy khởi động gặp vấn đề, có thể do firmware bị lỗi. |
| **PPT001** | Không phát hiện pin (Battery was not detected).                                  |
| **PPR001** | Có thể có lỗi với bộ xử lý (CPU).                                                |
| **PPP001** | Có thể có lỗi với bộ sạc / adapter nguồn.                                        |
| **NDK001** | Có thể có lỗi với bàn phím.                                                      |
