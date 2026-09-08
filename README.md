<div align="center">

# MSHF — Multi-Scale Hybrid Fusion

**Phát hiện và định vị giả mạo tài liệu PDF tiếng Việt**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![GitHub Release](https://img.shields.io/github/v/release/Susois/mshf)](https://github.com/Susois/mshf/releases/latest)

[English](README_EN.md)

</div>

---

## Giới thiệu

MSHF là hệ thống phát hiện giả mạo tài liệu PDF tiếng Việt sử dụng kiến trúc **Multi-Scale Hybrid Fusion** — kết hợp 3 nhánh đặc trưng độc lập:

- **Nhánh A** — Semantic: CER/WER + PhoBERT + LayoutLMv3 *(8 features)*
- **Nhánh B1** — Line-level: đếm insert/delete/modify theo từng dòng *(16 features)*
- **Nhánh C** — Geometric: delta font/spacing/margin + residual *(16 features)*

Tất cả hội tụ vào **XGBoost** phân loại 5 nhãn: `original` · `insert` · `delete` · `modify` · `layout`

Ngoài phân loại, hệ thống còn **định vị từng dòng bị can thiệp** trên từng trang và xuất báo cáo HTML trực quan.

---

## Hệ thống làm được gì?

MSHF nhận một tài liệu PDF tham chiếu và một tài liệu cần kiểm tra, sau đó:

- So sánh nội dung và bố cục giữa hai tài liệu.
- Phân loại tài liệu thành `original`, `insert`, `delete`, `modify` hoặc `layout`.
- Phân biệt các dòng bị chèn, xóa hoặc sửa và xác định vị trí của chúng theo từng trang.
- Kết hợp tín hiệu ngữ nghĩa, cấp dòng và hình học để giảm phụ thuộc vào một loại đặc trưng duy nhất.
- Xuất kết quả dự đoán, độ tin cậy và báo cáo HTML có đánh dấu trực quan các vị trí nghi vấn.

Hệ thống hướng tới các tài liệu pháp luật tiếng Việt, nơi thay đổi nhỏ về câu chữ, dấu câu, khoảng cách hoặc vị trí trình bày đều có thể mang ý nghĩa quan trọng.

## Điểm chính của repo

Repo hiện thực đầy đủ quy trình từ trích xuất dữ liệu PDF, căn chỉnh các dòng tương ứng, tạo đặc trưng, huấn luyện mô hình, đánh giá và định vị giả mạo. Ngoài mô hình MSHF, repo còn có các công cụ phục vụ xây dựng dataset, kiểm tra chất lượng dữ liệu, đánh giá độ bền và phân tích trên các tập tài liệu khác nhau.

Ba nhánh đặc trưng được thiết kế để bổ trợ cho nhau:

- **Semantic** phát hiện khác biệt về nội dung và ngữ nghĩa.
- **Line-level** mô hình hóa các thao tác chèn, xóa và sửa ở cấp dòng.
- **Geometric** phát hiện bất thường về font, khoảng cách, lề và vị trí trình bày.

---

## Kết quả

| Task | Cấu hình | Macro F1 | Balanced Acc |
|---|---|---|---|
| Binary (tampered/authentic) | A+B1+C / single | **98.86%** | 99.41% |
| 5-class | A+B1+C / stacking | **98.52%** | 98.52% |

Đánh giá trên bộ dữ liệu **VEDTD** (1.490 tài liệu pháp luật, 5-fold source-disjoint cross-validation).

---

## License

Dự án này được phân phối theo giấy phép [MIT](LICENSE).  
Xem [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md) để biết thông tin về các thư viện bên thứ ba.
