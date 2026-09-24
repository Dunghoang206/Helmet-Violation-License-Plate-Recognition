# NCKH - Helmet Violation Detection System

Dự án nghiên cứu khoa học về hệ thống phát hiện vi phạm giao thông liên quan đến mũ bảo hiểm và biển số xe. Project gồm hai phần chính:

- `HelmetViolationSystem/`: ứng dụng desktop WinForms trên .NET.
- `HelmetViolationML/`: phần AI để phát hiện người không đội mũ và nhận dạng biển số bằng OCR.

---

## 1. Mục tiêu dự án

- Phát hiện người đi xe máy trong ảnh/video.
- Kiểm tra xem người đó có đội mũ bảo hiểm hay không.
- Phát hiện biển số xe.
- Đọc nội dung biển số bằng OCR.
- Ghi nhận vi phạm và hỗ trợ giám sát giao thông tự động.

---

## 2. Công nghệ sử dụng

- C# / WinForms cho phần desktop.
- Python cho phần AI và xử lý dữ liệu.
- YOLO để phát hiện đối tượng.
- OpenCV để xử lý ảnh/video.
- PyTorch / Ultralytics cho huấn luyện và inference.

---

## 3. Cấu trúc thư mục

```text
NCKH/
├── README.md
├── .gitignore
├── HelmetViolationSystem/
│   └── HelmetViolationSystem/
│       ├── Program.cs
│       ├── Forms/
│       ├── Models/
│       ├── Services/
│       ├── Helpers/
│       └── Properties/
│
├── HelmetViolationML/
│   ├── requirements.txt
│   ├── config/
│   ├── data/
│   ├── src/
│   ├── scripts/
│   ├── weights/
│   ├── notebooks/
│   ├── results/
│   ├── tests/
│   └── docs/
│
└── .gitignore
```

---

## 4. Ý nghĩa của từng thư mục

### `HelmetViolationSystem/`
Chứa ứng dụng desktop WinForms. Đây là phần giao diện và xử lý nghiệp vụ chính của hệ thống.

### `HelmetViolationML/`
Chứa toàn bộ phần học máy và AI:

- `config/`: file cấu hình cho mô hình và suy luận.
- `data/`: dữ liệu gốc, dữ liệu đã xử lý và tập train/val/test.
- `src/`: mã nguồn chính gồm tiền xử lý, phát hiện, OCR và pipeline.
- `scripts/`: script huấn luyện, đánh giá và chạy inference.
- `weights/`: trọng số mô hình sau khi train.
- `notebooks/`: notebook thử nghiệm và phân tích dữ liệu.
- `results/`: ảnh/video, báo cáo và kết quả đầu ra.
- `tests/`: kiểm thử các module quan trọng.
- `docs/`: tài liệu mô tả, hướng dẫn và ghi chú.

---

## 5. Quy trình làm việc của dự án

1. Nhận đầu vào là ảnh hoặc video.
2. Dùng YOLO để phát hiện người, mũ bảo hiểm và biển số xe.
3. Kiểm tra xem người đó có đội mũ hay không.
4. Cắt vùng biển số và gửi vào OCR.
5. Đọc ký tự trên biển số.
6. Xuất cảnh báo và lưu kết quả.

---

## 6. Hướng dẫn sơ bộ

### 6.1 Môi trường Python

Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

Linux/macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 6.2 Cài đặt dependency

```bash
pip install -r HelmetViolationML/requirements.txt
```

### 6.3 Chuẩn bị dữ liệu

Đặt dữ liệu vào thư mục `HelmetViolationML/data/` theo cấu trúc phù hợp:

- `data/raw/images/`
- `data/raw/videos/`
- `data/processed/crops/`
- `data/processed/plates/`
- `data/processed/helmets/`
- `data/splits/`

---

## 7. Lưu ý

- Project đang ở giai đoạn phát triển và nghiên cứu.
- Cấu trúc thư mục có thể cập nhật khi mở rộng thêm tính năng.
- Không nên commit các file tạm, cache và kết quả huấn luyện.

---

## 8. Tổng kết

Project này kết hợp giữa phần desktop và phần AI để giải quyết bài toán phát hiện vi phạm mũ bảo hiểm và nhận dạng biển số xe trong môi trường giao thông thực tế.
