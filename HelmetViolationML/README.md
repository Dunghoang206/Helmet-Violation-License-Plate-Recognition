# Helmet Violation Detection with YOLO and OCR

Dự án nghiên cứu phát hiện người lái xe không đội mũ và trích xuất biển số xe bằng mô hình học sâu.

## 1. Mục tiêu dự án

Dự án này nhằm xây dựng hệ thống nhận diện vi phạm giao thông với các nhiệm vụ chính:

- Phát hiện người lái xe trong ảnh hoặc video.
- Chẩn đoán xem người đó có đội mũ bảo hiểm hay không.
- Phát hiện biển số xe trên phương tiện.
- Sử dụng OCR để đọc nội dung biển số.
- Xuất cảnh báo khi phát hiện vi phạm.
- Lưu kết quả hình ảnh/video và thông tin biển số để kiểm tra hoặc thống kê sau này.

---

## 2. Kiến trúc tổng quan của hệ thống

Hệ thống sẽ chạy theo pipeline sau:

1. Nhận đầu vào: ảnh hoặc video.
2. Dùng YOLO để phát hiện đối tượng như người, mũ bảo hiểm, biển số xe.
3. Xác định các trường hợp người lái xe không đội mũ.
4. Cắt vùng biển số xe và gửi sang OCR.
5. Trích xuất văn bản biển số.
6. Kết hợp kết quả phát hiện và OCR để đưa ra cảnh báo cuối cùng.

Cấu trúc của hệ thống gồm 3 phần chính:

- Detection: phát hiện đối tượng bằng YOLO
- OCR: đọc văn bản biển số
- Pipeline: kết hợp logic và xử lý đầu ra

---

## 3. Cấu trúc thư mục dự án

```text
HelmetViolationML/
├─ README.md
├─ .gitignore
├─ requirements.txt
├─ config/
│  ├─ yolo_helmet.yaml
│  ├─ yolo_plate.yaml
│  ├─ ocr_config.yaml
│  └─ inference_config.yaml
├─ data/
│  ├─ raw/
│  │  ├─ images/
│  │  ├─ videos/
│  │  └─ labels/
│  ├─ processed/
│  │  ├─ crops/
│  │  ├─ plates/
│  │  ├─ helmets/
│  │  └─ metadata/
│  ├─ splits/
│  │  ├─ train.txt
│  │  ├─ val.txt
│  │  └─ test.txt
│  └─ utils/
│     ├─ split_dataset.py
│     ├─ label_to_yolo.py
│     └─ export_annotations.py
├─ src/
│  ├─ __init__.py
│  ├─ data/
│  │  ├─ __init__.py
│  │  ├─ dataset.py
│  │  ├─ augmentation.py
│  │  └─ preprocessing.py
│  ├─ models/
│  │  ├─ __init__.py
│  │  ├─ yolov8_model.py
│  │  ├─ helmet_model.py
│  │  ├─ plate_model.py
│  │  └─ ocr_model.py
│  ├─ detection/
│  │  ├─ helmet_detector.py
│  │  ├─ plate_detector.py
│  │  └─ postprocess.py
│  ├─ ocr/
│  │  ├─ plate_recognition.py
│  │  ├─ text_preprocessing.py
│  │  └─ ocr_pipeline.py
│  ├─ pipeline/
│  │  ├─ image_pipeline.py
│  │  ├─ video_pipeline.py
│  │  ├─ tracking.py
│  │  └─ rule_engine.py
│  ├─ utils/
│  │  ├─ logger.py
│  │  ├─ metrics.py
│  │  ├─ visualize.py
│  │  └─ helpers.py
│  └─ app/
│     ├─ main.py
│     ├─ cli.py
│     └─ api.py
├─ notebooks/
│  ├─ 01_dataset_review.ipynb
│  ├─ 02_label_check.ipynb
│  ├─ 03_yolo_training.ipynb
│  └─ 04_ocr_experiments.ipynb
├─ weights/
│  ├─ helmet_yolo/
│  ├─ plate_yolo/
│  └─ ocr/
├─ scripts/
│  ├─ train_helmet.sh
│  ├─ train_plate.sh
│  ├─ evaluate.sh
│  └─ infer_video.sh
├─ tests/
│  ├─ test_dataset.py
│  ├─ test_pipeline.py
│  └─ test_ocr.py
├─ docs/
│  ├─ dataset.md
│  ├─ training.md
│  └─ deployment.md
├─ results/
│  ├─ prediction_images/
│  ├─ prediction_videos/
│  └─ reports/
└─ .gitignore
```

---

## 4. Hướng dẫn sử dụng

### Bước 1: Thiết lập môi trường

1. Tạo môi trường ảo Python.

```bash
python -m venv venv
```

2. Kích hoạt môi trường.

Windows:

```bash
venv\Scripts\activate
```

Linux/macOS:

```bash
source venv/bin/activate
```

3. Cài đặt thư viện cần thiết.

```bash
pip install -r requirements.txt
```

> Nếu dự án đang ở giai đoạn khởi đầu, bạn cần bổ sung các thư viện như `ultralytics`, `opencv-python`, `torch`, `numpy`, `pandas`, `matplotlib`, và OCR phù hợp như `easyocr` hoặc `paddleocr` khi bắt đầu triển khai thực tế.

---

### Bước 2: Chuẩn bị dữ liệu

Đặt dữ liệu vào các thư mục sau:

- `data/raw/images/`: ảnh mẫu
- `data/raw/videos/`: video mẫu
- `data/raw/labels/`: file annotation gắn nhãn
- `data/processed/crops/`: ảnh cắt sau khi phát hiện
- `data/processed/plates/`: ảnh biển số đã cắt
- `data/processed/helmets/`: ảnh vùng đầu / mũ bảo hiểm

Bạn nên chuẩn bị dataset theo định dạng YOLO:

- file hình ảnh: `.jpg`, `.png`
- file nhãn: `.txt`
- các nhãn nên theo các class như:
  - `person`
  - `helmet`
  - `no_helmet`
  - `license_plate`

---

### Bước 3: Chia dữ liệu train/val/test

Sử dụng thư mục `data/splits/` để lưu danh sách tập train, validation và test.

Ví dụ:

```text
data/splits/train.txt
data/splits/val.txt
data/splits/test.txt
```

Mỗi file chứa đường dẫn đến ảnh tương ứng.

---

### Bước 4: Huấn luyện mô hình YOLO

Dùng mô hình YOLO để phát hiện người, mũ bảo hiểm và biển số.

Các file cấu hình nên lưu trong thư mục `config/`:

- `yolo_helmet.yaml`
- `yolo_plate.yaml`
- `inference_config.yaml`

Cách chạy huấn luyện theo mô hình chuẩn:

```bash
python src/models/yolov8_model.py --config config/yolo_helmet.yaml
python src/models/yolov8_model.py --config config/yolo_plate.yaml
```

> Đây là hướng dẫn mô hình hóa, phù hợp với giai đoạn phát triển dự án. Bạn có thể thay đổi lệnh theo cách triển khai code thực tế của mình sau khi viết file huấn luyện.

---

### Bước 5: Chạy OCR biển số

Sau khi phát hiện biển số, vùng biển số được crop và xử lý bằng OCR.

Các bước thường thực hiện:

1. Cắt vùng biển số từ ảnh hoặc video.
2. Chuyển ảnh sang grayscale hoặc binary.
3. Làm sạch nhiễu và resize ảnh.
4. Dùng model OCR để đọc ký tự.
5. Chuẩn hóa kết quả biển số.

Thực hiện theo các file trong `src/ocr/` hoặc `src/pipeline/` khi đã triển khai code.

---

### Bước 6: Chạy suy luận trên ảnh/video

Sau khi có các model đã huấn luyện hoặc checkpoint đã lưu trong `weights/`, bạn chạy pipeline suy luận.

Ví dụ khái niệm:

```bash
python src/app/main.py --input data/raw/videos/test.mp4 --output results/prediction_videos
```

Một số kết quả mong muốn:

- Hiển thị box phát hiện người
- Hiển thị box mũ bảo hiểm
- Gắn cảnh báo: `Không đội mũ`
- Hiển thị box biển số và text nhận diện
- Xuất output hình ảnh/video mới với các label đã vẽ

---

## 5. Luồng xử lý nghiệp vụ

Hệ thống nên làm việc theo luồng sau:

1. Người lái xe xuất hiện trong frame.
2. YOLO phát hiện người và mũ bảo hiểm.
3. Nếu không phát hiện mũ trong vùng đầu người, gán trạng thái `no_helmet`.
4. Nếu phát hiện biển số xe, cắt vùng biển số.
5. OCR đọc văn bản trong biển số.
6. Xuất kết quả dạng:

```text
Tên sự kiện: Vi phạm không đội mũ
Biển số: 29A-12345
Thời gian: 2026-09-24 12:30:15
```

---

## 6. Cách đánh giá mô hình

Sau khi huấn luyện, cần đánh giá theo các chỉ số chính:

- Precision
- Recall
- F1-score
- mAP (mean Average Precision)
- Accuracy của OCR

Thống kê có thể lưu trong thư mục `results/reports/`.

---

## 7. Tài liệu và ghi chú nghiên cứu

Bạn nên lưu thêm các thông tin quan trọng sau:

- mô tả bộ dữ liệu
- số lượng hình ảnh và video
- tỷ lệ train/val/test
- các class gắn nhãn
- cấu hình YOLO
- cấu hình OCR
- lưu ý về độ phân giải ảnh
- lỗi thường gặp và cách khắc phục

---

## 8. Lưu ý khi phát triển dự án

- Luôn tách dữ liệu gốc và dữ liệu xử lý để tránh nhầm lẫn.
- Không lưu checkpoint và output vào cùng thư mục dữ liệu gốc.
- Nên kiểm tra nhãn trước khi huấn luyện để tránh dataset sai.
- Khi làm OCR, nên lọc nhiễu và kiểm tra bộ ký tự đầu vào.
- Với bộ dữ liệu thực tế, nên giữ cả ảnh gốc lẫn ảnh đã cắt để kiểm tra lại kết quả.

---

## 9. Kết luận

Dự án này là một hệ thống nhận diện vi phạm giao thông bằng học sâu, tập trung vào 2 nhiệm vụ chính:

- phát hiện người không đội mũ
- đọc biển số xe bằng OCR

Cấu trúc project hiện tại đang được xây dựng theo hướng phát triển khoa học và dễ mở rộng trong tương lai. Khi bạn bắt đầu viết code, bạn có thể triển khai từng module theo layout đã quy định trong thư mục `src/`.

---

## 10. Todo khuyến nghị

- Chuẩn bị dataset
- Gắn nhãn YOLO cho các lớp mục tiêu
- Chia tập train/val/test
- Huấn luyện model phát hiện helmet
- Huấn luyện model phát hiện license plate
- Kiểm tra OCR biển số
- Thiết kế pipeline tổng hợp
- Đánh giá và tối ưu độ chính xác

---

Dự án này đang ở trạng thái khung nghiên cứu và có thể được phát triển tiếp theo theo hướng mô hình sản phẩm thực tế.
