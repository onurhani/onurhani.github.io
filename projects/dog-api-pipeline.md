# Dog API Pipeline

**Stack:** dlt · Apache Airflow · DuckDB · Python

**Repo:** [github.com/onurhani/dog-api-pipeline](https://github.com/onurhani/dog-api-pipeline)

---

A production-style data ingestion pipeline that pulls data from the [Dog API](https://dog.ceo/dog-api/) and loads it into DuckDB using dlt (data load tool), orchestrated with Apache Airflow.

**What it does:**

- Ingests breed data and image metadata from the Dog API
- Uses dlt for schema inference, incremental loading, and data normalisation
- Orchestrated via Airflow DAGs with proper scheduling and retry logic
- Stores results in DuckDB for lightweight, fast local analytics

**Why I built it:**

dlt makes the extraction and loading layer genuinely easy — schema inference, incremental state, and normalisation come for free. Pairing it with Airflow gives you proper orchestration without overengineering. This project was about showing what a clean, minimal ingestion pipeline looks like in the modern data stack.
