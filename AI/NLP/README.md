### Tokenization là gì?

Tokenization (phân tách từ) là quá trình chia nhỏ văn bản thành các đơn vị nhỏ hơn gọi là “token”. Mỗi token thường là một từ, nhưng tùy vào ngữ cảnh, token có thể là một ký tự, từ, hoặc thậm chí là cụm từ.

### Các cách chuẩn hóa văn bản?

- Chuyển về chữ thường:

```
text = "Hello World!" → "hello world!"
```

- Loại bỏ dấu câu:

```
"hello, how are you?" → "hello how are you"
```

- Loại bỏ khoảng trắng thừa:

```
"I   am   fine" → "I am fine"
```

- Chuẩn hóa unicode:

```
"Thầy giáo dạy toán" và "Thầy giáo dạy toán" (có thể khác nhau nếu dùng dấu tổ hợp)
```

- Loại bỏ stop words:
  Stop words là những từ thường gặp nhưng ít mang nghĩa như: "a", "the", "is", "and" (hoặc trong tiếng Việt: "là", "của", "và", "nhưng").

```
"I am a student" → "student"
```

- Stemming / Lemmatization:
  - Stemming: cắt phần hậu tố để đưa từ về gốc (ví dụ: "running", "runs" → "run")
  - Lemmatization: giống stemming nhưng dùng từ điển để đưa về đúng gốc ngữ pháp

```
"running", "ran", "runs" → "run"
```

- Thay thế viết tắt (Expand contractions):

```
"don't" → "do not"
"can't" → "cannot"
```

- Chuyển đổi số và ký hiệu đặc biệt:
  Loại bỏ số: "I have 2 dogs" → "I have dogs" . Hoặc giữ số lại nếu cần thiết

- Chuẩn hóa chính tả (nếu có):
  Dùng các thư viện để sửa lỗi chính tả:

```
"heloo" → "hello"
```

### Bag of Words là gì?

- BoW xem một văn bản như một tập hợp (bag) các từ (words), bỏ qua ngữ pháp và thứ tự từ, chỉ quan tâm đến:

  - Các từ gì xuất hiện trong văn bản.
  - Tần suất xuất hiện của chúng (số lần lặp lại mỗi từ).

- Cách hoạt đôngj của BoW:
  Lấy tất cả các từ trong tập văn bản và tạo thành một danh sách duy nhất (loại trùng lặp).

Ví dụ với 2 câu sau:

```
1. "I love NLP"
2. "I love deep learning"
```

Sẽ tạo thành Vocabulary: ["I", "love", "NLP", "deep", "learning"]

Sau đó biểu diễn mỗi văn bản thành vector đếm từ:
| Văn bản | I | love | NLP | deep | learning |
| ------------------------- | - | ---- | --- | ---- | -------- |
| 1. "I love NLP" | 1 | 1 | 1 | 0 | 0 |
| 2. "I love deep learning" | 1 | 1 | 0 | 1 | 1 |

- Một số biết thể của BoW:
  - Count Vectorizer: đơn thuần đếm số lần mỗi từ xuất hiện (như ví dụ trên).
  - Binary BoW: chỉ quan tâm từ có xuất hiện hay không (1 hoặc 0).
  - TF-IDF (Term Frequency – Inverse Document Frequency): Thay vì chỉ đếm tần suất, TF-IDF giảm trọng số của từ phổ biến và tăng trọng số của từ đặc trưng.
- Ưu điểm của BoW:
  - Tốc độ nhanh.
  - Dễ hiểu dễ triển khai.
  - Hiệu quả vơí dữ liệu nhỏ.
  - Là nền tảng cho các mô hình đơn giản như Naive Bayes, Logistic Regression
- Nhược điểm của BoW:
  - Bỏ qua ngữ cảnh và thứ tự từ
  - Tạo ra vector rất lớn nếu tập từ vựng lớn (sparse matrix)
  - Không hiểu ý nghĩa ngữ nghĩa giữa các từ (ví dụ "dog" và "puppy" được xem là hoàn toàn khác nhau)
