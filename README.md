🎵 Music-EDA — Automated ETL + GitHub Actions + Power BI Pipeline

This project implements a fully automated ETL pipeline using Python, GitHub Actions, and Power BI.
It extracts music data from the Deezer API, transforms it, stores it in GitHub, and refreshes automatically for visualization.

📁 Project Structure
Music-EDA/
│
├── .github/
│   └── workflows/
│       └── etl.yml
│
├── etl/
│   ├── main.py
│   └── output/
│       └── tracks.csv
│
└── requirements.txt

⚙️ ETL Script

etl/main.py:

Calls the Deezer API

Converts JSON into a pandas DataFrame

Saves the CSV into etl/output/tracks.csv

Ensures the output folder exists

📦 Dependencies

requirements.txt:

pandas
requests

🤖 GitHub Actions Workflow

File: .github/workflows/etl.yml

Runs daily at 06:00 UTC and on-demand.

name: Update tracks CSV

on:
  schedule:
    - cron: "0 6 * * *"
  workflow_dispatch:

jobs:
  run-etl:
    runs-on: ubuntu-latest

    permissions:
      contents: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Install dependencies
        run: |
          pip install -r requirements.txt

      - name: Run ETL script
        run: |
          python etl/main.py

      - name: Commit and push updated CSV
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add etl/output/tracks.csv || echo "No CSV to add"

          if git diff --cached --quiet; then
            echo "No changes to commit"
          else
            git commit -m "Update tracks CSV via GitHub Actions"
            git push
          fi


📊 Power BI Integration

Open Power BI

Get Data → Web

Paste RAW CSV URL:

https://raw.githubusercontent.com/setoKaiba00/Music-EDA/main/etl/output/tracks.csv



⏱ Automation Frequency

Cron expression:

0 6 * * *   → runs daily at 06:00 UTC


Power BI dashboard connected to live data

Error-resistant automation
