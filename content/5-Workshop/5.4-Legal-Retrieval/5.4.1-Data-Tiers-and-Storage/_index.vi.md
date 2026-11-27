---
title: "Các tầng dữ liệu và đơn vị lưu trữ"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

#### Ba tầng dữ liệu

| Tầng | Nơi lưu | Nội dung |
| --- | --- | --- |
| Catalog (5 bảng theo bản nghiên cứu) | `backend/sca/data/legal/` trong repo | 114 văn bản, mỗi văn bản 15 trường metadata; 13 khu vực pháp lý; 10 vụ việc; bộ kiểm soát tuân thủ (khung điều khoản C01–C20, thẩm định đối tác, thang R = P × I); 25 nguồn chính thức |
| Kho văn bản | Bucket S3 `legal-corpus` | `sources/` bản gốc tải về, không sửa; `staging/` các điều chờ duyệt; `laws/` các điều đã duyệt, prefix duy nhất Knowledge Base đọc |
| Chỉ mục vector | Amazon Bedrock Knowledge Base trên Amazon S3 Vectors | Vector 1.024 chiều từ Cohere Embed Multilingual v3, kèm metadata dùng để lọc |

#### Một điều luật, một tệp

Mỗi điều đã duyệt là một tệp `laws/{khu vực}/{văn bản}/art-{số điều}.md` kèm `.metadata.json`. Đơn vị lưu trữ trùng với đơn vị trích dẫn, nên một kết quả truy xuất luôn mang đúng mã văn bản và số điều, và bước kiểm chứng chỉ cần so hai giá trị này.

```json
{
  "metadataAttributes": {
    "country": "VN",
    "law_id": "ltm-2005",
    "article": "301",
    "title": "Luật Thương mại",
    "doc_type": "law",
    "binding": "binding",
    "sectors": ["all"],
    "effective_from": "2006-01-01",
    "status": "in_force",
    "source_url": "<link văn bản gốc>",
    "lang": "vi"
  }
}
```

- `sectors` chứa mã ngành theo Hệ thống ngành kinh tế Việt Nam (Quyết định 36/2025/QĐ-TTg) hoặc `all`.
- `status` cho biết văn bản còn hiệu lực hay không.
- `law_id` và `article` là cặp giá trị mọi trích dẫn phải khớp.

#### Mức kiểm chứng trong catalog

Mỗi văn bản trong catalog có một trong ba mức kiểm chứng:

| Mức | Ý nghĩa |
| --- | --- |
| `pending` | Lấy từ bản nghiên cứu, chưa đối chiếu |
| `desk_checked` | Số hiệu hoặc ngày hiệu lực đã đối chiếu với nguồn công khai |
| `verified` | Pháp chế đã đọc bản chính thức, kèm tên và ngày |

Chỉ người duyệt được đặt mức `verified`.
