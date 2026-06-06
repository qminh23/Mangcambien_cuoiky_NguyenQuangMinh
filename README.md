# Đồ án: Nhận Diện Vật Thể Văn Phòng Phẩm bằng AI FOMO (Edge AI)

**Môn học:** Mạng cảm biến  
**Học viện Công nghệ Bưu chính Viễn thông (PTIT)**

## 1. Thông tin thành viên
- **Sinh viên thực hiện:** Nguyễn Quang Minh
- **MSSV:** N23DCCI046
- **Lớp:** D23CQCI01-N
- **Giảng viên hướng dẫn:** Thầy Hồ Nhựt Minh

## 2. Mô tả dự án
Dự án ứng dụng trí tuệ nhân tạo tại thiết bị biên (Edge AI) để nhận diện 4 loại vật dụng văn phòng phẩm: **Bút chì, Ghim, Kẹp giấy, Tẩy**. 

Mô hình sử dụng mạng **MobileNetV2 0.35** kết hợp cùng thuật toán phân giải tâm vật thể **FOMO (Faster Objects, More Objects)**. Mô hình được lượng hóa (int8) để tối ưu hóa hoàn toàn cho phần cứng hạn chế (chỉ tiêu tốn ~114KB RAM) và được đóng gói dưới dạng thư viện WebAssembly (WASM) nhằm mục đích nhận diện thời gian thực (real-time) trực tiếp trên trình duyệt Web.

## 3. Cấu trúc thư mục
Dự án được tổ chức thành các thành phần chính như sau:

```text
DoAn_MangCamBien_N23DCCI046/
├── deployment_wasm/             # Mã nguồn triển khai AI lên Web
│   ├── edge-impulse-standalone.js    # Thư viện lõi xử lý AI của Edge Impulse
│   ├── edge-impulse-standalone.wasm  # Mô hình mạng nơ-ron đã biên dịch
│   ├── index.html               # Giao diện Web hiển thị Camera và Bounding Box
│   ├── run-impulse.js           # Script xử lý logic nhận diện khung hình
│   └── server.py                # Script khởi tạo Local Web Server (Python)
└── README.md                    # Tài liệu hướng dẫn sử dụng kho lưu trữ
```

## 4. Các phụ thuộc (Dependencies)
Để chạy thử nghiệm mã nguồn này trên máy tính cá nhân, hệ thống cần đáp ứng các điều kiện:
- **Python 3.x:** Bắt buộc cài đặt để khởi chạy Local Server (nhằm vượt qua chính sách bảo mật CORS của trình duyệt khi load file `.wasm`).
- **Trình duyệt Web hiện đại:** Google Chrome, Microsoft Edge, hoặc Apple Safari (các phiên bản có hỗ trợ WebAssembly và WebRTC).
- **Thiết bị ngoại vi:** Webcam (đối với PC/Laptop) hoặc Camera thiết bị di động để test real-time.

## 5. Hướng dẫn cài đặt và chạy thử nghiệm

**Bước 1: Tải mã nguồn**
Tải hoặc clone toàn bộ thư mục dự án này về máy tính của bạn.

**Bước 2: Mở cửa sổ dòng lệnh**
Sử dụng Terminal (trên macOS/Linux) hoặc Command Prompt/PowerShell (trên Windows).

**Bước 3: Truy cập vào thư mục triển khai**
Sử dụng lệnh `cd` để di chuyển vào thư mục chứa mã nguồn WebAssembly:
```bash
cd đường_dẫn_đến_thư_mục/DoAn_MangCamBien_N23DCCI046/deployment_wasm
```

**Bước 4: Khởi chạy Local Server**
Chạy file Python đã được cung cấp sẵn để mở server cục bộ:
```bash
python3 server.py
```
*(Lưu ý: Trên hệ điều hành Windows, lệnh có thể chỉ là `python server.py`)*

**Bước 5: Truy cập trên trình duyệt**
Mở trình duyệt web và truy cập vào địa chỉ mạng cục bộ sau:
```text
http://localhost:8000
```
*(Cổng port 8000 có thể thay đổi tùy thuộc vào log in ra trên Terminal của bạn).*

**Bước 6: Kiểm thử (Inference Real-time)**
1. Khi trang web tải xong, trình duyệt sẽ yêu cầu quyền truy cập Camera $\rightarrow$ Nhấn chọn **Cho phép (Allow)**.
2. Đưa các vật dụng (Ghim, Tẩy, Kẹp giấy, Bút chì) vào trước ống kính camera ở khoảng cách tương đương với tập dữ liệu huấn luyện.
3. Hệ thống sẽ tự động quét, phát hiện tâm vật thể và hiển thị khung nhận diện (Bounding Box) cùng tốc độ khung hình trên giây (FPS).