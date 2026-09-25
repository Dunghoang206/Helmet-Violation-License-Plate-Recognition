## Cơ sở lý thuyết

### 1. Ultralytics YOLO26

**Ultralytics YOLO26** là một họ mô hình thị giác máy tính thời gian thực
(real-time computer vision) được phát triển bởi Ultralytics. YOLO là viết tắt
của **You Only Look Once**, thể hiện cách tiếp cận thực hiện dự đoán đối tượng
từ ảnh đầu vào trong một quá trình suy luận thống nhất.

YOLO26 được Ultralytics thiết kế để hỗ trợ nhiều bài toán thị giác máy tính,
trong đó có:

- Object Detection (phát hiện đối tượng)
- Instance Segmentation (phân đoạn đối tượng)
- Image Classification (phân loại ảnh)
- Pose Estimation (ước lượng tư thế)
- Oriented Bounding Box - OBB (phát hiện đối tượng bằng hộp giới hạn xoay)
- Các tác vụ thị giác máy tính khác được hỗ trợ trong hệ sinh thái Ultralytics

Trong đề tài này, YOLO26 được nghiên cứu chủ yếu cho bài toán **Object Detection**,
nhằm xác định vị trí các đối tượng liên quan đến hành vi không đội mũ bảo hiểm
và biển số phương tiện trong ảnh hoặc video giao thông.

> Tài liệu chính thức:
> https://docs.ultralytics.com/models/yolo26/

---

### 2. Các biến thể của YOLO26

Ultralytics cung cấp YOLO26 với nhiều kích thước mô hình khác nhau:

| Biến thể | Ý nghĩa | Đặc điểm tổng quát |
|----------|---------|--------------------|
| YOLO26n | Nano | Nhỏ và ưu tiên tốc độ |
| YOLO26s | Small | Nhỏ, cân bằng hơn giữa tốc độ và độ chính xác |
| YOLO26m | Medium | Kích thước trung bình |
| YOLO26l | Large | Mô hình lớn |
| YOLO26x | Extra Large | Biến thể lớn nhất trong nhóm |

Các biến thể cho phép lựa chọn mô hình phù hợp với tài nguyên phần cứng,
tốc độ suy luận và yêu cầu độ chính xác của bài toán.

Trong quá trình nghiên cứu, hiệu quả của mô hình cần được đánh giá thực nghiệm
trên tập dữ liệu của đề tài thay vì chỉ dựa vào kích thước mô hình.

---

### 3. Một số đặc điểm của YOLO26

Theo tài liệu chính thức của Ultralytics, YOLO26 tập trung vào khả năng suy luận
thời gian thực và triển khai hiệu quả.

Một số đặc điểm đáng chú ý bao gồm:

- Hỗ trợ **end-to-end inference**.
- Có khả năng tạo dự đoán mà không cần bước Non-Maximum Suppression (NMS)
  truyền thống khi sử dụng chế độ end-to-end.
- Detection head được đơn giản hóa.
- Loại bỏ Distribution Focal Loss (DFL) trong hồi quy bounding box.
- Sử dụng các cải tiến trong quá trình huấn luyện như MuSGD,
  Progressive Loss và Small-Target-Aware Label Assignment (STAL).
- Hỗ trợ huấn luyện, đánh giá, dự đoán và export mô hình thông qua
  hệ sinh thái Ultralytics.

Các đặc điểm trên khiến YOLO26 phù hợp để nghiên cứu các bài toán phát hiện
đối tượng trong ảnh và video, tuy nhiên hiệu quả đối với dữ liệu giao thông
của đề tài vẫn cần được kiểm chứng bằng thực nghiệm.

---

### 4. Object Detection trong đề tài

Object Detection là bài toán xác định:

1. Đối tượng xuất hiện trong ảnh thuộc lớp nào.
2. Đối tượng nằm ở vị trí nào trong ảnh.

Kết quả của mô hình phát hiện đối tượng thường bao gồm:

- Nhãn lớp (class)
- Bounding box
- Confidence score

Ví dụ:

Ảnh/video giao thông

→ YOLO26

→ phát hiện đối tượng

→ xác định bounding box và confidence

→ xử lý kết quả phát hiện

Trong đề tài, mô hình được huấn luyện/fine-tune trên dữ liệu phù hợp để nhận
diện các lớp đối tượng cần thiết cho việc phát hiện hành vi không đội mũ bảo
hiểm và xác định vùng biển số.

---

### 5. Nhận dạng biển số bằng OCR

YOLO26 đảm nhiệm việc **phát hiện vị trí biển số**, nhưng không có nghĩa rằng
mô hình tự động đọc được nội dung ký tự trên biển số.

Sau khi vùng biển số được phát hiện, ảnh biển số được cắt (crop) và chuyển
sang mô-đun **OCR - Optical Character Recognition** để nhận dạng ký tự.

Quy trình tổng quát:

Video giao thông
        ↓
Phát hiện đối tượng bằng YOLO26
        ↓
Phát hiện trường hợp cần xử lý
        ↓
Phát hiện vùng biển số
        ↓
Crop biển số
        ↓
OCR
        ↓
Chuỗi ký tự biển số

Do đó:

- **YOLO26:** xác định đối tượng/vị trí đối tượng.
- **OCR:** nhận dạng nội dung ký tự trên biển số.

---

### 6. Pipeline tổng thể

### 6. Pipeline tổng thể

Pipeline dự kiến của hệ thống:

```text
Video giao thông
        ↓
Tiền xử lý dữ liệu
        ↓
YOLO26 Object Detection
        ↓
Phát hiện người điều khiển / xe / mũ bảo hiểm
        ↓
Xác định trường hợp không đội mũ bảo hiểm
        ↓
Phát hiện biển số
        ↓
OCR biển số
        ↓
Hậu xử lý kết quả
        ↓
Thông tin vi phạm
        ↓
Ứng dụng WinForms
        ↓
SQL Server
```

Phần Machine Learning được phát triển trong thư mục `HelmetViolationML`,
trong khi giao diện quản lý và lưu trữ dữ liệu được phát triển riêng bằng
C# WinForms và SQL Server.

---

## Tài liệu tham khảo

- Ultralytics YOLO26:
  https://docs.ultralytics.com/models/yolo26/
- Ultralytics Documentation:
  https://docs.ultralytics.com/
- Ultralytics:
  https://www.ultralytics.com/