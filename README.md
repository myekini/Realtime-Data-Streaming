# Realtime Data Streaming | End-to-End Data Engineering Project

[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen)](https://github.com/airscholar/e2e-data-engineering)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![GitHub Issues](https://img.shields.io/github/issues/airscholar/e2e-data-engineering.svg)](https://github.com/airscholar/e2e-data-engineering/issues)

## 🚀 Overview

This project serves as a comprehensive guide to building an end-to-end data engineering pipeline. It covers each stage from data ingestion to processing and finally to storage, utilizing a robust tech stack that includes Apache Airflow, Python, Apache Kafka, Apache Zookeeper, Apache Spark, and Cassandra. Everything is containerized using Docker for ease of deployment and scalability.

## 📋 Table of Contents

- [Realtime Data Streaming | End-to-End Data Engineering Project](#realtime-data-streaming--end-to-end-data-engineering-project)
  - [🚀 Overview](#-overview)
  - [📋 Table of Contents](#-table-of-contents)
  - [✨ Features](#-features)
  - [✨ Features](#-features-1)
  - [🛠️ Technologies (Requirements)](#️-technologies-requirements)
    - [🔧 🚀 Installation \& Setup](#--installation--setup)
  - [👩‍💻 Usage](#-usage)
  - [📊 Data Sources](#-data-sources)
  - [🔍 Data Processing](#-data-processing)
  - [💾 Data Storage](#-data-storage)
  - [🔄 Workflow Orchestration](#-workflow-orchestration)
    - [DAG Configuration](#dag-configuration)
    - [Tasks](#tasks)
      - [1. Stream Data from API](#1-stream-data-from-api)
    - [Execution Flow](#execution-flow)
  - [📈 Monitoring](#-monitoring)
  - [🤝 Contributing](#-contributing)
  - [📝 License](#-license)

##️ 🏗️ Architecture

![System Architecture](https://github.com/airscholar/e2e-data-engineering/blob/main/Data%20engineering%20architecture.png)

## ✨ Features
The project is designed with the following components:

- **Data Source**: We use `randomuser.me` API to generate random user data for our pipeline.
- **Apache Airflow**: Responsible for orchestrating the pipeline and storing fetched data in a PostgreSQL database.
- **Apache Kafka and Zookeeper**: Used for streaming data from PostgreSQL to the processing engine.
- **Control Center and Schema Registry**: Helps in monitoring and schema management of our Kafka streams.
- **Apache Spark**: For data processing with its master and worker nodes.
- **Cassandra**: Where the processed data will be stored.

## ✨ Features

Highlight the key features of the data engineering project. This could include data processing algorithms, integration with external systems, or any unique functionalities.


## 🛠️ Technologies (Requirements)

This end-to-end data engineering project leverages a diverse set of technologies to create a robust and scalable pipeline. The key technologies used in this project include:

- **Apache Airflow:** 
- **Apache Kafka:** 
- **Apache Zookeeper:**
- **Apache Spark:**
- **Cassandra:** 
- **PostgreSQL:** 
- **Docker:** 


### 🔧 🚀 Installation & Setup

Follow these steps to get started with the end-to-end data engineering project:

1. **Clone the repository:**
    ```bash
    git clone https://github.com/airscholar/e2e-data-engineering.git
    ```

2. **Navigate to the project directory:**
    ```bash
    cd e2e-data-engineering 
    ```

3. **Run Docker Compose to spin up the services:**
    ```bash
    docker-compose up 
    ```


## 👩‍💻 Usage

Once the services are up and running, you can interact with the data engineering components. Here are some common usage scenarios:

- **Access Apache Airflow UI:**
  - Open your web browser and go to [http://localhost:8080](http://localhost:8080).
  - Explore and manage your data pipeline using the Apache Airflow user interface.

- **Monitor Kafka Streams:**
  - Utilize the Control Center and Schema Registry to monitor and manage Kafka streams.
  - Access Control Center by navigating to [http://localhost:9021](http://localhost:9021).

- **Explore Processed Data:**
  - Connect to Cassandra and PostgreSQL to explore the processed data.
  - Cassandra can be accessed using CQLSH or a preferred client.
  - PostgreSQL can be accessed using a PostgreSQL client.

- **Customization and Development:**
  - Customize and enhance the project based on your specific requirements.
  - Develop additional data processing tasks, integrate new technologies, or extend the existing functionalities.


## 📊 Data Sources

The data source for this project is the `randomuser.me` API, providing random user data. The Python script `stream_data.py` fetches data from this API and streams it to a Kafka topic named 'users_created'.

## 🔍 Data Processing

The data processing involves two main steps:

1. **Format Data:**
   - The `format_data` function in `stream_data.py` processes the raw data obtained from the API, extracts relevant information, and structures it into a dictionary.
   - Fields such as `id`, `first_name`, `last_name`, `gender`, `address`, `post_code`, `email`, `username`, `dob`, `registered_date`, `phone`, and `picture` are included in the processed data.

2. **Streaming Data to Kafka:**
   - The formatted data is then streamed to the Kafka topic 'users_created' using the `KafkaProducer` from the `kafka` library.

## 💾 Data Storage

The processed data is stored in both Cassandra and PostgreSQL databases:

1. **Cassandra:**
   - A keyspace named 'spark_streams' is created in Cassandra, and a table named 'created_users' is created within that keyspace.
   - The table schema includes columns such as `id`, `first_name`, `last_name`, `gender`, `address`, `post_code`, `email`, `username`, `dob`, `registered_date`, `phone`, and `picture`.
   - The `insert_data` function inserts the processed data into the Cassandra table.

2. **PostgreSQL:**
   - PostgreSQL is utilized to store fetched data in the `created_users` table.
   - The table schema is similar to the Cassandra table, including columns such as `id`, `first_name`, `last_name`, `gender`, `address`, `post_code`, `email`, `username`, `dob`, `registered_date`, `phone`, and `picture`.
   - Data insertion into PostgreSQL is handled automatically by the Apache Spark structured streaming process. The Spark structured streaming writes the data to the Cassandra and PostgreSQL tables concurrently.



## 🔄 Workflow Orchestration

This project uses Apache Airflow for workflow orchestration. The primary DAG (Directed Acyclic Graph) responsible for orchestrating the data streaming process is defined in the file `user_automation.py`. Here's an overview of the workflow orchestration:

### DAG Configuration

The DAG is configured with the following parameters:

- **DAG ID:** user_automation
- **Owner:** airscholar
- **Start Date:** 3rd September 2023, 10:00 AM
- **Schedule Interval:** Daily
- **Catchup:** Disabled

### Tasks

#### 1. Stream Data from API

- **Task ID:** stream_data_from_api
- **Python Callable:** `stream_data` (defined in the same file)
- **Task Description:** This task fetches random user data from the "https://randomuser.me/api/" endpoint, formats the data, and streams it to the 'users_created' Kafka topic.
- **Dependencies:** None

### Execution Flow

1. The DAG starts running daily at the specified start date and time.
2. The task `stream_data_from_api` is executed.
3. Within the `stream_data_from_api` task, the `stream_data` Python callable is invoked.
4. The `stream_data` function fetches random user data, formats it, and streams it to the 'users_created' Kafka topic using a KafkaProducer.
5. The task completes its execution, and the DAG for the day concludes.

This orchestrated workflow ensures the daily streaming of random user data into the Kafka topic, forming a crucial part of the end-to-end data engineering pipeline.


## 📈 Monitoring

The project's performance and health are monitored through the following mechanisms:

- **Logging:**
  - Extensive logging is implemented throughout the codebase to capture important events, errors, and informational messages.
  - Logs are a valuable resource for diagnosing issues, understanding the flow of data, and tracking the system's behavior.

- **Alerting:**
  - Alerts can be configured based on specific events or conditions within the project.
  - Integration with alerting systems, such as email notifications or messaging services, enhances the ability to respond promptly to critical situations.

- **Integration with Monitoring Tools:**
  - The project can be integrated with external monitoring tools for comprehensive health checks and performance analysis.
  - Tools like Prometheus, Grafana, or custom monitoring solutions can be configured to provide real-time insights into the system's metrics.

## 🤝 Contributing

We welcome contributions to enhance and improve the end-to-end data engineering project. To contribute, follow these guidelines:

- **Bug Reports:**
  - If you encounter any issues or bugs, please submit detailed bug reports.
  - Include information about the environment, steps to reproduce, and expected vs. actual behavior.

- **Feature Requests:**
  - Propose new features or improvements by opening feature request issues.
  - Clearly describe the envisioned functionality and how it aligns with the project's goals.

- **Code Contributions:**
  - Fork the repository, create a branch, and submit pull requests for code contributions.
  - Follow coding standards, provide thorough documentation, and ensure tests are included.

- **Communication:**
  - Engage in discussions through issues and pull requests.
  - Respect the project's code of conduct and maintain a positive and collaborative environment.

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.