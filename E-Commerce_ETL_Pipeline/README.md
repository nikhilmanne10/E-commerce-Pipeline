# E-commerce Data Engineering Pipeline

A production-ready, end-to-end data pipeline for processing e-commerce data using modern cloud and orchestration tools.

## What This Project Does

This pipeline ingests raw e-commerce data, transforms it through an ETL process, computes business metrics, and stores the results in a scalable AWS S3 data lake — all orchestrated and containerized for production deployment.

### Pipeline Components:
1. **Data Ingestion**: Uploads raw CSV data to AWS S3
2. **ETL Processing**: Cleans, transforms, and computes business metrics using Pandas/Polars
3. **Workflow Orchestration**: End-to-end pipeline orchestration with Prefect
4. **Containerization**: Fully Dockerized for consistent, production-ready deployment
5. **Data Quality**: Validation and testing with Great Expectations

## Architecture

```
📊 Raw Data       →    📤 S3 Upload    →    🧹 Processing    →    📈 Metrics    →    ☁️ S3 Storage
    (CSV Files)         (Raw Layer)         (Transform)        (Analytics)       (Processed Layer)
        ↓                    ↓                  ↓                  ↓                ↓
   Python Scripts    →   boto3/S3 API    →   Pandas/ETL     →   Aggregations  →  Data Lake
                                              ↓
                                         🔄 Prefect Flows (Orchestration)
                                              ↓
                                         🐳 Docker Containers (Production)
```

## Technology Stack

| Category | Technology | Purpose |
|----------|------------|---------|
| **Language** | Python 3.12 | Core development language |
| **Cloud Platform** | AWS S3 | Scalable data lake storage |
| **Orchestration** | Prefect 3.x | Workflow management & monitoring |
| **Data Processing** | Pandas, Polars | Data transformation & analysis |
| **Containerization** | Docker + Docker Compose | Production deployment |
| **Data Validation** | Great Expectations | Data quality assurance |
| **Package Management** | pip, requirements.txt | Dependency management |

## Project Structure

```
Hands-On-Project/
├── 📄 .env                              # AWS credentials (create this!)
├── 📊 data/                             # Sample datasets
│   ├── raw/                             # Raw CSV files
│   ├── customers.csv                    # Customer data
│   ├── products.csv                     # Product catalog
│   ├── orders.csv                       # Order transactions
│   ├── order_items.csv                  # Order line items
│   └── reviews.csv                      # Customer reviews
├── 🔧 data-pipeline/                    # Main pipeline code
│   ├── 📄 requirements.txt              # Python dependencies
│   ├── 🐳 docker-compose.yml            # Container orchestration
│   ├── 📂 src/                          # Source code
│   │   ├── data_ingestion/              # S3 upload scripts
│   │   │   └── s3_uploader.py           # Upload data to AWS S3
│   │   ├── data_processing/             # ETL transformations
│   │   │   └── etl_processor.py         # Clean & transform data
│   │   └── orchestration/               # Workflow management
│   │       └── prefect_flows.py         # Prefect orchestration
│   ├── 📂 config/                       # Configuration files
│   │   ├── aws_config.yaml              # AWS settings
│   │   ├── prefect_config.yaml          # Prefect configuration
│   │   └── data_schemas.yaml            # Data validation rules
│   ├── 📂 infrastructure/               # Container infrastructure
│   │   └── docker/
│   │       └── Dockerfile               # Container build instructions
│   └── 📂 tests/                        # Test files
│       └── test_aws_connection.py       # AWS connectivity tests
└── 📚 CONTAINERIZATION_GUIDE.md         # Docker setup guide
```

## Getting Started

### Prerequisites
- **Python 3.10+** (recommended: 3.12)
- **AWS Account** with S3 access
- **Docker Desktop**
- **Git**

### Step 1: Environment Setup

1. **Navigate to the project directory**
   ```bash
   cd "D:\Hands-On-Project"
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   venv\Scripts\activate  # Windows
   ```

3. **Install dependencies**
   ```bash
   cd data-pipeline
   pip install -r requirements.txt
   ```

4. **Configure AWS credentials** — create a `.env` file in the project root:
   ```env
   AWS_ACCESS_KEY_ID=your_access_key_here
   AWS_SECRET_ACCESS_KEY=your_secret_key_here
   AWS_DEFAULT_REGION=us-east-1
   AWS_S3_BUCKET_NAME=your-unique-bucket-name
   ```

### Step 2: Verify Setup

```bash
python tests/test_aws_connection.py
```
✅ Should output: "Connection successful!"

### Step 3: Run the Pipeline Locally

**Upload raw data to S3:**
```bash
python src/data_ingestion/s3_uploader.py
```
✅ Uploads 5 CSV files to the configured S3 bucket

**Run ETL processing:**
```bash
python src/data_processing/etl_processor.py
```
✅ Cleans data and generates business metrics

**Run the orchestrated pipeline:**
```bash
python src/orchestration/prefect_flows.py
```
✅ Executes the full pipeline end-to-end via Prefect

### Step 4: Prefect Orchestration UI

```bash
prefect server start
```
Open [http://localhost:4200](http://localhost:4200) to monitor flow runs, task logs, and performance.

### Step 5: Run in Docker (Production)

**Build and start containers:**
```bash
cd "D:\Hands-On-Project"
docker-compose -f data-pipeline/docker-compose.yml up --build -d
```

**Execute the pipeline inside the container:**
```bash
docker exec -it data-pipeline-data-pipeline-1 python data-pipeline/src/orchestration/prefect_flows.py
```

Open [http://localhost:4200](http://localhost:4200) to view the containerized pipeline execution.

## Dataset

The pipeline processes realistic synthetic e-commerce data across five tables:

| Dataset | Records | Description |
|---------|---------|-------------|
| **customers.csv** | 1,000+ | Customer profiles and demographics |
| **products.csv** | 500+ | Product catalog with categories and pricing |
| **orders.csv** | 2,000+ | Order transactions with timestamps |
| **order_items.csv** | 6,000+ | Line items per order |
| **reviews.csv** | 1,500+ | Customer reviews and ratings |

## Troubleshooting

**"AWS credentials not found"**
- Ensure the `.env` file exists in the project root (not inside `data-pipeline/`) with valid credentials.

**"Docker daemon not running"**
- Open Docker Desktop and wait for it to fully start before running Docker commands.

**"Can't connect to Prefect server"**
- Wait ~30 seconds after starting containers, then retry [http://localhost:4200](http://localhost:4200).

**"Python module not found"**
- Activate the virtual environment and re-run `pip install -r requirements.txt`. Verify with `pip list`.
