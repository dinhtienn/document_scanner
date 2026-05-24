# Scan Tài Liệu

Chủ đề nghiên cứu, thực nghiệm: Xây dựng chương trình xử lý ảnh phục vụ bài toán scan tài liệu trong môn Thị giác máy tính. Nội dung tập trung vào tiền xử lý ảnh, phát hiện biên, tìm contour, hiệu chỉnh phối cảnh, nhị phân hóa / enhancement, và đánh giá chất lượng scan thông qua OCR.

Nội dung báo cáo: Tổng kết quá trình làm việc, nghiên cứu, thực nghiệm và đánh giá các phương pháp xử lý ảnh trong môn Thị giác máy tính. Báo cáo trình bày cách tổ chức chương trình, các bước tiền xử lý dữ liệu ảnh, các kịch bản phát hiện biên, trích xuất vùng tài liệu, các phương pháp nhị phân hóa và giữ màu, đánh giá định lượng (IoU, Dice), so sánh OCR (Tesseract vs PaddleOCR) và đánh giá tinh thần, trách nhiệm, thái độ làm việc của các thành viên trong nhóm.

## 1. Thông tin nhóm

- Tên nhóm: Nhóm 9
- Môn học: Thị giác máy tính
- Tên dự án: Scan tài liệu
- Số lượng thành viên: 3
    - Đinh Quang Tiến - 23001943 - Nhóm trưởng
    - Hà Minh Quang - 23001916
    - Nguyễn Đức Quang - 23001918

## 2. Công việc của nhóm

- Đọc hiểu yêu cầu của đề bài Project 2 (Document Scanner) môn Thị giác máy tính.
- Thảo luận hướng giải quyết bài toán scan tài liệu và xử lý ảnh đầu vào.
- Thu thập, tổ chức dữ liệu ảnh (≥150 ảnh tự chụp) và xây dựng các hàm tiện ích xử lý ảnh.
- Thực hiện tiền xử lý ảnh: chuyển đổi không gian màu, làm mịn, chuẩn hóa.
- Thực nghiệm phát hiện biên và phát hiện 4 góc tài liệu (Edge-based vs Hough-based).
- Hiệu chỉnh phối cảnh (perspective transform) để tạo ảnh scan.
- Thực nghiệm nhị phân hóa (Otsu, Adaptive, CLAHE+Otsu) và scan giữ màu (color-preserving).
- Annotate ground truth 4 góc cho test set, đánh giá định lượng bằng IoU, Dice, Corner Error.
- Thực nghiệm OCR trên các pipeline khác nhau, so sánh Tesseract và PaddleOCR.
- So sánh, đối chiếu và thảo luận kết quả của các kịch bản thực nghiệm.
- Sử dụng kết quả thực nghiệm để hoàn thiện báo cáo, slide thuyết trình và mã nguồn chương trình.
- Tổng hợp nhận xét, hạn chế và hướng cải thiện cho các phương pháp đã triển khai.

## 3. Phân công công việc

**Đinh Quang Tiến - 23001943**
- Thực hiện lên kế hoạch, phân chia, điều phối công việc.
- Thu thập và chia dữ liệu.
- Thực nghiệm tiền xứ lý dữ liệu.
- Gán nhãn và đánh giá detection.
- Thực nghiệm và đánh giá Tesseract OCR.
- Làm báo cáo và slide.

**Hà Minh Quang - 23001916**
- Thu thập và chia dữ liệu.
- Thực nghiệm phát hiện Contour và quad.
- Thực nghiệm và đánh giá Paddle OCR.
- Làm báo cáo và slide.

**Nguyễn Đức Quang - 23001918**
- Thu thập và chia dữ liệu.
- Thực nghiệm Binarization.
- Thực nghiệm so sánh ảnh scan đầu ra grayscale và color-preserving.
- Làm báo cáo và slide.

## 4. Cách tổ chức chương trình

```
document_scanner/
├── readme.md                           
├── data/
│   └── readme.md                          # Mô tả nguồn dữ liệu và link Kaggle
├── docs/                                  # Báo cáo và slide
└── source/
    ├── 01_Preprocessing.ipynb             # Tiền xử lý dữ liệu
    ├── 02_detect_binarization.ipynb       # Phát hiện biên + nhị phân hóa
    ├── 03_grayscale_vs_color_scan.ipynb   # So sánh scan grayscale và color-preserving
    ├── 04_annotate_corners.ipynb          # Tool annotate 4 góc tài liệu cho test set
    ├── 05_eval_detection.ipynb            # Đánh giá định lượng phát hiện 4 góc
    ├── 06_ocr_annotate.ipynb              # Tool gõ ground truth text cho OCR
    ├── 07_ocr_eval_tesseract.ipynb        # Đánh giá OCR bằng Tesseract
    └── 08_ocr_eval_paddleocr.ipynb        # Đánh giá OCR bằng PaddleOCR
```

**Mô tả từng notebook**

| Notebook | Nội dung |
|---|---|
| `01_Preprocessing.ipynb` | Đọc ảnh, chuyển không gian màu, làm mịn, chuẩn hóa kích thước, so sánh các bộ lọc tiền xử lý. |
| `02_detect_binarization.ipynb` | Pipeline chính: edge detection, contour, tìm 4 góc, perspective transform; nhị phân hóa Otsu / Adaptive / CLAHE+Otsu. |
| `03_grayscale_vs_color_scan.ipynb` | So sánh ảnh scan đầu ra ở chế độ grayscale (đen trắng) và color-preserving (giữ màu, làm sạch nền). |
| `04_annotate_corners.ipynb` | Mini-tool click 4 góc tài liệu trên test set, lưu ground truth dạng JSON. |
| `05_eval_detection.ipynb` | Tính IoU, Dice, Corner Error cho 2 phương pháp phát hiện trên test set. |
| `06_ocr_annotate.ipynb` | Mini-tool gõ ground truth text (UTF-8) cho subset OCR (~15–20 ảnh). |
| `07_ocr_eval_tesseract.ipynb` | Chạy Tesseract trên 6 đầu vào (gốc / warped / Otsu / Adaptive / CLAHE / color-preserving), đo CER, WER, Char Acc, Word Acc. |
| `08_ocr_eval_paddleocr.ipynb` | Chạy PaddleOCR (deep learning baseline) trên cùng subset, so sánh với Tesseract. |

**Nguồn dữ liệu và mã nguồn**

- Dữ liệu sử dụng: bộ ảnh tài liệu tự chụp (≥150 ảnh) với đa dạng góc chụp, ánh sáng, nền, khoảng cách, tình trạng tài liệu. Mô tả chi tiết trong `data/readme.md`.
- Link dataset trên Kaggle: https://www.kaggle.com/datasets/tiendq/scanner
- Link source code tham khảo: https://github.com/dinhtienn/document_scanner_

## 5. Các kịch bản thực nghiệm

### Kịch bản 1: So sánh phương pháp phát hiện 4 góc tài liệu
- **Phương pháp:** Edge-based (Canny + largest contour + approxPolyDP) vs Hough-based (Hough lines + line intersection).
- **Dữ liệu:** test set (25–30 ảnh) đã annotate ground truth 4 góc.
- **Mục tiêu:** xác định pipeline phát hiện 4 góc tài liệu chính xác và ổn định hơn.
- **Đánh giá:** IoU, Dice, Corner Error .

### Kịch bản 2: So sánh các phương pháp nhị phân hóa
- **Phương pháp:** Otsu, Adaptive Threshold, CLAHE + Otsu.
- **Dữ liệu:** ảnh sau khi đã warp đúng phối cảnh.
- **Mục tiêu:** kiểm tra phương pháp nào cho ảnh scan rõ chữ, sạch nền nhất ở các điều kiện ánh sáng khác nhau.
- **Đánh giá:** độ rõ văn bản qua quan sát, độ chính xác OCR (Kịch bản 4).

### Kịch bản 3: Grayscale vs Color-preserving scan
- **Phương pháp:** Grayscale binarization (đen trắng) vs Color-preserving (chia ảnh cho ước lượng nền + cân bằng màu để giữ logo, hình minh họa).
- **Dữ liệu:** test set, đặc biệt các ảnh có hình ảnh, biểu đồ, logo màu.
- **Mục tiêu:** xác định khi nào nên dùng output đen trắng, khi nào nên giữ màu.
- **Đánh giá:** so sánh trực quan + độ chính xác OCR.

### Kịch bản 4: Đánh giá OCR (giá trị ứng dụng)
- **Phương pháp:** chạy OCR trên 6 đầu vào: ảnh gốc / warped grayscale / Otsu / Adaptive / CLAHE+Otsu / Color-preserving.
- **Engine:** Tesseract 5 và PaddleOCR (deep learning baseline).
- **Dữ liệu:** subset 15–20 ảnh từ test set có chữ rõ, đã gõ ground truth text UTF-8.
- **Mục tiêu:** chứng minh scan giúp OCR đọc tốt hơn ảnh gốc; xác định pipeline binarize tốt nhất cho OCR; so sánh classical (Tesseract) và deep learning (PaddleOCR).
- **Đánh giá:** CER, Character Accuracy, Word Accuracy.

## 6. Hướng dẫn chạy chương trình

### 6.1. Trình tự chạy notebook

1. `01_Preprocessing.ipynb` — tải dữ liệu, tiền xử lý, sinh ảnh trung gian.
2. `02_detect_binarization.ipynb` — pipeline scan chính + nhị phân hóa.
3. `03_grayscale_vs_color_scan.ipynb` — so sánh grayscale và color-preserving.
4. `04_annotate_corners.ipynb` — annotate 4 góc cho test set (chỉ cần chạy 1 lần).
5. `05_eval_detection.ipynb` — đánh giá định lượng pipeline phát hiện.
6. `06_ocr_annotate.ipynb` — gõ ground truth text cho subset OCR (chỉ cần chạy 1 lần).
7. `07_ocr_eval_tesseract.ipynb` — đánh giá OCR Tesseract.
8. `08_ocr_eval_paddleocr.ipynb` — đánh giá OCR PaddleOCR và so sánh.

Notebook 04 và 06 sinh annotation thủ công, kết quả đã được commit nên các bước đánh giá (05, 07, 08) có thể chạy trực tiếp mà không cần annotate lại.

### 6.2. Chạy trên Google Colab (khuyến nghị)

Mở từng notebook trên Colab và bấm **Runtime → Run all**. Mọi thư viện cần thiết đã được cài sẵn trên Colab hoặc được cài tự động trong cell đầu tiên của notebook; dữ liệu được tự động tải về qua `kagglehub`.

Lưu ý nhỏ trên Colab:
- Lần đầu dùng `kagglehub` cần đăng nhập Kaggle (notebook sẽ hiện hướng dẫn).
- Notebook 07 (Tesseract) có cell `apt-get install tesseract-ocr tesseract-ocr-vie` ở đầu, cứ chạy bình thường.
- Một số notebook dùng `google.colab.files` / `google.colab.output` — chỉ hoạt động trên Colab.

### 6.3. Chạy trên máy local

Cần chuẩn bị thêm so với Colab:

**1. Cài thư viện Python**

```
pip install -r requirements.txt
```

Khuyến nghị Python ≥ 3.10 và dùng virtual environment (`venv` hoặc `conda`).

**2. Cài Tesseract engine** (cho notebook 07)

- Windows: tải installer từ https://github.com/UB-Mannheim/tesseract/wiki, cài kèm tùy chọn Vietnamese language data.
- Nếu thiếu tiếng Việt: tải `vie.traineddata` từ https://github.com/tesseract-ocr/tessdata_best và copy vào `C:\Program Files\Tesseract-OCR\tessdata\`.
- Verify: chạy `tesseract --list-langs` trong cmd, phải thấy `vie` và `eng`.

**3. Lưu ý đường dẫn dữ liệu**

Các notebook được viết để chạy chính trên Colab nên có một số đường dẫn cần điều chỉnh khi chạy local:

- Đường dẫn dataset trỏ tới `/content/...` hoặc Google Drive (`/content/drive/MyDrive/...`) cần đổi sang đường dẫn local. Cách đơn giản nhất là gọi `kagglehub.dataset_download("tiendq/scanner")` rồi dùng path trả về.
- Các cell dùng `google.colab.files.upload()`, `google.colab.files.download()`, `google.colab.output.eval_js(...)` không chạy được local — bỏ qua hoặc thay bằng đọc/ghi file thẳng từ ổ đĩa.
- Tool annotate trong notebook 04 và 06 dùng JS bridge của Colab; trên local nên dùng phiên bản annotation đã commit sẵn thay vì chạy lại.
- Output trung gian (ảnh warped, kết quả CSV...) mặc định ghi vào `/content/...`; đổi thành thư mục local (ví dụ `./outputs/`).

**4. Tải dữ liệu**

Dataset được tự động tải qua `kagglehub` trong notebook 01. Nếu muốn tải thủ công: https://www.kaggle.com/datasets/tiendq/scanner — sau đó cập nhật đường dẫn trong notebook cho khớp.

## 7. Kết quả chính

- Pipeline Edge-based ổn định hơn Hough-based ở điều kiện nền có nhiễu nhẹ; Hough-based tốt hơn ở ảnh có cạnh thẳng rõ.
- Adaptive Threshold cho OCR tốt nhất ở ảnh có bóng đổ; Otsu thắng ở điều kiện ánh sáng đều.
- Color-preserving không cải thiện OCR text thuần nhưng giữ được logo, hình minh họa cho người đọc.
- Scan giảm CER trung bình ~30–50% so với OCR trên ảnh gốc.
- PaddleOCR cho kết quả tốt hơn Tesseract trên ảnh khó (chữ tay).

Số liệu chi tiết, bảng so sánh và phân tích fail case có trong báo cáo + slide.
