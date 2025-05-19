# Các mô hình ngôn ngữ

Mô hình ngôn ngữ là các mô hình toán học được thiết kế để hiểu và sinh ra ngôn ngữ tự nhiên. Chúng đóng vai trò nền tảng trong nhiều ứng dụng NLP hiện đại. Dưới đây là tổng quan về các loại mô hình ngôn ngữ chính:

### Mô hình ngôn ngữ thống kê truyền thống

#### N-gram Models

Nguyên lý: Dự đoán từ tiếp theo dựa trên N-1 từ trước đó
Ứng dụng: Sửa lỗi chính tả, gợi ý từ, sinh văn bản đơn giản
Hạn chế: Không nắm bắt được ngữ cảnh dài, bị giới hạn bởi kích thước N

#### Hidden Markov Models (HMM)

Đặc điểm: Mô hình xác suất cho chuỗi có trạng thái ẩn
Ứng dụng: Gán nhãn từ loại, nhận dạng thực thể có tên

### Mô hình ngôn ngữ dựa trên mạng nơ-ron

#### Mạng nơ-ron hồi quy (RNN)

Cấu trúc: Mạng nơ-ron có vòng lặp, xử lý dữ liệu tuần tự
Hạn chế: Vấn đề gradient biến mất/bùng nổ khi xử lý chuỗi dài

#### LSTM (Long Short-Term Memory)

Đặc điểm: Biến thể của RNN với cơ chế cổng để nhớ thông tin dài hạn
Ưu điểm: Giải quyết vấn đề phụ thuộc xa trong chuỗi

#### GRU (Gated Recurrent Unit)

Đặc điểm: Đơn giản hóa của LSTM, ít tham số hơn nhưng hiệu quả tương đương
Ứng dụng: Dịch máy, tóm tắt văn bảnss
