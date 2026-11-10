# Automated Sales Reporting & Anomaly Detection Pipeline

![n8n](https://img.shields.io/badge/n8n-FF6D5W?style=for-the-badge&logo=n8n&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Gemini AI](https://img.shields.io/badge/Gemini_AI-4285F4?style=for-the-badge&logo=google&logoColor=white)

A fully automated, end-to-end n8n workflow that fetches daily sales data, detects statistical anomalies using Python, generates an executive summary using Google Gemini AI, and alerts stakeholders via email.

---

##  Problem Statement

In modern retail and e-commerce operations, tracking massive volumes of daily transactions manually is inefficient and prone to human error. Businesses struggle to:
* Rapidly identify abnormal sales spikes, unusual transaction amounts, or fraudulent purchasing behavior in real-time.
* Aggregate large datasets quickly without setting up heavy cloud data infrastructure.
* Turn raw transactional anomalies into concise, human-readable executive summaries without spending hours writing reports.

##  The Solution

This project implements an automated pipeline using **n8n** that solves these issues by acting as an intelligent, automated data analyst. 

### How it works:
1. **Automated Ingestion:** Triggers on a scheduled basis to fetch 100+ raw cart records from an external sales API endpoint (`DummyJSON`) and flattens nested JSON structures.
2. **Data Transformation & Statistical Analysis:** Simulates order dates, parses item properties, and executes a custom **Python script** to compute statistical Z-scores for total transaction amounts. It flags transactions with an absolute Z-score > 2 as anomalies.
3. **Storage & AI Reporting:** Automatically inserts flagged anomalies into an n8n Data Table for auditing, aggregates the records, and feeds them into **Google Gemini** to generate a clear, 5-line executive summary report.
4. **Instant Notification:** Emails the generated intelligence report directly to stakeholders via a Gmail integration.

---

##  Tech Stack

* **Workflow Automation:** n8n (Cloud / Self-hosted)
* **Scripting & Analytics:** Python (Native node for Data Processing & Z-Score Anomaly Detection)
* **AI & LLM Integration:** Google Gemini Model (`models/gemini-3-flash-preview`)
* **Storage & Comms:** n8n Data Tables & Gmail API

---

##  Installation & Setup

### Prerequisites
To use this workflow, you will need:
* An active [n8n](https://n8n.io/) instance (Cloud or Self-hosted).
* A Google Gemini API Key.
* A connected Gmail account (OAuth2 setup in n8n).

### Importing the Workflow
1. Download or clone this repository to your local machine.
2. Open your n8n dashboard and navigate to **Workflows**.
3. Create a new workflow.
4. Click the options menu (`...`) in the top right corner of the canvas and select **Import from File**.
5. Select the `Automated_Sales_Reporting.json` file from this repository.
6. The workflow will populate on your screen.

### Configuring Credentials
Once imported, you will notice some nodes might have warning icons. You need to link your own credentials:
1. Double-click the **Message a model (Gemini)** node and connect your Google AI credentials.
2. Double-click the **Send a message (Gmail)** node and authenticate with your Google Workspace / Gmail account.
3. Update the "Send To" email address in the Gmail node to your preferred recipient.
4. Update the **Insert row (Data Table)** node to point to a Data Table inside your own n8n instance.

## 💡 Usage

Once your credentials are set up, you can test the pipeline by clicking the **Test Workflow** or **Execute** button at the bottom of the n8n canvas. To run it continuously, toggle the workflow switch to **Active** in the top right corner.
