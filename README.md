HelmetViolationML/
├── README.md
├── requirements.txt
├── .gitignore
│
├── config/
│   ├── helmet.yaml
│   ├── plate.yaml
│   └── inference.yaml
│
├── data/
│   ├── raw/
│   │   ├── images/
│   │   └── videos/
│   │
│   ├── processed/
│   │   ├── crops/
│   │   ├── plates/
│   │   ├── helmets/
│   │   └── metadata/
│   │
│   └── splits/
│       ├── train.txt
│       ├── val.txt
│       └── test.txt
│
├── src/
│   ├── __init__.py
│   │
│   ├── data/
│   │   ├── __init__.py
│   │   ├── preprocessing.py
│   │   └── dataset.py
│   │
│   ├── detection/
│   │   ├── __init__.py
│   │   ├── helmet_detector.py
│   │   └── plate_detector.py
│   │
│   ├── ocr/
│   │   ├── __init__.py
│   │   ├── plate_recognition.py
│   │   └── preprocessing.py
│   │
│   ├── pipeline/
│   │   ├── __init__.py
│   │   ├── image_pipeline.py
│   │   └── video_pipeline.py
│   │
│   └── utils/
│       ├── __init__.py
│       ├── metrics.py
│       └── visualize.py
│
├── scripts/
│   ├── train_helmet.py
│   ├── train_plate.py
│   ├── evaluate.py
│   └── infer_video.py
│
├── weights/
│   ├── helmet/
│   └── plate/
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_training_yolo.ipynb
│   └── 03_ocr_analysis.ipynb
│
├── results/
│   ├── images/
│   ├── videos/
│   └── reports/
│
├── tests/
│   ├── test_dataset.py
│   ├── test_pipeline.py
│   └── test_ocr.py
│
└── docs/
    ├── dataset.md
    ├── training.md
    └── deployment.md# Helmet-Violation-License-Plate-Recognition

Deep learning-based system for detecting motorcycle riders without helmets and recognizing license plates from traffic videos.

## 1. Mục tiêu dự án

Dự án này nhằm xây dựng hệ thống giám sát giao thông tự động để:

- phát hiện người đi xe máy trong ảnh/video;
- xác định xem người đó có đội mũ bảo hiểm hay không;
- phát hiện biển số xe;
- đọc nội dung biển số bằng OCR;
- cảnh báo vi phạm nếu người lái xe không đội mũ;
- lưu lại kết quả hình ảnh, video và thông tin biển số để kiểm tra sau này.

---

## 2. Công nghệ dự kiến sử dụng

- YOLO: phát hiện đối tượng như người, mũ bảo hiểm, biển số xe.
- OCR: đọc ký tự trên biển số.
- OpenCV: xử lý ảnh/video, cắt vùng, tiền xử lý hình ảnh.
- Python: triển khai pipeline xử lý dữ liệu và mô hình.
- PyTorch / Ultralytics: huấn luyện và suy luận YOLO.

---

## 3. Cấu trúc thư mục dự án

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
│   ├── README.md
│   ├── requirements.txt
│   ├── config/
│   │   ├── helmet.yaml
│   │   ├── plate.yaml
│   │   └── inference.yaml
│   │
│   ├── data/
│   │   ├── raw/
│   │   │   ├── images/
│   │   │   └── videos/
│   │   ├── processed/
│   │   │   ├── crops/
│   │   │   ├── plates/
│   │   │   ├── helmets/
│   │   │   └── metadata/
│   │   └── splits/
│   │       ├── train.txt
│   │       ├── val.txt
│   │       └── test.txt
│   │
│   ├── src/
│   │   ├── __init__.py
│   │   ├── data/
│   │   │   ├── __init__.py
│   │   │   ├── preprocessing.py
│   │   │   └── dataset.py
│   │   ├── detection/
│   │   │   ├── __init__.py
│   │   │   ├── helmet_detector.py
│   │   │   └── plate_detector.py
│   │   ├── ocr/
│   │   │   ├── __init__.py
│   │   │   ├── plate_recognition.py
│   │   │   └── preprocessing.py
│   │   ├── pipeline/
│   │   │   ├── __init__.py
│   │   │   ├── image_pipeline.py
│   │   │   └── video_pipeline.py
│   │   └── utils/
│   │       ├── __init__.py
│   │       ├── metrics.py
│   │       └── visualize.py
│   │
│   ├── scripts/
│   │   ├── train_helmet.py
│   │   ├── train_plate.py
│   │   ├── evaluate.py
│   │   └── infer_video.py
│   │
│   ├── weights/
│   │   ├── helmet/
│   │   └── plate/
│   │
│   ├── notebooks/
│   ├── results/
│   │   ├── images/
│   │   ├── videos/
│   │   └── reports/
│   ├── tests/
│   └── docs/
│       ├── dataset.md
│       ├── training.md
│       └── deployment.md
│
└── .gitignore
```

---

## 4. Ý nghĩa của từng thư mục trong cấu trúc

### 4.1 `config/`
Chứa các file cấu hình cho mô hình và quá trình chạy. Ví dụ:
- `helmet.yaml`: cấu hình cho mô hình phát hiện mũ bảo hiểm
- `plate.yaml`: cấu hình cho mô hình phát hiện biển số
- `inference.yaml`: cấu hình khi chạy suy luận trên ảnh/video

### 4.2 `data/`
Lưu trữ dữ liệu của dự án, chia thành 3 nhóm chính:
- `raw/`: dữ liệu gốc chưa xử lý
- `processed/`: dữ liệu đã cắt, xử lý, crop và chuẩn bị để đưa vào mô hình
- `splits/`: tập train, validation và test

Trong đó:
- `images/`: ảnh đầu vào
- `videos/`: video đầu vào
- `crops/`: ảnh cắt từ đối tượng phát hiện
- `plates/`: ảnh biển số đã cắt
- `helmets/`: ảnh mũ bảo hiểm hoặc vùng đầu người
- `metadata/`: lưu thông tin bổ sung, nhãn, log hoặc dữ liệu phụ trợ

### 4.3 `src/`
Nơi chứa toàn bộ source code của dự án. Mỗi thư mục có vai trò riêng:
- `data/`: đọc dữ liệu, tiền xử lý và dataset loader
- `detection/`: phát hiện người, mũ bảo hiểm, biển số xe bằng YOLO
- `ocr/`: xử lý OCR để nhận diện biển số
- `pipeline/`: kết hợp nhiều bước thành một quy trình tổng thể
- `utils/`: các hàm phụ trợ như đánh giá, visualize, metrics

### 4.4 `scripts/`
Chứa các script để chạy huấn luyện, đánh giá và suy luận. Đây là nơi bạn dùng để thực thi các nhiệm vụ chính của dự án, ví dụ:
- `train_helmet.py`: huấn luyện mô hình phát hiện mũ
- `train_plate.py`: huấn luyện mô hình phát hiện biển số
- `evaluate.py`: đánh giá mô hình
- `infer_video.py`: chạy dự đoán trên video

### 4.5 `weights/`
Lưu trọng số mô hình sau khi huấn luyện. Thường chia theo từng mô hình:
- `helmet/`: mô hình phát hiện mũ bảo hiểm
- `plate/`: mô hình phát hiện biển số

### 4.6 `notebooks/`
Dùng để thử nghiệm, phân tích dữ liệu, trực quan hóa kết quả và kiểm tra từng bước trong nghiên cứu.

### 4.7 `results/`
Lưu kết quả đầu ra của dự án như:
- `images/`: ảnh kết quả đã đánh dấu box
- `videos/`: video kết quả đã xử lý
- `reports/`: báo cáo, thống kê, bảng so sánh đánh giá

### 4.8 `tests/`
Dùng để kiểm thử các thành phần quan trọng của project như dataset, pipeline và OCR.

### 4.9 `docs/`
Lưu tài liệu mô tả bộ dữ liệu, cách huấn luyện, triển khai và vận hành hệ thống.

### 4.10 `README.md`
Tài liệu mô tả tổng quan dự án, mục tiêu, cấu trúc, hướng dẫn cài đặt và cách sử dụng.

### 4.11 `.gitignore`
Giúp bỏ qua các file không cần đưa lên Git như:
- thư mục ảo hóa môi trường (`venv/`)
- file cache Python (`__pycache__/`)
- file nhạy cảm, file tạm, output training

---

## 5. Quy trình làm việc của dự án

Dự án này sẽ chạy theo một pipeline rõ ràng như sau:

1. Nhận đầu vào là ảnh hoặc video.
2. Dùng YOLO để phát hiện xe máy, người, mũ bảo hiểm và biển số.
3. Kiểm tra nếu có người không đội mũ.
4. Cắt vùng biển số và gửi sang OCR.
5. Đọc văn bản trên biển số.
6. Xuất cảnh báo và lưu kết quả.

---

## 5. Hướng dẫn cài đặt môi trường

### 5.1 Tạo môi trường ảo Python

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

### 5.2 Cài đặt các thư viện cần thiết

```bash
pip install -r HelmetViolationML/requirements.txt
```

Các thư viện thường dùng gồm:

- ultralytics
- opencv-python
- torch
- torchvision
- numpy
- pandas
- matplotlib
- easyocr hoặc paddleocr

---

## 6. Chuẩn bị dữ liệu

Bạn cần đặt dữ liệu vào thư mục `HelmetViolationML/data/` theo cấu trúc:

- `data/raw/images/`: ảnh gốc
- `data/raw/videos/`: video gốc
- `data/processed/crops/`: vùng ảnh cắt ra sau phát hiện
- `data/processed/plates/`: ảnh biển số đã cắt
- `data/processed/helmets/`: ảnh đầu người / vùng mũ bảo hiểm
- `data/splits/`: file chia tập train/val/test

Bạn nên chuẩn hóa dữ liệu trước khi huấn luyện:

- kiểm tra hình ảnh có bị thiếu không;
- kiểm tra nhãn có đúng class không;
- đảm bảo mỗi ảnh có annotation tương ứng;
- tạo file train/val/test rõ ràng.

---

## 7. Huấn luyện mô hình YOLO

Mục tiêu của YOLO ở đây là phát hiện:

- người lái xe;
- mũ bảo hiểm;
- biển số xe.

Thực hiện huấn luyện bằng cách chạy script trong thư mục `HelmetViolationML/scripts/`.

Ví dụ:

```bash
python HelmetViolationML/scripts/train_helmet.py
python HelmetViolationML/scripts/train_plate.py
```

Mô hình sau khi huấn luyện sẽ được lưu trong:

- `HelmetViolationML/weights/helmet/`
- `HelmetViolationML/weights/plate/`

---

## 8. Chạy OCR biển số

Sau khi có box biển số, tiến hành:

1. cắt vùng biển số từ ảnh/video;
2. tiền xử lý ảnh (grayscale, threshold, resize, làm mờ nhiễu nếu cần);
3. chạy OCR để trích xuất ký tự;
4. chuẩn hóa kết quả biển số.

Thường phần logic OCR sẽ nằm trong:

- `src/ocr/plate_recognition.py`
- `src/ocr/preprocessing.py`

---

## 9. Chạy suy luận trên ảnh hoặc video

Sau khi có checkpoint, bạn có thể chạy pipeline dự đoán trên bài toán của mình.

Ví dụ minh họa:

```bash
python HelmetViolationML/scripts/infer_video.py --input HelmetViolationML/data/raw/videos/video1.mp4 --output HelmetViolationML/results/videos
```

Kết quả đầu ra có thể bao gồm:

- ảnh/video đã vẽ box phát hiện;
- cảnh báo “không đội mũ”;
- nội dung biển số OCR được đọc;
- báo cáo thống kê trong `results/reports/`.

---

## 10. Đánh giá hiệu suất

Sau mỗi vòng huấn luyện, bạn nên đánh giá dựa trên các chỉ số:

- Precision
- Recall
- F1-score
- mAP
- Accuracy của OCR

Kết quả đánh giá có thể lưu trong `HelmetViolationML/results/reports/`.

---

## 11. Mức độ hoàn thành của project

Project hiện tại đang ở giai đoạn:

- thiết kế cấu trúc dự án
- chuẩn bị môi trường và dữ liệu
- xây dựng pipeline AI
- chưa hoàn thiện code mô hình và quy trình chạy chi tiết

Nói cách khác, đây là project đang trong quá trình phát triển theo hướng nghiên cứu và triển khai thực tế.

---

## 12. Gợi ý phát triển tiếp

Bạn nên làm theo thứ tự sau:

1. Chuẩn bị dữ liệu và nhãn.
2. Xây dựng dataset loader.
3. Huấn luyện mô hình phát hiện mũ bảo hiểm.
4. Huấn luyện mô hình phát hiện biển số.
5. Tạo module OCR.
6. Kết hợp pipeline hoàn chỉnh.
7. Thử nghiệm trên video thực tế.
8. Đánh giá và cải tiến độ chính xác.

---

## 13. Kết luận

Dự án này kết hợp hai phần quan trọng:

- phát hiện vi phạm không đội mũ bằng YOLO;
- nhận dạng biển số bằng OCR.

Đây là hướng tiếp cận phù hợp cho hệ thống giám sát giao thông tự động, có thể mở rộng sang giám sát thực tế trên camera đường bộ hoặc các khu vực kiểm soát giao thông.

---

## 14. Lưu ý

- Không lưu dữ liệu thô và dữ liệu đã xử lý lẫn nhau.
- Luôn kiểm tra nhãn trước khi huấn luyện.
- Nên giữ lại ảnh gốc để đối chiếu khi mô hình sai.
- Nên có báo cáo rõ ràng cho mỗi vòng training và evaluation.
