# 🔄 Transformer Architecture

## 📖 Giới thiệu

Transformer là kiến trúc mạng neural cách mạng được giới thiệu trong paper "Attention Is All You Need" (2017) bởi team Google. Khác với RNN/LSTM xử lý tuần tự, Transformer sử dụng hoàn toàn cơ chế Self-Attention để xử lý song song, tạo ra bước ngoặt trong NLP và trở thành nền tảng cho GPT, BERT, T5.

![Transformer](../assets/transformer.png)

## 🏗️ Kiến trúc tổng thể

Transformer gồm hai phần chính:

### Encoder Stack (6 layers)

- **Multi-Head Self-Attention**
- **Position-wise Feed-Forward Network**
- **Residual Connections & Layer Normalization**

### Decoder Stack (6 layers)

- **Masked Multi-Head Self-Attention**
- **Multi-Head Cross-Attention** (Encoder-Decoder)
- **Position-wise Feed-Forward Network**
- **Residual Connections & Layer Normalization**

---

## 🔧 Các thành phần chính

### 1. 🎯 Self-Attention Mechanism

**Mục đích**: Cho phép model "chú ý" đến các phần khác nhau của input sequence

**Công thức**:

```
Attention(Q,K,V) = softmax(QK^T / √d_k) × V
```

**Cách hoạt động**:

1. **Query (Q)**: "Tôi đang tìm kiếm thông tin gì?"
2. **Key (K)**: "Tôi chứa thông tin nào?"
3. **Value (V)**: "Thông tin thực tế là gì?"
4. **Scores**: Tính similarity giữa Q và tất cả K
5. **Weights**: Softmax để tạo attention weights
6. **Output**: Weighted sum của tất cả V

### 2. 🔀 Multi-Head Attention

**Mục đích**: Học nhiều loại quan hệ khác nhau cùng lúc

```
MultiHead(Q,K,V) = Concat(head₁, head₂, ..., head_h) × W^O

head_i = Attention(Q×W_i^Q, K×W_i^K, V×W_i^V)
```

**Ưu điểm**:

- Học được nhiều patterns đồng thời
- Tăng khả năng biểu diễn của model
- Mỗi head tập trung vào aspect khác nhau

### 3. 📍 Positional Encoding

**Mục đích**: Thêm thông tin vị trí vào embeddings

```
PE(pos, 2i) = sin(pos / 10000^(2i/d_model))
PE(pos, 2i+1) = cos(pos / 10000^(2i/d_model))
```

**Tại sao cần**:

- Attention không có khái niệm thứ tự
- Giúp model hiểu vị trí của từ trong câu

### 4. 🔄 Feed-Forward Network

**Kiến trúc**:

```
FFN(x) = max(0, x×W₁ + b₁)×W₂ + b₂
```

**Đặc điểm**:

- 2 linear layers với ReLU ở giữa
- Hidden dimension thường 4x model dimension
- Áp dụng độc lập cho mỗi position

### 5. 🔗 Residual Connections & Layer Norm

**Residual**: `LayerNorm(x + Sublayer(x))`

**Lợi ích**:

- Giải quyết vanishing gradient problem
- Giúp training các model sâu
- Layer Norm ổn định training

### 6. 🎭 Masked Attention (Decoder)

**Mục đích**: Ngăn model nhìn thấy future tokens

**Cách hoạt động**:

- Mask future positions với -∞
- Sau softmax → attention weight = 0
- Đảm bảo autoregressive generation

---

## ⚙️ Cách hoạt động chi tiết

### 🔍 Encoder Process

1. **Input Embedding**: Token → Vector
2. **Positional Encoding**: Thêm thông tin vị trí
3. **Self-Attention**: Mỗi token "nhìn" tất cả tokens khác
4. **Add & Norm**: Residual connection + LayerNorm
5. **Feed-Forward**: Xử lý từng position độc lập
6. **Add & Norm**: Residual connection + LayerNorm
7. **Repeat**: 6 encoder layers

### 🎯 Decoder Process

1. **Output Embedding**: Previous tokens → Vectors
2. **Positional Encoding**: Thêm thông tin vị trí
3. **Masked Self-Attention**: Chỉ nhìn previous tokens
4. **Add & Norm**: Residual + LayerNorm
5. **Cross-Attention**: Attend to encoder output
6. **Add & Norm**: Residual + LayerNorm
7. **Feed-Forward**: Xử lý position-wise
8. **Add & Norm**: Residual + LayerNorm
9. **Linear + Softmax**: Dự đoán next token

### 🔄 Training Process

1. **Teacher Forcing**: Sử dụng ground truth làm decoder input
2. **Parallel Training**: Tất cả positions được tính đồng thời
3. **Loss Calculation**: CrossEntropy cho từng position
4. **Backpropagation**: Update tất cả parameters

---

## ✅ Ưu điểm

- **Parallelization**: Xử lý song song → training nhanh
- **Long-range Dependencies**: Attention trực tiếp giữa mọi cặp tokens
- **Scalability**: Scale tốt với dữ liệu và compute lớn
- **Transfer Learning**: Pre-trained models hiệu quả
- **Interpretability**: Attention weights có thể visualize

## ⚠️ Hạn chế

- **Quadratic Complexity**: O(n²) với sequence length
- **Memory Requirements**: Cần nhiều GPU memory
- **Data Hungry**: Yêu cầu dataset lớn để training hiệu quả
- **Computational Cost**: Expensive để train từ đầu

---

## 🎯 Ứng dụng

### Language Models

- **GPT series**: Text generation, chat
- **BERT**: Understanding tasks, classification
- **T5**: Text-to-text transfer transformer

### Computer Vision

- **Vision Transformer (ViT)**: Image classification
- **DETR**: Object detection
- **Swin Transformer**: Hierarchical vision

### Multimodal

- **CLIP**: Vision-language understanding
- **DALL-E**: Text-to-image generation
- **Flamingo**: Few-shot learning

---

## 🛠️ Hyperparameters quan trọng

### Architecture

- **Model Dimension (d_model)**: 512, 768, 1024
- **Number of Heads**: 8, 12, 16
- **Number of Layers**: 6, 12, 24
- **Feed-Forward Dimension**: 2048, 3072, 4096

### Training

- **Learning Rate**: Warmup + decay schedule
- **Batch Size**: Lớn (1000+ samples)
- **Dropout**: 0.1, 0.2
- **Label Smoothing**: 0.1

---

## 💡 Biến thể quan trọng

### GPT (Generative Pre-trained Transformer)

- **Decoder-only**: Chỉ sử dụng decoder stack
- **Autoregressive**: Generate từ trái sang phải
- **Causal Attention**: Masked self-attention

### BERT (Bidirectional Encoder Representations)

- **Encoder-only**: Chỉ sử dụng encoder stack
- **Bidirectional**: Nhìn cả hai hướng
- **Masked Language Model**: Predict masked tokens

### T5 (Text-to-Text Transfer Transformer)

- **Full Transformer**: Cả encoder và decoder
- **Text-to-text**: Mọi task đều là text generation

---

## 🚀 Cải tiến hiện đại

### Efficiency Improvements

- **Linformer**: Linear attention complexity
- **Performer**: Fast attention via random features
- **Longformer**: Sparse attention patterns

### Scale Improvements

- **Switch Transformer**: Sparse expert models
- **PaLM**: 540B parameter model
- **GPT-4**: Multimodal capabilities

---

## 💡 Kết luận

Transformer đã cách mạng hóa AI với kiến trúc "Attention Is All You Need". Từ NLP đến Computer Vision, từ GPT đến ChatGPT, Transformer là nền tảng của hầu hết breakthrough trong AI hiện đại. Hiểu rõ Transformer là chìa khóa để nắm bắt và phát triển các ứng dụng AI tiên tiến.
