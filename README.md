# Ai_DataPipeline_and_Analyzer

AI-driven data pipeline: **one sentence → chart**. You say what you want to analyze; the AI picks dimension, metric, and chart type, then the app queries the Database and Draws the chart which you can download.

## Where is the Large Language model?

The model runs **locally** via Ollama. It is **not** stored in this project folder. Ollama keeps models in its own directory (on macOS often `~/.ollama/models`). This app talks to `http://localhost:11434`; no API key needed.

Note that: everything of this application is only useable when you deployed LLMs, data sources and databases locaolly, if you have LLMs or databases which deployed in cloud platform, you can insert the API keys yourself.

## How to run

1. **Create a virtual environment and install dependencies** (from project root):
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate   # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

2. **Run ETL** (per data source; default is `sales`):
   ```bash
   python ETL/run_etl.py           # sales -> db/app.duckdb
   python ETL/run_etl.py events    # events -> db/events.duckdb
   ```
   You can also run ETL from the dashboard sidebar ("Run ETL for this data source").

3. **Start Ollama** (if not already running):
   ```bash
   brew services start ollama   # or open the Ollama app
   ollama pull qwen2.5:7b       # if you haven’t already, you can also pull whaterver it is on your computer
   ```

4. **Launch the dashboard**:
   ```bash
   streamlit run Dashboard/app.py
   ```
   
   After running this there should be a URL in your terminal which will be a localhost address, it is a front-end page of this application.
   
   Open the URL, **choose a data source** in the sidebar (Sales / Events), run ETL for it to load datasource into the database if needed, then type what you want (e.g. *sales by category*, *revenue by country*), click **Generate chart**.

## Data sources (multi-source)

- **Sales** (`data/raw/sales.csv`): date, category, region, sub_category, product, sales, quantity, profit, discount. ~8k rows.
- **Events** (`data/raw/events.csv`): event_date, country, device_type, channel, event_name, sessions, conversions, revenue. ~6k rows.

Each source has its own DuckDB file and its own dimensions/metrics; the same "one sentence → AI → SQL → chart" flow runs per source.

## Scripts folder

After you have `data/raw/sales.csv`, the **scripts/** folder is optional. You can delete it; ETL, AI, and dashboard do not depend on it.

## Local AI

See **OLLAMA_SETUP.md** for installing Ollama and pulling a model.
# Ai_Analyzer
