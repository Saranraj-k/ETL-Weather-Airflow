# ETL Weather Airflow 🚀🌦️

Welcome to the **ETL Weather Airflow** project!  
This repository contains a robust ETL pipeline for fetching, transforming, and loading weather data using [Apache Airflow](https://airflow.apache.org/) and modern Python best practices.

---

## 🛠️ Tools & Libraries Used

| Tool / Library             | Description                                                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------|
| **Apache Airflow** 🌬️     | Orchestrate ETL workflows and schedule DAGs.                                                  |
| **Astro Runtime** 🪐       | Enhanced Airflow Docker image ([quay.io/astronomer/astro-runtime](https://quay.io/astronomer/astro-runtime)). |
| **Docker** 🐳              | Containerize and run Airflow locally.                                                         |
| **Python** 🐍              | Write ETL logic and DAGs.                                                                    |
| **requests** 🌐            | Make HTTP requests to external APIs.                                                          |
| **PostgreSQL** 🐘          | Store processed weather data.                                                                |
| **Airflow Providers** 🔌   | Use [HTTP](https://airflow.apache.org/docs/apache-airflow-providers-http/stable/index.html) and [Postgres](https://airflow.apache.org/docs/apache-airflow-providers-postgres/stable/index.html) hooks for data transfer. |
| **TaskFlow API** 🏗️        | Write Pythonic and scalable Airflow tasks.                                                    |

---

## 📂 Project Structure

```
├── dags/
│   ├── etlWeather.py  # Main ETL pipeline DAG
│   └── exampledag.py  # Example DAG for learning
├── Dockerfile         # Astro Runtime Docker image
├── requirements.txt   # Python dependencies
├── packages.txt       # OS-level packages (optional)
├── airflow_settings.yaml # Airflow connections, variables, pools
├── plugins/           # Custom Airflow plugins (optional)
└── include/           # Extra files to bundle (optional)
```

---

## 🌦️ What Does This Project Do?

- **Extract** weather data from [Open-Meteo API](https://open-meteo.com/) using Airflow HTTP Hook.
- **Transform** data (temperature, windspeed, direction, weathercode).
- **Load** transformed data into a PostgreSQL database using Airflow Postgres Hook.

Example DAGs use dynamic task mapping and Pythonic workflows for modern ETL.

---

## 🚀 Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/Saranraj-k/ETL-Weather-Airflow.git
cd ETL-Weather-Airflow
```

### 2. Start Airflow locally

```bash
astro dev start
```
This spins up Docker containers for:
- **Postgres** 🐘 (metadata DB)
- **Webserver** 🌐 (Airflow UI)
- **Scheduler** ⏰ (schedules DAGs)
- **Triggerer** 🎯 (triggers deferred tasks)

### 3. Access Airflow UI

- Go to [http://localhost:8080/](http://localhost:8080/)
- Login: `admin` / `admin`

---

## 📝 Example: Weather ETL DAG

```python
from airflow.providers.https.hooks.http import HttpHook
from airflow.providers.postgres.hooks.postgres import PostgresHook
from airflow.decorators import task

@task()
def extract_weather_data():
    http_hook = HttpHook(http_conn_id="open_meteo_api", method="GET")
    endpoint = f"/v1/forecast?latitude=20.5937&longitude=78.9629&current_weather=true"
    response = http_hook.run(endpoint)
    return response.json() if response.status_code == 200 else {}

@task()
def transform_weather_data(weather_data):
    current_weather = weather_data["current_weather"]
    return {
        "temperature": current_weather["temperature"],
        "windspeed": current_weather["windspeed"],
        "winddirection": current_weather["winddirection"],
        "weathercode": current_weather["weathercode"]
    }
```

---

## 📚 Resources

- [Airflow Docs](https://airflow.apache.org/docs/)
- [Astro CLI Docs](https://www.astronomer.io/docs/astro/cli/)
- [Open-Meteo API Docs](https://open-meteo.com/en/docs)

---

## ✨ Credits  
Built with Astronomer, Airflow, Python, and open APIs!


