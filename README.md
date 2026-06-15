# 📚 Amazon Book Reviews Analysis

> [!NOTE]
> This project was developed as part of the **YZV 411E - Big Data Analytics** course at Istanbul Technical University (ITU).

A large-scale data engineering and natural language processing (NLP) project designed to analyze Amazon book reviews using **Apache Spark (PySpark)** and **Spark NLP**.

This project supports both **local execution** (for testing on sampled subsets) and **cloud cluster execution** (utilizing GCP Dataproc, GCS, and HDFS for distributed computing).

---

<p align="left">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Apache_Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white" />
  <img src="https://img.shields.io/badge/Google_Cloud-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white" />
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" />
</p>

---

## 📂 Project Structure

```
├── docs/
│   ├── Amazon-Book-Reviews-Presentation.pptx   # Project presentation slides
│   └── Amazon-Book-Reviews-Report.pdf         # Comprehensive project report
├── Amazon-Book-Reviews-Local.ipynb            # PySpark script for local testing (clean outputs)
├── Amazon-Book-Reviews-Cloud.ipynb            # PySpark script configured for Dataproc cluster execution
├── books_rating_100.csv                       # Sample dataset partition for local testing (~28 MB)
├── requirements.txt                           # Project dependencies
└── README.md                                  # Project documentation
```

---

## 💻 1. Local Setup & Execution

### **Prerequisites**
Make sure the following are installed locally:
- **Python**: Version `3.8` or higher.
- **Java**: OpenJDK 11 (required for running Apache Spark).
- **Apache Spark**: PySpark `3.5.3`.

### **Steps to Run Locally**

1. **Clone the Repository**
   ```bash
   git clone https://github.com/furkngoksu/Amazon-Book-Reviews-Analysis.git
   cd Amazon-Book-Reviews-Analysis
   ```

2. **Download the Datasets**
   - The sample file `books_rating_100.csv` is already included in the root folder.
   - For the full local test, download the `books_data.csv` file from the [Kaggle Amazon Books Reviews Dataset](https://www.kaggle.com/datasets/mohamedbakhet/amazon-books-reviews?select=books_data.csv) and place it in the project root folder.

3. **Set Up a Virtual Environment**
   - Create a virtual environment:
     ```bash
     python3 -m venv venv
     ```
   - Activate the virtual environment:
     - **macOS / Linux**:
       ```bash
       source venv/bin/activate
       ```
     - **Windows**:
       ```bash
       venv\Scripts\activate
       ```
   - Install dependencies:
     ```bash
     pip install -r requirements.txt
     ```

4. **Run the Analysis**
   - Launch Jupyter:
     ```bash
     jupyter notebook
     ```
   - Open **`Amazon-Book-Reviews-Local.ipynb`** and run the cells sequentially.

---

## ☁️ 2. Cloud Setup & Execution (GCP Dataproc)

### **Prerequisites**
- An active **Google Cloud Platform (GCP)** account.
- **Google Cloud SDK** installed and configured:
  ```bash
  gcloud auth login
  ```

### **Steps to Run in the Cloud**

1. **Prepare the Datasets**
   Download the raw datasets from Kaggle:
   - `books_info.csv`
   - `books_rating.csv`

2. **Create a GCS Bucket**
   Create a Google Cloud Storage bucket (e.g., `spark-data-bucket`) to host the raw files.

3. **Upload Data to GCS**
   ```bash
   gsutil cp path/to/books_info.csv gs://spark-data-bucket/
   gsutil cp path/to/books_rating.csv gs://spark-data-bucket/
   ```

4. **Transfer Files to HDFS on Dataproc Master Node**
   Once your GCP Dataproc cluster is running, SSH into the master node and load the datasets into HDFS:
   ```bash
   hadoop fs -copyFromLocal /path/to/books_info.csv /hdfs/path/books_info.csv
   hadoop fs -copyFromLocal /path/to/books_rating.csv /hdfs/path/books_rating.csv
   ```

5. **Update Paths in Cloud Notebook**
   In **`Amazon-Book-Reviews-Cloud.ipynb`**, verify that the paths are set to the HDFS URL scheme:
   ```python
   books_info = spark.read.csv("hdfs:///hdfs/path/books_info.csv", header=True, schema=books_info_schema)
   books_rating = spark.read.csv("hdfs:///hdfs/path/books_rating.csv", header=True, schema=books_rating_schema)
   ```

6. **Run the Notebook**
   Execute the `Amazon-Book-Reviews-Cloud.ipynb` cells on the cluster to complete the full-scale analysis.
