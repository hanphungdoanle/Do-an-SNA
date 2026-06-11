# Amazon Co-purchasing Network Analysis

## 1. Giới thiệu dự án

Đây là đồ án cuối kỳ môn **Social Network Analysis (SNA)** với chủ đề:

**Analyzing Amazon's Co-purchasing Network: Exploring Product Community Structure and Link Prediction Application in the Recommendation System.**

Dự án mô hình hóa dữ liệu mua chung sản phẩm Amazon thành một mạng lưới sản phẩm, trong đó:

- **Node**: một sản phẩm Amazon.
- **Edge**: hai sản phẩm thường được mua cùng nhau.

Mục tiêu chính của dự án:

- Phân tích đặc trưng mạng lưới Amazon co-purchasing.
- Xác định sản phẩm quan trọng bằng các chỉ số centrality.
- Phát hiện cộng đồng sản phẩm bằng thuật toán Louvain.
- Dự đoán liên kết mua chung bằng Jaccard Coefficient và Adamic-Adar Index.
- Xây dựng demo gợi ý sản phẩm dựa trên Adamic-Adar.

---

## 2. Cấu trúc thư mục

```text
SNA-Amazon-CoPurchasing/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   └── processed/
│       ├── amazon_graph.gpickle
│       ├── centrality_scores.csv
│       ├── cluster_labels.csv
│       ├── demo_recommendations_product_415888.csv
│       └── top_pagerank_nodes_for_report.csv      
│
├── notebooks/
│   ├── 01_data_preprocessing.ipynb
│   ├── 02_centrality_analysis.ipynb
│   ├── 03_community_detection.ipynb
│   ├── 04_link_prediction.ipynb
│   └── 05_live_demo.ipynb
│
└── outputs/
    └── figures/
        ├── auc_chart.png
        ├── network_overview.png
        └── demo_recommendation_415888.png


```

---

## 3. Dataset

Dự án sử dụng bộ dữ liệu **Amazon product co-purchasing network** từ Stanford SNAP.

Nguồn dữ liệu:

```text
https://snap.stanford.edu/data/bigdata/communities/com-amazon.ungraph.txt.gz
```

Do dữ liệu gốc có quy mô lớn, nhóm trích xuất một subgraph liên thông gồm:

| Chỉ số | Giá trị |
|---|---:|
| Nodes | 8,000 |
| Edges | 19,903 |
| Density | 0.00062 |
| Average Shortest Path | 6.2674 |

File graph sau xử lý được lưu tại:

```text
data/processed/amazon_graph.gpickle
```

---

## 4. Kết quả chính

### 4.1. Centrality Analysis

Nhóm tính ba chỉ số centrality:

- **Degree Centrality**: phát hiện sản phẩm có nhiều quan hệ mua chung trực tiếp.
- **Betweenness Centrality**: phát hiện sản phẩm đóng vai trò cầu nối giữa các cụm.
- **PageRank**: phát hiện sản phẩm có ảnh hưởng cao trong toàn mạng.

Output:

```text
data/processed/centrality_scores.csv
```

### 4.2. Community Detection

Thuật toán Louvain được dùng để phát hiện cộng đồng sản phẩm.

Kết quả:

| Chỉ số | Giá trị |
|---|---:|
| Number of communities | 53 |
| Modularity | 0.8643 |

Output:

```text
data/processed/cluster_labels.csv
```

### 4.3. Link Prediction

Nhóm so sánh hai thuật toán:

| Thuật toán | AUC-ROC |
|---|---:|
| Adamic-Adar Index | 0.7923 |
| Jaccard Coefficient | 0.7897 |

Vì Adamic-Adar có AUC cao hơn, demo sử dụng **Adamic-Adar Index** làm thuật toán gợi ý chính.

Output hình ROC:

```text
outputs/figures/auc_chart.png
```

### 4.4. Live Demo Recommendation

Demo nhận đầu vào là `Product ID`, sau đó trả về:

- Community ID của sản phẩm.
- Degree Count và PageRank.
- Top 5 sản phẩm gợi ý bằng Adamic-Adar.
- Network minh họa quanh sản phẩm đầu vào.

Notebook demo:

```text
notebooks/05_live_demo.ipynb
```

---

## 5. Cài đặt môi trường

Cài các thư viện cần thiết:

```bash
pip install -r requirements.txt
```

Nội dung gợi ý cho `requirements.txt`:

```text
networkx
pandas
numpy
matplotlib
scikit-learn
python-louvain
jupyter
notebook
```

---

## 6. Cách chạy project

### Bước 1: Chạy tiền xử lý dữ liệu

```text
notebooks/01_data_preprocessing.ipynb
```

Notebook này tải dữ liệu từ SNAP, trích xuất subgraph và tạo file:

```text
data/processed/amazon_graph.gpickle
```

### Bước 2: Chạy centrality analysis

```text
notebooks/02_centrality_analysis.ipynb
```

Output:

```text
data/processed/centrality_scores.csv
```

### Bước 3: Chạy community detection

```text
notebooks/03_community_detection.ipynb
```

Output:

```text
data/processed/cluster_labels.csv
```

### Bước 4: Chạy link prediction

```text
notebooks/04_link_prediction.ipynb
```

Output:

```text
outputs/figures/auc_chart.png
```

### Bước 5: Chạy live demo

```text
notebooks/05_live_demo.ipynb
```

Ví dụ demo:

```python
demo_recommendation(415888)
```

---

## 7. Lưu ý khi chạy trên Google Colab

Nếu chạy trên Google Colab, cần upload hoặc mount Google Drive chứa các file sau:

```text
amazon_graph.gpickle
centrality_scores.csv
cluster_labels.csv
```

Nếu notebook bị lỗi đường dẫn, hãy chỉnh lại biến path ở đầu notebook, ví dụ:

```python
GRAPH_PATH = "../data/processed/amazon_graph.gpickle"
CENTRALITY_PATH = "../data/processed/centrality_scores.csv"
CLUSTER_PATH = "../data/processed/cluster_labels.csv"
```

---
