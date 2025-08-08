# 🧠 Single Layer Perceptron (SLP)

## 📖 Giới thiệu

Single Layer Perceptron là một thuật toán học máy cơ bản thuộc nhóm mạng neural nhân tạo. Đây là mô hình đơn giản nhất của perceptron, được phát triển bởi Frank Rosenblatt vào năm 1957.

![SLP](../assets/single_layer_perceptron.jpg)

## ⚙️ Cách thức hoạt động

SLP hoạt động dựa trên nguyên lý:

1. **Đầu vào**: Nhận các giá trị đầu vào x₁, x₂, ..., xₙ
2. **Trọng số**: Mỗi đầu vào được nhân với trọng số tương ứng w₁, w₂, ..., wₙ
3. **Tổng hợp**: Tính tổng có trọng số: Σ(wᵢ × xᵢ) + bias
4. **Kích hoạt**: Áp dụng hàm kích hoạt (thường là hàm step) để ra quyết định

## 🧮 Công thức

```
y = f(Σ(wᵢ × xᵢ) + b)
```

Trong đó:

- `y`: đầu ra
- `wᵢ`: trọng số thứ i
- `xᵢ`: đầu vào thứ i
- `b`: bias
- `f()`: hàm kích hoạt

## ✅ Ưu điểm

- **Đơn giản**: Dễ hiểu và implement
- **Nhanh**: Tính toán và huấn luyện nhanh
- **Phù hợp**: Tốt cho các bài toán phân loại tuyến tính

## ⚠️ Hạn chế

- **Chỉ giải được bài toán tuyến tính**: Không thể phân loại dữ liệu phi tuyến
- **Không giải được XOR**: Bài toán XOR là ví dụ kinh điển về hạn chế này
- **Một lớp duy nhất**: Không thể học các mẫu phức tạp

## 🎯 Ứng dụng

- Phân loại email spam
- Nhận dạng ký tự đơn giản
- Phân loại dữ liệu có thể phân tách tuyến tính
- Nền tảng để hiểu các mạng neural phức tạp hơn

## 💡 Kết luận

Single Layer Perceptron là bước đầu quan trọng trong lĩnh vực mạng neural. Mặc dù có hạn chế, nó là nền tảng để hiểu và phát triển các thuật toán phức tạp hơn như Multi-Layer Perceptron và Deep Learning.
