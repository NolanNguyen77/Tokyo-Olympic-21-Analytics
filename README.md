# 🏅 Olympic Tokyo Data Analytics with Azure

## 📑 Table of Contents

1. [📖 Giới thiệu](#-giới-thiệu)
2. [📂 Cấu trúc thư mục](#-cấu-trúc-thư-mục)
3. [⚙️ Công nghệ sử dụng](#️-công-nghệ-sử-dụng)
4. [🔄 Data Pipeline](#-data-pipeline)
5. [🚀 Cách chạy dự án](#-cách-chạy-dự-án)
6. [📈 Kết quả đạt được](#-kết-quả-đạt-được)

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
│
├── data/                     # Dữ liệu Olympic
│   ├── Athletes.csv          # Thông tin vận động viên
│   ├── Coaches.csv           # Thông tin huấn luyện viên
│   ├── EntriesGender.csv     # Số lượng VĐV theo giới tính
│   ├── Medals.csv            # Bảng tổng kết huy chương
│   └── Teams.csv             # Thông tin đội tuyển
│
├── Transformation.ipynb      # Notebook phân tích & xử lý dữ liệu
└── README.md                 # Tài liệu mô tả dự án


```


---

## ⚙️ Công nghệ sử dụng  
- **Azure Data Factory** → Thu thập & ingest dữ liệu từ nguồn vào Data Lake.  
- **Azure Data Lake Storage Gen2** → Lưu trữ dữ liệu raw và transformed.  
- **Azure Databricks (Apache Spark)** → Làm sạch, chuẩn hóa, biến đổi dữ liệu.
- **Azure Synapse Analytics** → Phân tích dữ liệu đã biến đổi, chạy truy vấn OLAP.  

---

## 🔄 Data Pipeline  

### 1. Data Ingestion  
- Dữ liệu từ nhiều nguồn (`CSV`, `API`) được ingest thông qua **Azure Data Factory**.  
- Lưu vào **Raw Zone** của **Azure Data Lake Gen2**.  

### 2. Transformation  
- **Azure Databricks (PySpark)** đọc dữ liệu raw từ Data Lake.  
- Thực hiện làm sạch, join, chuẩn hóa và lưu lại vào **Transformed Zone**.  

### 3. Analytics  
- **Azure Synapse Analytics** kết nối với Data Lake (transformed data).  
- Thực hiện các truy vấn phân tích: thống kê huy chương, phân tích giới tính, đội tuyển, vận động viên.  


## 🚀 Cách chạy dự án  

### 1️⃣ Chuẩn bị môi trường  
- Tạo **Azure Data Lake Storage Gen2** và upload folder `data/` vào container (ví dụ: `tokyo`).  
- Tạo **Azure Databricks Workspace** và cluster Spark (Python runtime).  

### 2️⃣ Import Notebook  
- Trên Azure Databricks, import file `Transformation.ipynb` vào Workspace.  

### 3️⃣ Cấu hình kết nối Spark với ADLS bằng OAuth  
Trong Databricks Notebook, chạy đoạn lệnh sau (thay `<...>` bằng thông tin thực tế):  

```python
spark.conf.set(
    "fs.azure.account.auth.type.tokyoolympicdata7.dfs.core.windows.net",
    "OAuth"
)
spark.conf.set(
    "fs.azure.account.oauth.provider.type.tokyoolympicdata7.dfs.core.windows.net",
    "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider"
)
spark.conf.set(
    "fs.azure.account.oauth2.client.id.tokyoolympicdata7.dfs.core.windows.net",
    "<APPLICATION_CLIENT_ID>"
)
spark.conf.set(
    "fs.azure.account.oauth2.client.secret.tokyoolympicdata7.dfs.core.windows.net",
    "<CLIENT_SECRET>"
)
spark.conf.set(
    "fs.azure.account.oauth2.client.endpoint.tokyoolympicdata7.dfs.core.windows.net",
    "https://login.microsoftonline.com/<TENANT_ID>/oauth2/token"
)
```
### 4️⃣ Đọc dữ liệu từ Data Lake vào Spark
```bash
athletes_df = spark.read.csv("abfss://tokyo@tokyoolympicdata7.dfs.core.windows.net/Athletes.csv",
                             header=True, inferSchema=True)
athletes_df.show(5)
```
### 5️⃣ Thực thi Notebook
- Chạy các cell trong Transformation.ipynb để làm sạch, biến đổi dữ liệu.  
- Kết quả được lưu lại trong Data Lake (Transformed Zone) và có thể truy vấn bằng Azure Synapse Analytics hoặc visualize bằng Power BI / Tableau / Looker Studio. 


## 📈 Kết quả đạt được
Phân tích dữ liệu Olympic Tokyo 2021 trên Spark.

Thực hiện nhiều thống kê về vận động viên, đội tuyển, huy chương, giới tính.

Tích hợp dữ liệu thành công với Azure Cloud.

