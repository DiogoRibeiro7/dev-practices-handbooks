# Detailed Chapter Outlines: Machine Learning Engineering Book

## Chapter 1: Introduction to ML Engineering vs Data Science

### 1.1 The Evolution from Data Science to ML Engineering

**Content:**
- Historical context: From statistical analysis to production ML
- The "model in a notebook" problem
- Industry statistics on ML project failure rates
- The emergence of MLOps as a discipline

**Practical Example:**
- Case study: A data scientist's notebook vs a production-ready ML system
- Side-by-side comparison showing the transformation

**Code Listing 1.1:**
```python
# Data Scientist's Exploratory Code
import pandas as pd
from sklearn.ensemble import RandomForestClassifier

df = pd.read_csv('data.csv')
X = df.drop('target', axis=1)
y = df['target']
model = RandomForestClassifier()
model.fit(X, y)
# Accuracy: 92%!
```

**Code Listing 1.2:**
```python
# ML Engineer's Production Code
class ModelTrainingPipeline:
    def __init__(self, config):
        self.config = config
        self.logger = setup_logging()
        self.mlflow_client = mlflow.tracking.MlflowClient()

    def run(self):
        try:
            data = self.load_and_validate_data()
            X_train, X_test, y_train, y_test = self.split_data(data)
            model = self.train_model(X_train, y_train)
            metrics = self.evaluate_model(model, X_test, y_test)
            self.log_experiment(model, metrics)
            self.save_model(model)
            return model
        except Exception as e:
            self.logger.error(f"Pipeline failed: {e}")
            raise
```

### 1.2 Key Differences: Roles, Responsibilities, and Mindsets

**Content:**
- Role comparison matrix
- Skills comparison (overlap vs specialization)
- Typical daily activities
- Career progression paths

**Practical Example:**
- Real job description analysis from FAANG companies
- Venn diagram of skills with percentage breakdowns

**Table 1.1: Role Comparison**
```
+-----------------------+-------------------------+---------------------------+
| Aspect                | Data Scientist          | ML Engineer               |
+-----------------------+-------------------------+---------------------------+
| Primary Focus         | Model accuracy          | System reliability        |
| Main Deliverable      | Insights, prototypes    | Production systems        |
| Success Metric        | Model performance       | Uptime, latency, cost     |
| Time Allocation       | 70% exploration         | 70% engineering           |
|                       | 30% communication       | 30% optimization          |
| Tools                 | Jupyter, R, pandas      | Docker, K8s, Airflow      |
+-----------------------+-------------------------+---------------------------+
```

### 1.3 The ML Engineering Workflow: End-to-End

**Content:**
- Complete workflow diagram
- Phase breakdown with time allocation
- Feedback loops and iteration cycles
- Integration points with other teams

**Practical Example:**
- E-commerce recommendation system workflow
- Timeline showing 6-month project breakdown

**Diagram 1.1: ML Engineering Workflow**
```
Problem Definition (2 weeks)
    ↓
Data Collection & EDA (3 weeks)
    ↓
Feature Engineering (4 weeks)
    ↓
Model Development (4 weeks)
    ↓ ← Iteration Loop
Model Evaluation (2 weeks)
    ↓
Deployment (2 weeks)
    ↓
Monitoring & Maintenance (Ongoing)
    ↓
Model Retraining (Monthly)
```

### 1.4 Production ML Systems: Components and Architecture

**Content:**
- High-level architecture patterns
- Component interactions
- Scalability considerations
- Fault tolerance and resilience

**Practical Example:**
- Real-time fraud detection system architecture
- Batch recommendation system architecture

**Code Listing 1.3: System Architecture as Code**
```python
# Architecture definition using Diagrams library
from diagrams import Diagram, Cluster
from diagrams.aws.compute import Lambda, ECS
from diagrams.aws.database import RDS, S3
from diagrams.aws.ml import Sagemaker

with Diagram("ML System Architecture", show=False):
    with Cluster("Data Layer"):
        s3 = S3("Raw Data")
        rds = RDS("Feature Store")

    with Cluster("Training"):
        sagemaker = Sagemaker("Training Job")

    with Cluster("Serving"):
        api = Lambda("API Gateway")
        model_server = ECS("Model Server")

    s3 >> sagemaker >> model_server
    rds >> model_server
    api >> model_server
```

### 1.5 Technical Debt in ML Systems

**Content:**
- Types of ML technical debt
- Hidden technical debt (Sculley et al. paper analysis)
- Debt accumulation patterns
- Strategies for debt prevention and remediation

**Practical Example:**
- Anti-patterns in production ML
- Refactoring case study

**Code Listing 1.4: Technical Debt Example**
```python
# ANTI-PATTERN: Glue Code and Pipeline Jungles
def train_model(data_path):
    # Load data with custom format
    df = pd.read_csv(data_path, sep='|', encoding='latin1')

    # Hardcoded preprocessing
    df['feature1'] = df['raw1'].apply(lambda x: custom_transform(x))
    df = df[df['date'] > '2020-01-01']  # Magic date

    # Inconsistent feature engineering
    if 'category' in df.columns:
        df = pd.get_dummies(df, columns=['category'])

    # Model training with magic numbers
    model = XGBClassifier(n_estimators=273, max_depth=8)  # Why 273?
    model.fit(df.drop('target', axis=1), df['target'])

    # Save without versioning
    pickle.dump(model, open('model.pkl', 'wb'))

# BETTER: Modular, Testable Pipeline
class DataProcessor:
    def __init__(self, config_path):
        self.config = load_config(config_path)

    def load_data(self, path):
        return pd.read_csv(path, **self.config.data_loading_params)

    def preprocess(self, df):
        pipeline = Pipeline([
            ('imputer', SimpleImputer(strategy='median')),
            ('scaler', StandardScaler()),
            ('encoder', OneHotEncoder(handle_unknown='ignore'))
        ])
        return pipeline.fit_transform(df)

class ModelTrainer:
    def __init__(self, config_path):
        self.config = load_config(config_path)
        self.experiment = mlflow.set_experiment(self.config.experiment_name)

    def train(self, X, y):
        with mlflow.start_run():
            model = XGBClassifier(**self.config.model_params)
            model.fit(X, y)

            mlflow.log_params(self.config.model_params)
            mlflow.sklearn.log_model(model, "model")

            return model
```

### 1.6 The MLOps Maturity Model

**Content:**
- Level 0: Manual process
- Level 1: ML pipeline automation
- Level 2: CI/CD pipeline automation
- Level 3: Full MLOps automation
- Assessment framework

**Practical Example:**
- Company maturity assessment
- Roadmap from Level 1 to Level 3

**Table 1.2: MLOps Maturity Levels**
```
Level 0: Manual
- Manual data analysis
- Jupyter notebooks
- Manual deployment
- No monitoring

Level 1: DevOps, No MLOps
- Source control
- Unit tests
- CI/CD for code
- Manual ML steps

Level 2: Automated Training
- Data pipeline automation
- Model training automation
- Automated model validation
- Basic monitoring

Level 3: Automated Deployment
- CI/CD for ML
- A/B testing
- Automated retraining
- Comprehensive monitoring

Level 4: Full MLOps Automation
- Automated feature engineering
- AutoML integration
- Continuous training
- Advanced monitoring & alerting
- Automated model governance
```

### 1.7 Building an ML Engineering Mindset

**Content:**
- Systems thinking for ML
- Reliability engineering principles
- Performance optimization mindset
- Balancing trade-offs (accuracy vs latency vs cost)

**Practical Example:**
- Decision framework for trade-offs
- Real scenarios with solution analysis

**Code Listing 1.5: Trade-off Analysis**
```python
class ModelSelector:
    """Select model based on production constraints"""

    def __init__(self, constraints):
        self.max_latency_ms = constraints.get('max_latency_ms', 100)
        self.max_memory_mb = constraints.get('max_memory_mb', 512)
        self.min_accuracy = constraints.get('min_accuracy', 0.90)
        self.max_monthly_cost = constraints.get('max_monthly_cost', 1000)

    def evaluate_candidates(self, models):
        results = []
        for model_name, model in models.items():
            # Benchmark
            latency = self.benchmark_latency(model)
            memory = self.estimate_memory(model)
            accuracy = self.cross_validate(model)
            cost = self.estimate_cost(model, expected_qps=100)

            # Check constraints
            meets_constraints = (
                latency <= self.max_latency_ms and
                memory <= self.max_memory_mb and
                accuracy >= self.min_accuracy and
                cost <= self.max_monthly_cost
            )

            results.append({
                'model': model_name,
                'latency_ms': latency,
                'memory_mb': memory,
                'accuracy': accuracy,
                'monthly_cost': cost,
                'meets_constraints': meets_constraints,
                'score': self.composite_score(accuracy, latency, cost)
            })

        return pd.DataFrame(results).sort_values('score', ascending=False)

    def composite_score(self, accuracy, latency, cost):
        # Higher is better
        # Normalize and weight
        accuracy_score = accuracy  # 0-1
        latency_score = max(0, 1 - (latency / self.max_latency_ms))
        cost_score = max(0, 1 - (cost / self.max_monthly_cost))

        # Weighted average
        return 0.5 * accuracy_score + 0.3 * latency_score + 0.2 * cost_score
```

**Exercises:**
1. Compare a Jupyter notebook approach vs production pipeline for a given ML task
2. Create an MLOps maturity assessment for your organization
3. Build a trade-off analysis framework for model selection
4. Identify and refactor technical debt in an existing ML codebase

---

## Chapter 2: Data Pipeline Architecture

### 2.1 Data Pipeline Fundamentals

**Content:**
- ETL vs ELT paradigms
- Batch vs streaming data
- Data pipeline patterns (Lambda, Kappa architectures)
- Push vs pull data ingestion

**Practical Example:**
- E-commerce clickstream pipeline
- IoT sensor data pipeline

**Code Listing 2.1: Simple Batch ETL Pipeline**
```python
from datetime import datetime, timedelta
from typing import Dict, List
import pandas as pd
import logging

class BatchETLPipeline:
    """Simple batch ETL pipeline for daily processing"""

    def __init__(self, config: Dict):
        self.config = config
        self.logger = logging.getLogger(__name__)

    def extract(self, date: datetime) -> pd.DataFrame:
        """Extract data from source for specific date"""
        self.logger.info(f"Extracting data for {date}")

        # Example: Read from S3
        path = f"s3://bucket/data/date={date.strftime('%Y-%m-%d')}/"
        df = pd.read_parquet(path)

        self.logger.info(f"Extracted {len(df)} records")
        return df

    def transform(self, df: pd.DataFrame) -> pd.DataFrame:
        """Apply transformations"""
        self.logger.info("Starting transformations")

        # Data quality checks
        initial_count = len(df)
        df = df.dropna(subset=['user_id', 'timestamp'])
        dropped = initial_count - len(df)
        self.logger.info(f"Dropped {dropped} invalid records")

        # Feature engineering
        df['hour'] = pd.to_datetime(df['timestamp']).dt.hour
        df['day_of_week'] = pd.to_datetime(df['timestamp']).dt.dayofweek

        # Aggregations
        df_agg = df.groupby('user_id').agg({
            'event': 'count',
            'value': 'sum',
            'hour': 'first'
        }).reset_index()

        return df_agg

    def load(self, df: pd.DataFrame, date: datetime):
        """Load data to destination"""
        self.logger.info(f"Loading {len(df)} records")

        # Example: Write to data warehouse
        table_name = "user_daily_features"
        df.to_sql(
            table_name,
            con=self.config['db_connection'],
            if_exists='append',
            index=False
        )

        self.logger.info("Load complete")

    def run(self, date: datetime):
        """Execute full pipeline"""
        try:
            df = self.extract(date)
            df_transformed = self.transform(df)
            self.load(df_transformed, date)
            self.logger.info("Pipeline completed successfully")
        except Exception as e:
            self.logger.error(f"Pipeline failed: {e}")
            raise
```

### 2.2 Data Ingestion Strategies

**Content:**
- Real-time vs batch ingestion
- Change Data Capture (CDC)
- API-based ingestion
- File-based ingestion
- Database replication

**Practical Example:**
- Multi-source data ingestion architecture
- Kafka-based streaming ingestion

**Code Listing 2.2: Streaming Ingestion with Kafka**
```python
from kafka import KafkaConsumer, KafkaProducer
from confluent_kafka import Consumer, Producer
import json
from typing import Callable

class StreamingDataIngestion:
    """Real-time data ingestion from Kafka"""

    def __init__(self, config: Dict):
        self.config = config
        self.consumer = KafkaConsumer(
            config['topic'],
            bootstrap_servers=config['bootstrap_servers'],
            value_deserializer=lambda m: json.loads(m.decode('utf-8')),
            auto_offset_reset='latest',
            enable_auto_commit=True,
            group_id=config['consumer_group']
        )

    def process_message(self, message: Dict) -> Dict:
        """Process individual message"""
        # Validate schema
        required_fields = ['user_id', 'event_type', 'timestamp']
        if not all(field in message for field in required_fields):
            raise ValueError(f"Missing required fields in message: {message}")

        # Enrich data
        message['processed_at'] = datetime.utcnow().isoformat()
        message['date'] = datetime.fromisoformat(
            message['timestamp']
        ).strftime('%Y-%m-%d')

        return message

    def batch_write(self, messages: List[Dict], batch_size: int = 1000):
        """Write messages in batches"""
        df = pd.DataFrame(messages)

        # Write to data lake
        path = f"s3://bucket/streaming/date={df['date'].iloc[0]}/"
        df.to_parquet(path, index=False)

    def consume(self, callback: Callable = None):
        """Consume messages from Kafka"""
        batch = []

        try:
            for message in self.consumer:
                processed = self.process_message(message.value)
                batch.append(processed)

                # Execute callback if provided
                if callback:
                    callback(processed)

                # Batch write
                if len(batch) >= 1000:
                    self.batch_write(batch)
                    batch = []

        except KeyboardInterrupt:
            print("Stopping consumer...")
        finally:
            if batch:
                self.batch_write(batch)
            self.consumer.close()
```

### 2.3 Data Validation and Quality Checks

**Content:**
- Data validation frameworks
- Schema validation
- Statistical checks
- Anomaly detection in data
- Great Expectations framework

**Practical Example:**
- Building a comprehensive validation suite
- Automated data quality monitoring

**Code Listing 2.3: Data Validation with Great Expectations**
```python
import great_expectations as ge
from great_expectations.core import ExpectationSuite
from great_expectations.checkpoint import SimpleCheckpoint
from typing import Dict, List

class DataValidator:
    """Comprehensive data validation using Great Expectations"""

    def __init__(self, context_root_dir: str):
        self.context = ge.data_context.DataContext(context_root_dir)

    def create_expectation_suite(
        self,
        suite_name: str,
        df: pd.DataFrame
    ) -> ExpectationSuite:
        """Create expectation suite for dataset"""

        # Convert to GE DataFrame
        ge_df = ge.from_pandas(df)

        # Table-level expectations
        ge_df.expect_table_row_count_to_be_between(
            min_value=1000,
            max_value=10000000
        )

        ge_df.expect_table_column_count_to_equal(
            value=len(df.columns)
        )

        # Column-level expectations
        # User ID should be unique and not null
        ge_df.expect_column_values_to_be_unique('user_id')
        ge_df.expect_column_values_to_not_be_null('user_id')

        # Timestamps should be recent
        ge_df.expect_column_values_to_be_between(
            'timestamp',
            min_value=(datetime.now() - timedelta(days=7)).isoformat(),
            max_value=datetime.now().isoformat(),
            parse_strings_as_datetimes=True
        )

        # Numerical columns
        ge_df.expect_column_values_to_be_between(
            'age',
            min_value=18,
            max_value=100
        )

        # Categorical columns
        ge_df.expect_column_values_to_be_in_set(
            'country',
            value_set=['US', 'UK', 'CA', 'AU', 'DE', 'FR']
        )

        # Statistical expectations
        ge_df.expect_column_mean_to_be_between(
            'purchase_amount',
            min_value=10,
            max_value=500
        )

        ge_df.expect_column_stdev_to_be_between(
            'purchase_amount',
            min_value=5,
            max_value=200
        )

        # Save suite
        suite = ge_df.get_expectation_suite()
        self.context.save_expectation_suite(suite, suite_name)

        return suite

    def validate_data(
        self,
        df: pd.DataFrame,
        suite_name: str
    ) -> Dict:
        """Validate data against expectation suite"""

        # Get suite
        suite = self.context.get_expectation_suite(suite_name)

        # Create batch
        batch = ge.dataset.PandasDataset(df, expectation_suite=suite)

        # Run validation
        results = batch.validate()

        # Parse results
        validation_results = {
            'success': results.success,
            'statistics': results.statistics,
            'results_summary': {
                'total': results.statistics['evaluated_expectations'],
                'successful': results.statistics['successful_expectations'],
                'failed': results.statistics['unsuccessful_expectations'],
                'success_rate': results.statistics['success_percent']
            },
            'failed_expectations': [
                {
                    'expectation_type': r.expectation_config.expectation_type,
                    'kwargs': r.expectation_config.kwargs,
                    'result': r.result
                }
                for r in results.results
                if not r.success
            ]
        }

        return validation_results

    def validate_with_checkpoint(
        self,
        checkpoint_name: str,
        batch_request: Dict
    ):
        """Run validation using checkpoint"""

        checkpoint = SimpleCheckpoint(
            checkpoint_name,
            self.context,
            batch_request=batch_request
        )

        results = checkpoint.run()

        return results
```

### 2.4 Data Preprocessing and Transformation

**Content:**
- Scalable preprocessing strategies
- Transformation best practices
- Handling missing data
- Feature scaling and normalization
- Encoding categorical variables

**Practical Example:**
- Building reusable transformation pipelines
- Handling different data types

**Code Listing 2.4: Modular Preprocessing Pipeline**
```python
from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.impute import SimpleImputer
import numpy as np

class DateFeatureExtractor(BaseEstimator, TransformerMixin):
    """Extract features from datetime columns"""

    def __init__(self, date_columns: List[str]):
        self.date_columns = date_columns

    def fit(self, X, y=None):
        return self

    def transform(self, X):
        X = X.copy()

        for col in self.date_columns:
            if col in X.columns:
                X[col] = pd.to_datetime(X[col])
                X[f'{col}_year'] = X[col].dt.year
                X[f'{col}_month'] = X[col].dt.month
                X[f'{col}_day'] = X[col].dt.day
                X[f'{col}_dayofweek'] = X[col].dt.dayofweek
                X[f'{col}_hour'] = X[col].dt.hour
                X = X.drop(col, axis=1)

        return X

class OutlierClipper(BaseEstimator, TransformerMixin):
    """Clip outliers using IQR method"""

    def __init__(self, factor: float = 1.5):
        self.factor = factor
        self.lower_bounds = {}
        self.upper_bounds = {}

    def fit(self, X, y=None):
        for col in X.select_dtypes(include=[np.number]).columns:
            Q1 = X[col].quantile(0.25)
            Q3 = X[col].quantile(0.75)
            IQR = Q3 - Q1

            self.lower_bounds[col] = Q1 - (self.factor * IQR)
            self.upper_bounds[col] = Q3 + (self.factor * IQR)

        return self

    def transform(self, X):
        X = X.copy()

        for col in self.lower_bounds.keys():
            if col in X.columns:
                X[col] = X[col].clip(
                    lower=self.lower_bounds[col],
                    upper=self.upper_bounds[col]
                )

        return X

class DataPreprocessor:
    """Complete preprocessing pipeline"""

    def __init__(self, config: Dict):
        self.config = config
        self.pipeline = self._build_pipeline()

    def _build_pipeline(self) -> Pipeline:
        """Build sklearn pipeline"""

        # Define column types
        numeric_features = self.config['numeric_features']
        categorical_features = self.config['categorical_features']
        date_features = self.config.get('date_features', [])

        # Numeric pipeline
        numeric_transformer = Pipeline(steps=[
            ('imputer', SimpleImputer(strategy='median')),
            ('outlier_clipper', OutlierClipper()),
            ('scaler', StandardScaler())
        ])

        # Categorical pipeline
        categorical_transformer = Pipeline(steps=[
            ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
            ('onehot', OneHotEncoder(handle_unknown='ignore', sparse=False))
        ])

        # Combine transformers
        preprocessor = ColumnTransformer(
            transformers=[
                ('num', numeric_transformer, numeric_features),
                ('cat', categorical_transformer, categorical_features)
            ],
            remainder='drop'
        )

        # Full pipeline
        pipeline = Pipeline(steps=[
            ('date_features', DateFeatureExtractor(date_features)),
            ('preprocessor', preprocessor)
        ])

        return pipeline

    def fit(self, X, y=None):
        """Fit preprocessing pipeline"""
        self.pipeline.fit(X, y)
        return self

    def transform(self, X):
        """Transform data"""
        return self.pipeline.transform(X)

    def fit_transform(self, X, y=None):
        """Fit and transform data"""
        return self.pipeline.fit_transform(X, y)

    def save(self, path: str):
        """Save pipeline"""
        import joblib
        joblib.dump(self.pipeline, path)

    @classmethod
    def load(cls, path: str):
        """Load pipeline"""
        import joblib
        instance = cls(config={})
        instance.pipeline = joblib.load(path)
        return instance
```

### 2.5 Orchestration with Apache Airflow

**Content:**
- Airflow fundamentals (DAGs, operators, sensors)
- Task dependencies and scheduling
- Monitoring and alerting
- Best practices for DAG design

**Practical Example:**
- End-to-end ML data pipeline DAG
- Dynamic DAG generation

**Code Listing 2.5: Production Airflow DAG**
```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.operators.bash import BashOperator
from airflow.providers.amazon.aws.sensors.s3 import S3KeySensor
from airflow.providers.postgres.operators.postgres import PostgresOperator
from airflow.utils.dates import days_ago
from datetime import datetime, timedelta
import sys

# Add project to path
sys.path.insert(0, '/opt/airflow/dags/ml_pipeline')

from data_validation import DataValidator
from preprocessing import DataPreprocessor
from feature_engineering import FeatureEngineer

default_args = {
    'owner': 'ml_team',
    'depends_on_past': False,
    'start_date': days_ago(1),
    'email': ['ml-team@company.com'],
    'email_on_failure': True,
    'email_on_retry': False,
    'retries': 3,
    'retry_delay': timedelta(minutes=5),
    'execution_timeout': timedelta(hours=2),
}

def extract_data(**context):
    """Extract data from source"""
    import pandas as pd
    from sqlalchemy import create_engine

    execution_date = context['execution_date']
    date_str = execution_date.strftime('%Y-%m-%d')

    # Extract from database
    engine = create_engine('postgresql://user:pass@host:5432/db')
    query = f"""
        SELECT * FROM events
        WHERE DATE(timestamp) = '{date_str}'
    """
    df = pd.read_sql(query, engine)

    # Save to S3
    s3_path = f"s3://bucket/raw/date={date_str}/data.parquet"
    df.to_parquet(s3_path, index=False)

    # Push metadata to XCom
    context['task_instance'].xcom_push(
        key='record_count',
        value=len(df)
    )
    context['task_instance'].xcom_push(
        key='s3_path',
        value=s3_path
    )

def validate_data(**context):
    """Validate extracted data"""
    import pandas as pd

    # Get S3 path from XCom
    s3_path = context['task_instance'].xcom_pull(
        task_ids='extract_data',
        key='s3_path'
    )

    df = pd.read_parquet(s3_path)

    # Validate
    validator = DataValidator('/opt/airflow/ge')
    results = validator.validate_data(df, 'events_suite')

    if not results['success']:
        raise ValueError(
            f"Data validation failed: {results['failed_expectations']}"
        )

    print(f"Validation passed: {results['results_summary']}")

def preprocess_data(**context):
    """Preprocess data"""
    import pandas as pd

    execution_date = context['execution_date']
    date_str = execution_date.strftime('%Y-%m-%d')

    # Load raw data
    s3_path = context['task_instance'].xcom_pull(
        task_ids='extract_data',
        key='s3_path'
    )
    df = pd.read_parquet(s3_path)

    # Preprocess
    config = {
        'numeric_features': ['age', 'income', 'credit_score'],
        'categorical_features': ['country', 'occupation'],
        'date_features': ['signup_date']
    }

    preprocessor = DataPreprocessor(config)
    df_processed = preprocessor.fit_transform(df)

    # Save processed data
    processed_path = f"s3://bucket/processed/date={date_str}/data.parquet"
    df_processed.to_parquet(processed_path, index=False)

    # Save preprocessor
    preprocessor.save(f"s3://bucket/artifacts/preprocessor_{date_str}.pkl")

    context['task_instance'].xcom_push(
        key='processed_path',
        value=processed_path
    )

def engineer_features(**context):
    """Engineer features"""
    import pandas as pd

    execution_date = context['execution_date']
    date_str = execution_date.strftime('%Y-%m-%d')

    # Load processed data
    processed_path = context['task_instance'].xcom_pull(
        task_ids='preprocess_data',
        key='processed_path'
    )
    df = pd.read_parquet(processed_path)

    # Feature engineering
    engineer = FeatureEngineer()
    df_features = engineer.create_features(df)

    # Save features
    features_path = f"s3://bucket/features/date={date_str}/data.parquet"
    df_features.to_parquet(features_path, index=False)

    context['task_instance'].xcom_push(
        key='features_path',
        value=features_path
    )

with DAG(
    'ml_data_pipeline',
    default_args=default_args,
    description='ML training data pipeline',
    schedule_interval='@daily',
    catchup=False,
    max_active_runs=1,
    tags=['ml', 'data-pipeline'],
) as dag:

    # Wait for source data
    wait_for_data = S3KeySensor(
        task_id='wait_for_source_data',
        bucket_name='source-bucket',
        bucket_key='raw/{{ ds }}/*.csv',
        aws_conn_id='aws_default',
        timeout=3600,
        poke_interval=300,
    )

    # Extract data
    extract = PythonOperator(
        task_id='extract_data',
        python_callable=extract_data,
        provide_context=True,
    )

    # Validate data
    validate = PythonOperator(
        task_id='validate_data',
        python_callable=validate_data,
        provide_context=True,
    )

    # Preprocess data
    preprocess = PythonOperator(
        task_id='preprocess_data',
        python_callable=preprocess_data,
        provide_context=True,
    )

    # Engineer features
    features = PythonOperator(
        task_id='engineer_features',
        python_callable=engineer_features,
        provide_context=True,
    )

    # Update metadata table
    update_metadata = PostgresOperator(
        task_id='update_metadata',
        postgres_conn_id='warehouse',
        sql="""
            INSERT INTO pipeline_runs (
                date, pipeline_name, status, record_count, created_at
            ) VALUES (
                '{{ ds }}',
                'ml_data_pipeline',
                'success',
                {{ task_instance.xcom_pull(task_ids='extract_data', key='record_count') }},
                NOW()
            )
        """
    )

    # Trigger training pipeline
    trigger_training = BashOperator(
        task_id='trigger_training',
        bash_command='airflow dags trigger ml_training_pipeline --conf \'{"date": "{{ ds }}"}\''
    )

    # Define task dependencies
    wait_for_data >> extract >> validate >> preprocess >> features >> update_metadata >> trigger_training

# Monitoring callbacks
def on_failure_callback(context):
    """Send alert on DAG failure"""
    from airflow.providers.slack.operators.slack_webhook import SlackWebhookOperator

    slack_msg = f"""
    :red_circle: DAG Failed
    *DAG*: {context['dag'].dag_id}
    *Task*: {context['task_instance'].task_id}
    *Execution Date*: {context['execution_date']}
    *Log*: {context['task_instance'].log_url}
    """

    alert = SlackWebhookOperator(
        task_id='slack_alert',
        http_conn_id='slack_webhook',
        message=slack_msg,
    )
    alert.execute(context=context)
```

### 2.6 Data Versioning with DVC

**Content:**
- Data versioning fundamentals
- DVC architecture and workflow
- Integration with Git
- Remote storage configuration
- Reproducibility benefits

**Practical Example:**
- Versioning large datasets
- Tracking data lineage

**Code Listing 2.6: DVC Workflow**
```bash
# Initialize DVC in project
dvc init

# Configure remote storage
dvc remote add -d myremote s3://mybucket/dvc-storage
dvc remote modify myremote region us-east-1

# Track data file
dvc add data/raw/large_dataset.csv
# This creates data/raw/large_dataset.csv.dvc

# Commit DVC file to Git
git add data/raw/large_dataset.csv.dvc data/raw/.gitignore
git commit -m "Track large dataset with DVC"

# Push data to remote storage
dvc push

# Pull data on different machine
git clone <repo>
dvc pull

# Create data pipeline with DVC
# dvc.yaml
stages:
  extract:
    cmd: python src/extract_data.py
    deps:
      - src/extract_data.py
    outs:
      - data/raw/data.csv

  preprocess:
    cmd: python src/preprocess.py
    deps:
      - src/preprocess.py
      - data/raw/data.csv
    outs:
      - data/processed/data.csv

  feature_engineering:
    cmd: python src/features.py
    deps:
      - src/features.py
      - data/processed/data.csv
    outs:
      - data/features/features.csv
    metrics:
      - metrics/feature_stats.json

# Run pipeline
dvc repro

# Show pipeline
dvc dag

# Compare metrics across versions
dvc metrics diff HEAD~1 HEAD
```

### 2.7 Building Scalable Pipelines with Spark

**Content:**
- When to use Spark for data pipelines
- Spark architecture fundamentals
- PySpark for data transformation
- Optimization techniques
- Integration with cloud storage

**Practical Example:**
- Processing billions of records
- Distributed feature engineering

**Code Listing 2.7: PySpark Data Pipeline**
```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import (
    col, when, udf, pandas_udf,
    window, count, avg, sum as spark_sum
)
from pyspark.sql.types import DoubleType, StringType
import pyspark.sql.functions as F
from typing import Iterator
import pandas as pd

class SparkDataPipeline:
    """Scalable data pipeline using PySpark"""

    def __init__(self, app_name: str = "ML Data Pipeline"):
        self.spark = SparkSession.builder \
            .appName(app_name) \
            .config("spark.sql.adaptive.enabled", "true") \
            .config("spark.sql.adaptive.coalescePartitions.enabled", "true") \
            .config("spark.dynamicAllocation.enabled", "true") \
            .getOrCreate()

    def extract_data(self, paths: List[str]) -> DataFrame:
        """Extract data from multiple sources"""

        # Read parquet files with schema evolution
        df = self.spark.read \
            .option("mergeSchema", "true") \
            .parquet(*paths)

        return df

    def clean_data(self, df: DataFrame) -> DataFrame:
        """Clean and filter data"""

        # Remove duplicates
        df = df.dropDuplicates(['user_id', 'timestamp'])

        # Filter invalid records
        df = df.filter(
            (col('user_id').isNotNull()) &
            (col('timestamp').isNotNull()) &
            (col('value') > 0)
        )

        # Handle outliers
        stats = df.select(
            F.percentile_approx('value', [0.01, 0.99]).alias('percentiles')
        ).collect()[0]['percentiles']

        df = df.filter(
            (col('value') >= stats[0]) &
            (col('value') <= stats[1])
        )

        return df

    def feature_engineering(self, df: DataFrame) -> DataFrame:
        """Create features at scale"""

        # Time-based features
        df = df.withColumn('hour', F.hour('timestamp'))
        df = df.withColumn('day_of_week', F.dayofweek('timestamp'))
        df = df.withColumn('is_weekend',
            when(col('day_of_week').isin([1, 7]), 1).otherwise(0)
        )

        # Window functions for aggregations
        from pyspark.sql.window import Window

        # Last 7 days rolling average
        window_7d = Window \
            .partitionBy('user_id') \
            .orderBy(F.unix_timestamp('timestamp')) \
            .rangeBetween(-7*24*3600, 0)

        df = df.withColumn(
            'value_7d_avg',
            F.avg('value').over(window_7d)
        )

        df = df.withColumn(
            'value_7d_sum',
            spark_sum('value').over(window_7d)
        )

        # User-level aggregations
        user_stats = df.groupBy('user_id').agg(
            count('*').alias('total_events'),
            avg('value').alias('avg_value'),
            spark_sum('value').alias('total_value'),
            F.stddev('value').alias('stddev_value')
        )

        # Join back
        df = df.join(user_stats, on='user_id', how='left')

        return df

    @pandas_udf(DoubleType())
    def complex_transformation(pdf_iter: Iterator[pd.DataFrame]) -> Iterator[pd.Series]:
        """Pandas UDF for complex transformations"""
        for pdf in pdf_iter:
            # Apply complex pandas operations
            result = pdf['value'].apply(lambda x: custom_transform(x))
            yield result

    def write_features(
        self,
        df: DataFrame,
        output_path: str,
        partition_cols: List[str] = ['date']
    ):
        """Write features to storage"""

        # Optimize partitioning
        df.write \
            .mode('overwrite') \
            .partitionBy(*partition_cols) \
            .parquet(output_path)

    def run_pipeline(
        self,
        input_paths: List[str],
        output_path: str
    ):
        """Execute complete pipeline"""

        # Extract
        print("Extracting data...")
        df = self.extract_data(input_paths)
        print(f"Extracted {df.count():,} records")

        # Clean
        print("Cleaning data...")
        df_clean = self.clean_data(df)
        print(f"After cleaning: {df_clean.count():,} records")

        # Feature engineering
        print("Engineering features...")
        df_features = self.feature_engineering(df_clean)

        # Write
        print(f"Writing features to {output_path}")
        self.write_features(df_features, output_path)

        print("Pipeline complete!")

        # Show sample
        df_features.show(10)

        # Statistics
        df_features.describe().show()

# Usage
if __name__ == "__main__":
    pipeline = SparkDataPipeline()

    input_paths = [
        "s3://bucket/raw/2024-01-*/",
        "s3://bucket/raw/2024-02-*/"
    ]

    output_path = "s3://bucket/features/v1/"

    pipeline.run_pipeline(input_paths, output_path)
```

**Exercises:**
1. Build an ETL pipeline for a multi-source data integration problem
2. Implement a data validation suite using Great Expectations for your dataset
3. Create an Airflow DAG for a complete ML data pipeline with error handling
4. Design and implement a DVC workflow for version tracking dataset evolution
5. Build a PySpark pipeline to process 1B+ records with feature engineering

---

## Chapter 3: Training Pipeline Design Patterns

### 3.1 Training Pipeline Architecture

**Content:**
- Pipeline components and stages
- Modular pipeline design
- Configuration management
- Experiment tracking integration
- Pipeline orchestration patterns

**Practical Example:**
- End-to-end training pipeline architecture
- Microservices vs monolithic pipeline design

**Code Listing 3.1: Modular Training Pipeline**
```python
from dataclasses import dataclass
from typing import Dict, Any, Optional, Tuple
import mlflow
import yaml
from pathlib import Path
import logging

@dataclass
class PipelineConfig:
    """Configuration for training pipeline"""
    experiment_name: str
    model_type: str
    data_path: str
    output_path: str
    hyperparameters: Dict[str, Any]
    validation_split: float = 0.2
    random_state: int = 42

    @classmethod
    def from_yaml(cls, path: str):
        """Load configuration from YAML file"""
        with open(path, 'r') as f:
            config_dict = yaml.safe_load(f)
        return cls(**config_dict)

class TrainingPipeline:
    """Modular ML training pipeline"""

    def __init__(self, config: PipelineConfig):
        self.config = config
        self.logger = self._setup_logging()
        self.mlflow_run = None

    def _setup_logging(self) -> logging.Logger:
        """Setup logging"""
        logging.basicConfig(
            level=logging.INFO,
            format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
        )
        return logging.getLogger(__name__)

    def load_data(self) -> Tuple[pd.DataFrame, pd.DataFrame]:
        """Load and split data"""
        self.logger.info(f"Loading data from {self.config.data_path}")

        df = pd.read_parquet(self.config.data_path)

        # Split into train/val
        from sklearn.model_selection import train_test_split
        train_df, val_df = train_test_split(
            df,
            test_size=self.config.validation_split,
            random_state=self.config.random_state,
            stratify=df['target'] if 'target' in df.columns else None
        )

        self.logger.info(
            f"Data loaded: {len(train_df)} train, {len(val_df)} validation"
        )

        return train_df, val_df

    def preprocess(
        self,
        train_df: pd.DataFrame,
        val_df: pd.DataFrame
    ) -> Tuple[np.ndarray, np.ndarray, np.ndarray, np.ndarray]:
        """Preprocess data"""
        self.logger.info("Preprocessing data")

        # Separate features and target
        X_train = train_df.drop('target', axis=1)
        y_train = train_df['target']
        X_val = val_df.drop('target', axis=1)
        y_val = val_df['target']

        # Load or create preprocessor
        preprocessor_path = Path(self.config.output_path) / 'preprocessor.pkl'

        if preprocessor_path.exists():
            self.logger.info("Loading existing preprocessor")
            import joblib
            self.preprocessor = joblib.load(preprocessor_path)
            X_train_processed = self.preprocessor.transform(X_train)
            X_val_processed = self.preprocessor.transform(X_val)
        else:
            self.logger.info("Creating new preprocessor")
            from data_preprocessing import DataPreprocessor

            self.preprocessor = DataPreprocessor(self.config.preprocessing_config)
            X_train_processed = self.preprocessor.fit_transform(X_train)
            X_val_processed = self.preprocessor.transform(X_val)

            # Save preprocessor
            import joblib
            preprocessor_path.parent.mkdir(parents=True, exist_ok=True)
            joblib.dump(self.preprocessor, preprocessor_path)

            self.logger.info(f"Preprocessor saved to {preprocessor_path}")

        return X_train_processed, X_val_processed, y_train.values, y_val.values

    def train_model(
        self,
        X_train: np.ndarray,
        y_train: np.ndarray
    ) -> Any:
        """Train model"""
        self.logger.info(f"Training {self.config.model_type} model")

        # Get model class
        model_class = self._get_model_class(self.config.model_type)

        # Initialize model
        model = model_class(**self.config.hyperparameters)

        # Train
        model.fit(X_train, y_train)

        self.logger.info("Model training complete")

        return model

    def evaluate_model(
        self,
        model: Any,
        X_val: np.ndarray,
        y_val: np.ndarray
    ) -> Dict[str, float]:
        """Evaluate model"""
        self.logger.info("Evaluating model")

        from sklearn.metrics import (
            accuracy_score, precision_score, recall_score,
            f1_score, roc_auc_score, classification_report
        )

        # Predictions
        y_pred = model.predict(X_val)
        y_pred_proba = model.predict_proba(X_val)[:, 1] if hasattr(model, 'predict_proba') else None

        # Calculate metrics
        metrics = {
            'accuracy': accuracy_score(y_val, y_pred),
            'precision': precision_score(y_val, y_pred, average='binary'),
            'recall': recall_score(y_val, y_pred, average='binary'),
            'f1': f1_score(y_val, y_pred, average='binary'),
        }

        if y_pred_proba is not None:
            metrics['roc_auc'] = roc_auc_score(y_val, y_pred_proba)

        # Log detailed report
        report = classification_report(y_val, y_pred)
        self.logger.info(f"\nClassification Report:\n{report}")

        # Log metrics
        for metric_name, metric_value in metrics.items():
            self.logger.info(f"{metric_name}: {metric_value:.4f}")

        return metrics

    def save_model(self, model: Any):
        """Save model"""
        model_path = Path(self.config.output_path) / 'model.pkl'
        model_path.parent.mkdir(parents=True, exist_ok=True)

        import joblib
        joblib.dump(model, model_path)

        self.logger.info(f"Model saved to {model_path}")

        return str(model_path)

    def log_to_mlflow(
        self,
        model: Any,
        metrics: Dict[str, float],
        model_path: str
    ):
        """Log experiment to MLflow"""
        self.logger.info("Logging to MLflow")

        mlflow.set_experiment(self.config.experiment_name)

        with mlflow.start_run() as run:
            self.mlflow_run = run

            # Log parameters
            mlflow.log_params(self.config.hyperparameters)
            mlflow.log_param("model_type", self.config.model_type)
            mlflow.log_param("data_path", self.config.data_path)

            # Log metrics
            mlflow.log_metrics(metrics)

            # Log model
            mlflow.sklearn.log_model(model, "model")

            # Log artifacts
            mlflow.log_artifact(model_path)

            # Log config
            config_path = Path(self.config.output_path) / 'config.yaml'
            with open(config_path, 'w') as f:
                yaml.dump(self.config.__dict__, f)
            mlflow.log_artifact(str(config_path))

            self.logger.info(f"MLflow run ID: {run.info.run_id}")

    def run(self) -> Dict[str, Any]:
        """Execute complete pipeline"""
        try:
            self.logger.info("Starting training pipeline")

            # Load data
            train_df, val_df = self.load_data()

            # Preprocess
            X_train, X_val, y_train, y_val = self.preprocess(train_df, val_df)

            # Train
            model = self.train_model(X_train, y_train)

            # Evaluate
            metrics = self.evaluate_model(model, X_val, y_val)

            # Save
            model_path = self.save_model(model)

            # Log to MLflow
            self.log_to_mlflow(model, metrics, model_path)

            self.logger.info("Pipeline completed successfully")

            return {
                'status': 'success',
                'metrics': metrics,
                'model_path': model_path,
                'mlflow_run_id': self.mlflow_run.info.run_id if self.mlflow_run else None
            }

        except Exception as e:
            self.logger.error(f"Pipeline failed: {str(e)}", exc_info=True)
            return {
                'status': 'failed',
                'error': str(e)
            }

    @staticmethod
    def _get_model_class(model_type: str):
        """Get model class by name"""
        from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
        from sklearn.linear_model import LogisticRegression
        from xgboost import XGBClassifier

        models = {
            'random_forest': RandomForestClassifier,
            'gradient_boosting': GradientBoostingClassifier,
            'logistic_regression': LogisticRegression,
            'xgboost': XGBClassifier,
        }

        if model_type not in models:
            raise ValueError(f"Unknown model type: {model_type}")

        return models[model_type]

# Usage
if __name__ == "__main__":
    # Load configuration
    config = PipelineConfig.from_yaml('config/training_config.yaml')

    # Run pipeline
    pipeline = TrainingPipeline(config)
    results = pipeline.run()

    print(results)
```

**Config File Example (config/training_config.yaml):**
```yaml
experiment_name: "customer_churn_prediction"
model_type: "xgboost"
data_path: "s3://bucket/features/2024-01-01/data.parquet"
output_path: "models/churn_model_v1"
validation_split: 0.2
random_state: 42

hyperparameters:
  n_estimators: 100
  max_depth: 6
  learning_rate: 0.1
  subsample: 0.8
  colsample_bytree: 0.8

preprocessing_config:
  numeric_features:
    - age
    - income
    - account_age_days
  categorical_features:
    - country
    - plan_type
  date_features:
    - last_login_date
```

### 3.2 Configuration Management

**Content:**
- Configuration best practices
- Environment-specific configs
- Secrets management
- Configuration validation
- Hydra framework for complex configs

**Practical Example:**
- Multi-environment configuration setup
- Hierarchical configuration with Hydra

**Code Listing 3.2: Advanced Configuration with Hydra**
```python
from hydra import compose, initialize
from hydra.core.config_store import ConfigStore
from dataclasses import dataclass, field
from typing import List, Dict, Any
from omegaconf import OmegaConf, MISSING
import hydra

@dataclass
class DataConfig:
    """Data configuration"""
    train_path: str = MISSING
    val_path: str = MISSING
    test_path: str = MISSING
    features: List[str] = field(default_factory=list)
    target: str = "target"

@dataclass
class ModelConfig:
    """Model configuration"""
    type: str = "xgboost"
    params: Dict[str, Any] = field(default_factory=dict)

@dataclass
class TrainingConfig:
    """Training configuration"""
    epochs: int = 100
    batch_size: int = 32
    learning_rate: float = 0.001
    early_stopping_patience: int = 10

@dataclass
class MLFlowConfig:
    """MLflow configuration"""
    tracking_uri: str = "http://localhost:5000"
    experiment_name: str = MISSING
    run_name: str = "${now:%Y-%m-%d_%H-%M-%S}"

@dataclass
class Config:
    """Main configuration"""
    data: DataConfig = field(default_factory=DataConfig)
    model: ModelConfig = field(default_factory=ModelConfig)
    training: TrainingConfig = field(default_factory=TrainingConfig)
    mlflow: MLFlowConfig = field(default_factory=MLFlowConfig)

    # Environment
    env: str = "dev"
    debug: bool = False
    seed: int = 42

# Register configurations
cs = ConfigStore.instance()
cs.store(name="config", node=Config)

@hydra.main(version_base=None, config_path="conf", config_name="config")
def train(cfg: Config) -> None:
    """Training with Hydra config"""

    # Print configuration
    print(OmegaConf.to_yaml(cfg))

    # Access configuration
    print(f"Model type: {cfg.model.type}")
    print(f"Learning rate: {cfg.training.learning_rate}")

    # Run training pipeline
    pipeline = TrainingPipeline(cfg)
    results = pipeline.run()

    return results

if __name__ == "__main__":
    train()
```

**Directory Structure:**
```
conf/
├── config.yaml                 # Base config
├── data/
│   ├── dev.yaml               # Dev data config
│   ├── staging.yaml           # Staging data config
│   └── prod.yaml              # Prod data config
├── model/
│   ├── xgboost.yaml           # XGBoost config
│   ├── random_forest.yaml     # Random Forest config
│   └── neural_net.yaml        # Neural Net config
└── training/
    ├── fast.yaml              # Quick training
    └── full.yaml              # Full training
```

**Example config.yaml:**
```yaml
defaults:
  - data: dev
  - model: xgboost
  - training: fast
  - _self_

env: dev
debug: true
seed: 42

mlflow:
  tracking_uri: http://localhost:5000
  experiment_name: ${model.type}_${env}
```

**Running with overrides:**
```bash
# Development training
python train.py

# Production training with different model
python train.py env=prod model=random_forest training=full

# Override specific parameters
python train.py model.params.n_estimators=500 training.learning_rate=0.01
```

### 3.3 Experiment Tracking and Versioning

**Content:**
- MLflow experiment tracking
- Model versioning strategies
- Artifact management
- Comparing experiments
- Best practices for reproducibility

**Practical Example:**
- Complete MLflow integration
- Experiment comparison and analysis

**Code Listing 3.3: Advanced MLflow Integration**
```python
import mlflow
from mlflow.tracking import MlflowClient
from mlflow.models.signature import infer_signature
from typing import Dict, Any, List
import pandas as pd
import numpy as np
from sklearn.metrics import get_scorer

class MLflowExperimentTracker:
    """Advanced MLflow experiment tracking"""

    def __init__(
        self,
        tracking_uri: str,
        experiment_name: str,
        artifact_location: str = None
    ):
        mlflow.set_tracking_uri(tracking_uri)
        self.client = MlflowClient()

        # Get or create experiment
        experiment = self.client.get_experiment_by_name(experiment_name)
        if experiment is None:
            experiment_id = self.client.create_experiment(
                experiment_name,
                artifact_location=artifact_location
            )
        else:
            experiment_id = experiment.experiment_id

        self.experiment_id = experiment_id
        mlflow.set_experiment(experiment_name)

    def start_run(
        self,
        run_name: str = None,
        tags: Dict[str, str] = None
    ):
        """Start MLflow run"""
        return mlflow.start_run(
            experiment_id=self.experiment_id,
            run_name=run_name,
            tags=tags
        )

    def log_parameters(self, params: Dict[str, Any]):
        """Log parameters"""
        # Flatten nested dicts
        flat_params = self._flatten_dict(params)
        mlflow.log_params(flat_params)

    def log_metrics(
        self,
        metrics: Dict[str, float],
        step: int = None
    ):
        """Log metrics"""
        mlflow.log_metrics(metrics, step=step)

    def log_model(
        self,
        model: Any,
        artifact_path: str,
        X_sample: pd.DataFrame = None,
        registered_model_name: str = None
    ):
        """Log model with signature"""

        # Infer signature if sample provided
        signature = None
        if X_sample is not None:
            predictions = model.predict(X_sample)
            signature = infer_signature(X_sample, predictions)

        # Log model
        mlflow.sklearn.log_model(
            model,
            artifact_path,
            signature=signature,
            registered_model_name=registered_model_name
        )

    def log_dataset(
        self,
        df: pd.DataFrame,
        name: str,
        targets: pd.Series = None
    ):
        """Log dataset as MLflow dataset"""
        from mlflow.data.pandas_dataset import PandasDataset

        dataset = PandasDataset(
            df,
            targets=targets,
            name=name
        )
        mlflow.log_input(dataset)

    def log_feature_importance(
        self,
        model: Any,
        feature_names: List[str]
    ):
        """Log feature importance"""
        import matplotlib.pyplot as plt

        if hasattr(model, 'feature_importances_'):
            importances = model.feature_importances_

            # Create dataframe
            feat_imp_df = pd.DataFrame({
                'feature': feature_names,
                'importance': importances
            }).sort_values('importance', ascending=False)

            # Log as table
            mlflow.log_table(feat_imp_df, "feature_importances.json")

            # Create plot
            plt.figure(figsize=(10, 6))
            plt.barh(
                feat_imp_df['feature'][:20],
                feat_imp_df['importance'][:20]
            )
            plt.xlabel('Importance')
            plt.title('Top 20 Feature Importances')
            plt.tight_layout()

            # Log plot
            mlflow.log_figure(plt.gcf(), "feature_importance.png")
            plt.close()

    def log_confusion_matrix(
        self,
        y_true: np.ndarray,
        y_pred: np.ndarray,
        labels: List[str] = None
    ):
        """Log confusion matrix"""
        from sklearn.metrics import confusion_matrix
        import seaborn as sns
        import matplotlib.pyplot as plt

        cm = confusion_matrix(y_true, y_pred)

        plt.figure(figsize=(8, 6))
        sns.heatmap(
            cm,
            annot=True,
            fmt='d',
            cmap='Blues',
            xticklabels=labels,
            yticklabels=labels
        )
        plt.ylabel('True')
        plt.xlabel('Predicted')
        plt.title('Confusion Matrix')

        mlflow.log_figure(plt.gcf(), "confusion_matrix.png")
        plt.close()

    def log_roc_curve(
        self,
        y_true: np.ndarray,
        y_pred_proba: np.ndarray
    ):
        """Log ROC curve"""
        from sklearn.metrics import roc_curve, auc
        import matplotlib.pyplot as plt

        fpr, tpr, _ = roc_curve(y_true, y_pred_proba)
        roc_auc = auc(fpr, tpr)

        plt.figure(figsize=(8, 6))
        plt.plot(fpr, tpr, label=f'ROC curve (AUC = {roc_auc:.2f})')
        plt.plot([0, 1], [0, 1], 'k--', label='Random')
        plt.xlim([0.0, 1.0])
        plt.ylim([0.0, 1.05])
        plt.xlabel('False Positive Rate')
        plt.ylabel('True Positive Rate')
        plt.title('Receiver Operating Characteristic')
        plt.legend(loc="lower right")

        mlflow.log_figure(plt.gcf(), "roc_curve.png")
        plt.close()

    def compare_runs(
        self,
        run_ids: List[str],
        metric_names: List[str]
    ) -> pd.DataFrame:
        """Compare multiple runs"""

        results = []
        for run_id in run_ids:
            run = self.client.get_run(run_id)

            row = {
                'run_id': run_id,
                'run_name': run.data.tags.get('mlflow.runName', ''),
                'start_time': run.info.start_time,
            }

            # Add metrics
            for metric_name in metric_names:
                row[metric_name] = run.data.metrics.get(metric_name, None)

            # Add key parameters
            for param_name, param_value in run.data.params.items():
                row[f'param_{param_name}'] = param_value

            results.append(row)

        return pd.DataFrame(results)

    def get_best_run(
        self,
        metric_name: str,
        ascending: bool = False
    ):
        """Get best run based on metric"""

        runs = self.client.search_runs(
            experiment_ids=[self.experiment_id],
            order_by=[f"metrics.{metric_name} {'ASC' if ascending else 'DESC'}"]
        )

        if runs:
            return runs[0]
        return None

    @staticmethod
    def _flatten_dict(d: Dict, parent_key: str = '', sep: str = '.') -> Dict:
        """Flatten nested dictionary"""
        items = []
        for k, v in d.items():
            new_key = f"{parent_key}{sep}{k}" if parent_key else k
            if isinstance(v, dict):
                items.extend(
                    MLflowExperimentTracker._flatten_dict(v, new_key, sep=sep).items()
                )
            else:
                items.append((new_key, v))
        return dict(items)

# Usage example
def train_and_track():
    tracker = MLflowExperimentTracker(
        tracking_uri="http://localhost:5000",
        experiment_name="customer_churn",
        artifact_location="s3://bucket/mlflow-artifacts"
    )

    with tracker.start_run(
        run_name="xgboost_baseline",
        tags={
            "team": "ml-team",
            "project": "churn-prediction",
            "version": "v1.0"
        }
    ):
        # Log parameters
        params = {
            "model": {
                "type": "xgboost",
                "n_estimators": 100,
                "max_depth": 6
            },
            "preprocessing": {
                "scaler": "standard",
                "imputer": "median"
            }
        }
        tracker.log_parameters(params)

        # Train model
        model = train_model()

        # Evaluate
        metrics = evaluate_model(model, X_val, y_val)
        tracker.log_metrics(metrics)

        # Log model
        tracker.log_model(
            model,
            "model",
            X_sample=X_val[:100],
            registered_model_name="churn_predictor"
        )

        # Log visualizations
        tracker.log_feature_importance(model, feature_names)
        tracker.log_confusion_matrix(y_val, y_pred, labels=['No Churn', 'Churn'])
        tracker.log_roc_curve(y_val, y_pred_proba)

        # Log dataset
        tracker.log_dataset(X_val, "validation_set", targets=y_val)
```

**Continuing in next message...**
