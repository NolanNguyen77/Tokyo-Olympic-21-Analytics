# 🏅 Olympic Tokyo Data Analytics with Azure

## 📖 Giới thiệu

Dự án **Olympic Tokyo** tập trung vào phân tích dữ liệu Thế vận hội Olympic Tokyo, bao gồm thông tin về vận động viên, huấn luyện viên, đội tuyển, huy chương... Dữ liệu gốc được lưu dưới dạng các file CSV trong thư mục `data/`.

Dự án hướng dẫn cách:
- Lưu trữ dữ liệu trên Azure Blob Storage
- Đọc và phân tích dữ liệu bằng Python & pandas
- Ứng dụng các dịch vụ Azure cho phân tích dữ liệu

---

## 📂 Cấu trúc thư mục
``` bash
OlympicTokyo/
├── data/
│ ├── Athletes.csv
│ ├── Coaches.csv
│ ├── EntriesGender.csv
│ ├── Medals.csv
│ └── Teams.csv
└── README.md

```

---

## ☁️ Yêu cầu & Chuẩn bị Azure

- Tài khoản Azure Portal: https://portal.azure.com/
- Tạo **Azure Storage Account** và **Blob Container** (ví dụ: `olympic-data`)
- Lấy **Connection String** từ Azure Storage

---

## 🛠️ Cài đặt thư viện

```bash
pip install azure-storage-blob pandas
```

---

## 🚀 Hướng dẫn sử dụng

### 1. Cấu hình Azure

Tạo file `azure_config.py`:

```python
AZURE_STORAGE_CONNECTION_STRING = "DefaultEndpointsProtocol=https;AccountName=YOUR_ACCOUNT_NAME;AccountKey=YOUR_ACCOUNT_KEY;EndpointSuffix=core.windows.net"
CONTAINER_NAME = "olympic-data"
```

### 2. Upload dữ liệu lên Azure Blob Storage

Tạo file `upload_to_azure.py`:

```python
import os
from azure.storage.blob import BlobServiceClient
from azure_config import AZURE_STORAGE_CONNECTION_STRING, CONTAINER_NAME
from io import StringIO

def read_csv_from_blob(blob_name):
    blob_service_client = BlobServiceClient.from_connection_string(AZURE_STORAGE_CONNECTION_STRING)
    blob_client = blob_service_client.get_blob_client(container=CONTAINER_NAME, blob=blob_name)
    blob_data = blob_client.download_blob().readall()
    return pd.read_csv(StringIO(blob_data.decode('utf-8')))

if __name__ == "__main__":
    df = read_csv_from_blob("Athletes.csv")
    print("Số lượng vận động viên:", len(df))
    print("Các quốc gia tham dự:", df['NOC'].unique())
    print(df.head())
```
Chạy lệnh:
```bash
python analyze_from_azure.py
```

---

## 📌 Ghi chú

- Không public Connection String lên GitHub.
- Có thể mở rộng phân tích với các file CSV khác trong thư mục `data/`.
- Nếu muốn dùng Jupyter Notebook, bạn có thể copy các đoạn code trên vào cell notebook.

---

**© 2025 Olympic Tokyo Data Analytics Project**
