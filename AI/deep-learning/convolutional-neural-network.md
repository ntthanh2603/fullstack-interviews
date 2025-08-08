# 🖼️ Convolutional Neural Network (CNN)

## 📖 Giới thiệu

Convolutional Neural Network (CNN) là một kiến trúc mạng neural sâu được thiết kế đặc biệt để xử lý dữ liệu có cấu trúc dạng lưới như hình ảnh. CNN được phát triển dựa trên cách thức hoạt động của vỏ não thị giác của động vật, lần đầu được giới thiệu bởi Yann LeCun vào những năm 1980s và trở thành nền tảng của computer vision hiện đại.

![CNN](../assets/CNNs.jpg)

## 🏗️ Kiến trúc

CNN bao gồm các thành phần chính:

### 1. Convolutional Layer (Lớp tích chập)

- Áp dụng các bộ lọc (filters/kernels) để trích xuất đặc trưng
- Giữ nguyên tính chất không gian của dữ liệu

### 2. Pooling Layer (Lớp gộp)

- Giảm kích thước dữ liệu (downsampling)
- Tăng tính bất biến với vị trí (translation invariance)

### 3. Fully Connected Layer (Lớp kết nối đầy đủ)

- Lớp MLP truyền thống ở cuối mạng
- Thực hiện phân loại hoặc hồi quy

### 4. Activation Functions (Hàm kích hoạt)

- ReLU, Sigmoid, Tanh, Leaky ReLU

## ⚙️ Cách thức hoạt động

### Convolution Operation

1. **Bộ lọc**: Kernel trượt qua input với stride nhất định
2. **Dot product**: Tính tích vô hướng tại mỗi vị trí
3. **Feature map**: Tạo ra bản đồ đặc trưng mới
4. **Padding**: Thêm padding để kiểm soát kích thước output

### Pooling Operation

1. **Max Pooling**: Lấy giá trị lớn nhất trong vùng
2. **Average Pooling**: Tính giá trị trung bình
3. **Global Pooling**: Áp dụng trên toàn bộ feature map

### Forward Pass

1. Input → Convolution → Activation → Pooling
2. Lặp lại qua nhiều lớp conv-pool
3. Flatten → Fully Connected → Output

## 🧮 Công thức

### Convolution

```
Y[i,j] = Σₘ Σₙ X[i+m, j+n] × K[m,n] + b
```

### Output Size

```
Output = (Input + 2×Padding - Kernel) / Stride + 1
```

### Pooling

```
Max Pooling: Y[i,j] = max(X[i×s:i×s+k, j×s:j×s+k])
Avg Pooling: Y[i,j] = mean(X[i×s:i×s+k, j×s:j×s+k])
```

Trong đó:

- `X`: Input feature map
- `K`: Kernel/Filter
- `Y`: Output feature map
- `s`: Stride
- `k`: Kernel size
- `b`: Bias

## 📊 Các loại CNN phổ biến

### LeNet (1998)

- Mạng CNN đầu tiên cho nhận dạng chữ viết
- Conv → Pool → Conv → Pool → FC → FC

### AlexNet (2012)

- Đột phá trong ImageNet competition
- Sử dụng ReLU và Dropout

### VGGNet (2014)

- Sử dụng kernel 3×3 nhỏ, mạng sâu
- VGG-16, VGG-19

### ResNet (2015)

- Giới thiệu Skip Connection
- Giải quyết vấn đề vanishing gradient

### Inception/GoogLeNet (2014)

- Multi-scale feature extraction
- Inception modules

## ✅ Ưu điểm

- **Translation Invariance**: Bất biến với vị trí đối tượng
- **Parameter Sharing**: Giảm số lượng tham số cần học
- **Hierarchical Learning**: Học đặc trưng từ đơn giản đến phức tạp
- **Spatial Hierarchy**: Giữ nguyên thông tin không gian
- **Feature Detection**: Tự động trích xuất đặc trưng quan trọng
- **Robust**: Ít bị ảnh hưởng bởi noise và biến dạng nhỏ

## ⚠️ Hạn chế

- **Computation Intensive**: Tốn nhiều tài nguyên tính toán
- **Memory Requirements**: Cần nhiều bộ nhớ cho training
- **Data Hungry**: Yêu cầu lượng dữ liệu lớn
- **Hyperparameter Sensitive**: Nhiều tham số cần điều chỉnh
- **Limited to Grid-like Data**: Chủ yếu cho dữ liệu dạng lưới
- **Black Box**: Khó giải thích quyết định của mô hình

## 🎯 Ứng dụng

### Computer Vision

- **Image Classification**: Phân loại hình ảnh (ImageNet, CIFAR)
- **Object Detection**: Phát hiện đối tượng (YOLO, R-CNN)
- **Image Segmentation**: Phân đoạn hình ảnh (U-Net, Mask R-CNN)
- **Face Recognition**: Nhận dạng khuôn mặt
- **Medical Imaging**: Chẩn đoán y khoa từ X-ray, MRI

### Các lĩnh vực khác

- **Natural Language Processing**: Text classification, sentiment analysis
- **Time Series**: Phân tích dữ liệu chuỗi thời gian
- **Speech Recognition**: Nhận dạng giọng nói
- **Game AI**: Chơi game (AlphaGo)
- **Autonomous Vehicles**: Xe tự lái

## 🛠️ Hyperparameters quan trọng

### Convolutional Layer

- **Number of filters**: Số lượng kernel
- **Kernel size**: Kích thước bộ lọc (3×3, 5×5, 7×7)
- **Stride**: Bước nhảy của kernel
- **Padding**: Same, Valid
- **Activation**: ReLU, Sigmoid, Tanh

### Pooling Layer

- **Pool size**: Kích thước pooling window
- **Stride**: Bước nhảy pooling
- **Type**: Max, Average, Global

### Training

- **Learning rate**: 0.001, 0.01, 0.1
- **Batch size**: 16, 32, 64, 128
- **Optimizer**: Adam, SGD, RMSprop
- **Regularization**: Dropout, L1/L2, Batch Norm

## 💡 Tips tối ưu

### Data Preparation

- **Data Augmentation**: Rotation, flip, crop, zoom
- **Normalization**: Pixel values to [0,1] or [-1,1]
- **Preprocessing**: Resize, center crop

### Architecture Design

- **Start Small**: Bắt đầu với mạng đơn giản
- **Progressive Growing**: Tăng dần độ phức tạp
- **Skip Connections**: Sử dụng residual connections
- **Batch Normalization**: Ổn định training

### Training Strategy

- **Transfer Learning**: Sử dụng pre-trained models
- **Learning Rate Scheduling**: Giảm dần learning rate
- **Early Stopping**: Tránh overfitting
- **Ensemble Methods**: Kết hợp nhiều models

## 📈 Metrics đánh giá

### Classification

- **Accuracy**: Độ chính xác tổng thể
- **Precision**: Độ chính xác dương tính
- **Recall**: Độ nhạy
- **F1-Score**: Điểm F1
- **Confusion Matrix**: Ma trận nhầm lẫn

### Object Detection

- **mAP**: Mean Average Precision
- **IoU**: Intersection over Union
- **Precision-Recall Curve**: Đường cong P-R

## 🔄 So sánh với MLP

| Đặc điểm               | MLP          | CNN              |
| ---------------------- | ------------ | ---------------- |
| Input                  | Vector 1D    | Ma trận 2D/3D    |
| Parameters             | Nhiều        | Ít hơn (sharing) |
| Spatial Info           | Mất          | Giữ nguyên       |
| Translation Invariance | Không        | Có               |
| Tốc độ training        | Nhanh        | Chậm hơn         |
| Ứng dụng chính         | Tabular data | Image data       |

## 🚀 Kiến trúc hiện đại

### Vision Transformer (ViT)

- Áp dụng Transformer cho computer vision
- Cạnh tranh với CNN trong một số tác vụ

### EfficientNet

- Tối ưu hóa depth, width, resolution
- Hiệu quả tính toán cao

### MobileNet

- Thiết kế cho mobile devices
- Depthwise separable convolutions

## 💡 Kết luận

CNN đã cách mạng hóa lĩnh vực computer vision và trở thành xương sống của nhiều ứng dụng AI hiện đại. Từ những ứng dụng đơn giản như phân loại hình ảnh đến các hệ thống phức tạp như xe tự lái, CNN đã chứng minh sức mạnh vượt trội trong việc xử lý dữ liệu hình ảnh. Hiểu rõ CNN không chỉ giúp phát triển các ứng dụng computer vision mà còn là nền tảng để tiếp cận các kiến trúc tiên tiến khác như Transformer và các mô hình multimodal.
