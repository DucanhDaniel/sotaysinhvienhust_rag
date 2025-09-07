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
```

### 3. Cách chạy

1. Khởi chạy ứng dụng:
Chạy file chính của dự án bằng lệnh sau:

```
  python main.py
```

2. Truy cập giao diện người dùng:
Sau khi ứng dụng đã chạy, mở file ui/index.html bằng bất kỳ trình duyệt web nào để bắt đầu sử dụng.

### 4. Lưu ý quan trọng

* Để sử dụng dữ liệu về bách khoa và pháp luật được lưu trữ trên Pinecone, bạn cần phải liên hệ với chủ sở hữu dự án để được cấp quyền truy cập.

* Nếu bạn muốn sử dụng dữ liệu riêng của mình, bạn cần chỉnh sửa file tools.py và thay đổi các biến namespace và index_name cho phù hợp với cơ sở dữ liệu Pinecone của bạn.



# Tài liệu API

Dưới đây là tài liệu cho các endpoint API của ứng dụng, bao gồm mô tả, tham số và định dạng phản hồi.

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


* **Mô tả**: Lấy thông tin chi tiết của một hoạt động cụ thể dựa trên ID.

* **Phương thức**: `GET`

* **Yêu cầu (Request Parameters)**:

  * `activity_id` (bắt buộc): ID của hoạt động cần xem chi tiết.

* **Phản hồi (Response Body)**:

  * Kiểu dữ liệu: `application/json`

  * Body: Một đối tượng chứa thông tin chi tiết của hoạt động.

    ```
    {
      "id": "int",
      "title": "string",
      "organizer": "string",
      "activity_type": "string",
      "start_time": "string",
      "image_url": "string",
      "finish_time": "string",
      "location": "string",
      "description": "string"
    }
    
    ```
