---
title: "Truy vấn lúc phân tích: công cụ search_law"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

#### Một lượt tra luật diễn ra thế nào

1. Agent tuân thủ hoặc agent tranh chấp gọi `search_law` với một câu truy vấn, tối đa 3 lượt mỗi agent.
2. Lambda gọi Bedrock `Retrieve` qua VPC endpoint: lấy 10 kết quả gần nhất, lọc ngay trong truy vấn theo `country = VN` và `status = in_force`.
3. Kết quả được lọc tiếp theo mã ngành của hợp đồng: giữ điều có `sectors` chứa mã đó hoặc `all`.
4. Agent nhận 5 đoạn đầu, mỗi đoạn tối đa 700 ký tự kèm `law_id`, số điều và tên văn bản, và chỉ được trích theo cặp `law_id` + số điều.

Request rút gọn gửi tới Knowledge Base:

```json
{
  "knowledgeBaseId": "<KB_ID>",
  "retrievalQuery": { "text": "mức phạt vi phạm hợp đồng thương mại tối đa" },
  "retrievalConfiguration": {
    "vectorSearchConfiguration": {
      "numberOfResults": 10,
      "filter": {
        "andAll": [
          { "equals": { "key": "country", "value": "VN" } },
          { "equals": { "key": "status", "value": "in_force" } }
        ]
      }
    }
  }
}
```

#### Các luật agent đang trích

Agent hiện chỉ trích 8 luật Việt Nam dưới đây; các văn bản quốc tế và nước ngoài trong catalog chỉ được truy xuất khi có yêu cầu rõ (hôm nay là bộ câu hỏi chuẩn).

| `law_id` | Văn bản | Số hiệu | Hiệu lực từ | Số điều trong kho |
| --- | --- | --- | --- | --- |
| `ltm-2005` | Luật Thương mại | 36/2005/QH11 | 01/01/2006 | 324 |
| `blds-2015` | Bộ luật Dân sự | 91/2015/QH13 | 01/01/2017 | 689 |
| `blttds-2015` | Bộ luật Tố tụng dân sự | 92/2015/QH13 | 01/07/2016 | 517 |
| `tttm-2010` | Luật Trọng tài thương mại | 54/2010/QH12 | 01/01/2011 | 82 |
| `shtt-2005` | Luật Sở hữu trí tuệ (văn bản hợp nhất 2026) | 50/2005/QH11 | 01/07/2006 | 240 |
| `bvqlntd-2023` | Luật Bảo vệ quyền lợi người tiêu dùng | 19/2023/QH15 | 01/07/2024 | 80 |
| `gddt-2023` | Luật Giao dịch điện tử | 20/2023/QH15 | 01/07/2024 | 53 |
| `bvdlcn-2025` | Luật Bảo vệ dữ liệu cá nhân | 91/2025/QH15 | 01/01/2026 | 39 |

Tổng cộng **2.024 điều**, dựng ngày 25/11/2026.

{{% notice note %}}
vbpl.vn ghi Luật Thương mại, Bộ luật Tố tụng dân sự và Luật Giao dịch điện tử là "hết hiệu lực một phần". Người duyệt cần đối chiếu trạng thái từng điều trước khi đặt mức `verified`.
{{% /notice %}}
