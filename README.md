# Sổ tay Sinh viên HUST 

---
## 🚀 Demo

🔗 **[Truy cập demo tại đây](https://ducanhdaniel.github.io/sotaysinhvienhust_rag/)**

> ⚠️ **Lưu ý:**  
> Trước khi mở demo, hãy chạy endpoint backend để “đánh thức” server:  
> 👉 [https://sotaysinhvienhust-rag.onrender.com](https://sotaysinhvienhust-rag.onrender.com)

---
## 📝 Giới thiệu dự án
**Sổ tay sinh viên HUST (RAG)** là một hệ thống **trợ lý số cho sinh viên Đại học Bách Khoa Hà Nội**.

- Cung cấp thông tin học tập, học bổng, hoạt động ngoại khóa, việc làm.  
- Tích hợp **Q&A với RAG (Retrieval Augmented Generation)**.  
- Hỗ trợ **Text-to-Speech (TTS)** để trả lời bằng giọng nói.  
- Giao diện web tĩnh (HTML/CSS/JS) + backend Python (FastAPI).  
- Kết nối nhiều nguồn dữ liệu (`job_data`, `audio`, API học bổng/hoạt động).

Dự án vừa hỗ trợ sinh viên, vừa là ví dụ demo **ứng dụng RAG + MCP + TTS**.

---

## 🏛️ Cấu trúc Dự án
**Dự án được tổ chức theo kiến trúc phân lớp (layered architecture) để đảm bảo tính module hóa, dễ bảo trì và mở rộng.**

```
.
├── app/
│   ├── api/
│   │   └── routers/            # Tầng API: Xử lý request/response HTTP
│   │       ├── chat.py
│   │       ├── tts.py
│   │       ├── jobs.py
│   │       ├── activities.py
│   │       └── scholarship.py
│   │
│   ├── services/               # Tầng nghiệp vụ: Chứa logic chính của ứng dụng
│   │   ├── rag_service.py
│   │   ├── tts_service.py
│   │   ├── job_service.py
│   │   ├── activity_service.py
│   │   └── scholarship_service.py
│   │
│   ├── models/                 # Tầng dữ liệu: Định nghĩa cấu trúc dữ liệu
│   │   └── schemas.py
│   │
│   │
│   ├── rag/                    # Module RAG (Retrieval-Augmented Generation) độc lập
│   │   ├── agent.py
│   │   └── tools.py
│   │
│   └── main.py                 # Điểm khởi tạo ứng dụng FastAPI
│
├── .env                        # Biến môi trường (API keys, etc.)
├── requirements.txt            # Các gói Python cần thiết
└── run.py                      # Script để khởi động server
```

### Giải thích các thành phần

- **`run.py`**: Đây là file duy nhất cần chạy (`python run.py`) để khởi động máy chủ web Uvicorn.

- **`app/`**: **Thư mục ứng dụng chính**. Chứa toàn bộ mã nguồn của ứng dụng:
    - **`main.py`**: Khởi tạo đối tượng FastAPI, cấu hình middleware, gắn router từ `api/routers`.
    - **`api/routers/`**: Định nghĩa các endpoint. Tầng này nhận yêu cầu HTTP và trả về phản hồi.
    - **`services/`**: Chứa logic xử lý cho từng chức năng (ví dụ: crawl dữ liệu, gọi RAG agent, v.v.).
    - **`models/schemas.py`**: Định nghĩa các class Pydantic để xác thực dữ liệu đầu vào và định dạng dữ liệu đầu ra.
    - **`rag/`**: Chứa tất cả logic liên quan đến agent AI, bao gồm việc xây dựng graph và định nghĩa các công cụ.
---

## 🔄 Luồng hoạt động backend
 **Backend xử lý**:
   - Xác định câu hỏi → gọi RAG pipeline (LangChain, Pinecone, Tavily, Google API).  
   - Trả về câu trả lời (text/audio).
     
   <img width="1208" height="443" alt="image" src="https://github.com/user-attachments/assets/e2fdfbd9-018b-450c-8bcc-53e3ba6d78fd" />


---

## 🛠️ Các tool MCP
Repo có tích hợp **Model Context Protocol (MCP)** để kết nối với Agent:

| Tool                    | Chức năng                                                                 |
|--------------------------|----------------------------------------------------------------------------|
| **query_classifier**     | Kiểm duyệt câu hỏi → phân loại `safe` hoặc `sensitive_political`.          |
| **search_student_handbook** | Tra cứu Sổ tay Sinh viên: đời sống, CLB, ký túc xá, học bổng...          |
| **search_academic_regulations** | Tra cứu Quy chế Đào tạo: tín chỉ, GPA, đăng ký học phần, tốt nghiệp... |
| **search_law_vietnam**   | Tra cứu văn bản pháp luật: Hiến pháp, Bộ luật, Luật Giáo dục...            |
| **search_website**       | Tìm kiếm Google & scrape nội dung web (bỏ domain `hust.edu.vn`).           |
| **get_scholarships**     | Lấy danh sách học bổng, lọc theo trạng thái (open/expired) & thời gian.   |


---


# Hướng dẫn cài đặt và sử dụng dự án sotaysinhvienhust_rag

Đây là hướng dẫn chi tiết để bạn có thể cài đặt, thiết lập và chạy dự án này trên máy tính của mình.

### 1. Yêu cầu

Dự án sử dụng uv để quản lý các gói thư viện Python. Bạn cần cài đặt uv trước khi bắt đầu.

### 2. Cài đặt

1. Clone repository:
   Mở terminal hoặc command prompt và sao chép dự án về máy tính của bạn:

```
    git clone https://github.com/DucanhDaniel/sotaysinhvienhust_rag
    cd sotaysinhvienhust_rag
```

2. Cài đặt các gói thư viện:
  Sử dụng uv để cài đặt tất cả các thư viện cần thiết từ file requirements.txt:

```
    uv pip install -r requirements.txt
```

3. Thiết lập file .env:
  Tạo một file có tên .env trong thư mục gốc của dự án và điền các khóa API của bạn vào đó. File này sẽ chứa các biến môi trường quan trọng.

```
  GOOGLE_API_KEY=
  PINECONE_API_KEY=
  LANGCHAIN_API_KEY=
  TAVILY_API_KEY=
  SERPAPI_KEY=
```

### 3. Cách chạy

1. Khởi chạy ứng dụng:
Chạy file chính của dự án bằng lệnh sau:

```
  python main.py
```

2. Truy cập giao diện người dùng:
   
   - Sau khi ứng dụng đã chạy, mở file ui/index.html bằng bất kỳ trình duyệt web nào để bắt đầu sử dụng.

3. Thiết lập host TTS model trên colab (optional):
   
   - Chạy `notebook/StyleTTS2_lite_vi_(2s).ipynb` trên Google Colab, dán link Ngrok thu được ở cuối file vào **EXTERNAL_TTS_URL** trong `app/services/tts_service.py`.

   - App vẫn sẽ có tính năng giọng nói, nhưng khi chạy phần này sẽ có tùy chọn giọng nói đa dạng và chất lượng tốt hơn.

### 4. Lưu ý quan trọng

* Để sử dụng dữ liệu về bách khoa và pháp luật được lưu trữ trên Pinecone, bạn cần phải liên hệ với chủ sở hữu dự án để được cấp quyền truy cập.

* Nếu bạn muốn sử dụng dữ liệu riêng của mình, bạn cần chỉnh sửa file tools.py và thay đổi các biến namespace và index_name cho phù hợp với cơ sở dữ liệu Pinecone của bạn.

---

# Tài liệu API

Dưới đây là tài liệu cho các endpoint API của ứng dụng, bao gồm mô tả, tham số và định dạng phản hồi. 

**Tất cả dữ liệu học bổng, việc làm đều được lấy từ trang CTSV của Đại học Bách khoa Hà Nội.**

### 1. Endpoint: /ask

* **Mô tả**: Nhận một câu hỏi và trả về câu trả lời. Endpoint này có khả năng duy trì ngữ cảnh hội thoại thông qua `session_id`.

* **Phương thức**: `POST`

* **Yêu cầu (Request Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body:

    ```
    {
      "question": "string",
      "session_id": "string | null"
    }
    
    ```

    * `question` (string): Câu hỏi của người dùng.

    * `session_id` (string, tùy chọn): ID của phiên hội thoại hiện tại. Nếu không được cung cấp hoặc không hợp lệ, server sẽ tạo một phiên mới.

* **Phản hồi (Response Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body:

    ```
    {
      "answer": "string",
      "session_id": "string"
    }
    
    ```

    * `answer` (string): Câu trả lời từ AI.

    * `session_id` (string): ID của phiên hội thoại hiện tại.

### 2. Endpoint: /tts

* **Mô tả**: Chuyển đổi văn bản thành giọng nói. Endpoint này ưu tiên sử dụng một API TTS ngoài và sẽ tự động chuyển sang `gTTS` nếu API ngoài không khả dụng.

* **Phương thức**: `POST`

* **Yêu cầu (Request Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body:

    ```
    {
      "text": "string",
      "speaker_id": "integer"
    }
    
    ```

    * `text` (string): Văn bản cần chuyển đổi.

    * `speaker_id` (integer, tùy chọn): ID của người nói (mặc định là `1`).

* **Phản hồi (Response Body)**:

  * Kiểu dữ liệu: `audio/wav` hoặc `audio/mpeg`

  * Body: Dữ liệu nhị phân của file âm thanh.

### 3. Endpoint: /scholarships

* **Mô tả**: Lấy danh sách tất cả các học bổng.

* **Phương thức**: `GET`

* **Yêu cầu (Request Body)**: Không có.

* **Phản hồi (Response Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body: Một mảng các đối tượng (object), mỗi đối tượng đại diện cho một học bổng.

    ```
    [
      {
        "DocumentId": "int",
        "Title": "string",
        "Deadline": "string",
        "TotalPrice": "string",
        "Description": "string",
        "Content": "string"
      },
      ...
    ]
    
    ```
    
  * Example: 

    ```
    [
        {
          "DocumentId": 175,
          "Title": "Học bổng Goertek Việt Nam năm học 2025-2026",
          "Deadline": "2025-08-27 23:55:00",
          "TotalPrice": "Lên tới 60.000.000",
          "Description": "Học bổng tài trợ",
          "Content": "<p>Goertek..."
       },
       ...

    ]
    
    ```

### 4. Endpoint: /jobs

* **Mô tả**: Lấy danh sách việc làm với các tùy chọn lọc.

* **Phương thức**: `GET`

* **Yêu cầu (Request Parameters)**:

  * `job_type` (bắt buộc): Loại công việc. Các giá trị hợp lệ: `hot`, `new`, `internship`.

  * `career` (tùy chọn): Tên chuyên ngành để lọc. Ví dụ: `công nghệ thông tin`.

  * `city` (tùy chọn): Tên thành phố để lọc. Ví dụ: `Hà Nội`.

* **Phản hồi (Response Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body: Một mảng các đối tượng, mỗi đối tượng đại diện cho một công việc.

    ```
    [
      {
        "document_id": "int",
        "source_link": "string",
        "title": "string",
        "company_name": "string",
        "salary": "string",
        "deadline": "string",
        "location": "string",
        "work_type": "string",
        "experience_required": "string",
        "majors_required": "string",
        "positions_available": "int",
        "description": "string"
      },
      ...
    ]
    
    ```

  * Example

    ```
      [
        {
          "document_id": 3787,
          "source_link": "https://ctsv.hust.edu.vn/#/doanh-nghiep/chi-tiet-bai-dang/3787",
          "title": "KỸ SƯ TỰ ĐỘNG HÓA/ QUẢN LÝ DỰ ÁN PM",
          "company_name": "CÔNG TY TNHH CÔNG NGHIỆP CHÍNH XÁC BOZHON VIỆT NAM",
          "salary": "Theo thỏa thuận",
          "deadline": "2025-09-30 00:00:00",
          "location": "Bắc Giang,Bắc Ninh,Nam Định,Phú Thọ,Vĩnh Phúc",
          "work_type": "Toàn thời gian cố định",
          "experience_required": "Không yêu cầu",
          "majors_required": "Điện tử viễn thông,Kỹ thuật cơ khí,Kỹ thuật cơ điện tử,Kỹ thuật điện - điện tử",
          "positions_available": 40,
          "description": "<ol>\n<li><strong>KỸ SƯ THIẾT KẾ MÁY TỰ ĐỘNG HÓA..."
      },
      ...
    ]
    
    ```

### 5. Endpoint: /jobs/careers

* **Mô tả**: Lấy danh sách tất cả các chuyên ngành có sẵn để lọc việc làm.

* **Phương thức**: `GET`

* **Yêu cầu (Request Body)**: Không có.

* **Phản hồi (Response Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body: Một mảng các chuỗi, mỗi chuỗi là tên một chuyên ngành.

    ```
    [
      "string",
      "string",
      ...
    ]
    
    ```

### 6. Endpoint: /jobs/cities

* **Mô tả**: Lấy danh sách tất cả các thành phố có sẵn để lọc việc làm.

* **Phương thức**: `GET`

* **Yêu cầu (Request Body)**: Không có.

* **Phản hồi (Response Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body: Một mảng các chuỗi, mỗi chuỗi là tên một thành phố.

    ```
    [
      "string",
      "string",
      ...
    ]
    
    ```

### 7. Endpoint: /activities

* **Mô tả**: Lấy danh sách tất cả các hoạt động, sự kiện.

* **Phương thức**: `GET`

* **Yêu cầu (Request Body)**: Không có.

* **Phản hồi (Response Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body: Một mảng các đối tượng, mỗi đối tượng đại diện cho một hoạt động.

    ```
    [
      {
        "id": "int",
        "title": "string",
        "organizer": "string",
        "activity_type": "string",
        "start_time": "string",
        "image_url": "string"
      },
      ...
    ]
    
    ```

  * Example:

    ```
      [
        {
          "id": 14515,
          "title": "Tuần định hướng của sinh viên K70",
          "organizer": "Ban CTSV",
          "activity_type": "Giáo dục Chính trị & Tư tưởng",
          "start_time": "2025-09-08 07:30:00",
          "image_url": "https://ctsv.hust.edu.vn/UploadFile/CTSV/DownloadImageAcitivity?AId=14515"
      },
      ...
    ]
    
    ```

### 8. Endpoint: /activities/{activity_id}

* **Mô tả**: Lấy thông tin chi tiết của một hoạt động cụ thể dựa trên ID.

* **Phương thức**: `GET`

* **Yêu cầu (Request Parameters)**:

  * `activity_id` (bắt buộc): ID của hoạt động cần xem chi tiết.

* **Phản hồi (Response Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body: Một đối tượng chứa thông tin chi tiết của hoạt động.

    ```
    {
      "id": "integer",
      "title": "string",
      "date": "string",
      "description": "string",
      "link": "string"
    }
    
    ```

  * Example:

    ```
      {
        "id": 14515,
        "title": "Tuần định hướng của sinh viên K70",
        "organizer": "Ban CTSV",
        "activity_type": "Giáo dục Chính trị & Tư tưởng",
        "start_time": "2025-09-08 07:30:00",
        "image_url": "https://ctsv.hust.edu.vn/UploadFile/CTSV/DownloadImageAcitivity?AId=14515",
        "finish_time": "2025-09-12 17:00:00",
        "location": "Hội trường C2",
        "description": "<p style=\"text-align: justify;\">heo Quy chế của Bộ GĐ&amp;ĐT..."
    }
    
    ```














