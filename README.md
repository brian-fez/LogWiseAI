# LogWiseAI — Intelligent Log Message Classification System

## 🔍 Overview

**LogWiseAI** is a modular, production-ready log message classification framework that intelligently routes log entries through a hybrid classification pipeline: rule-based regex, lightweight BERT embeddings, and large language models (LLMs). The system enables organizations to automatically categorize large volumes of semi-structured or unstructured log data from various sources in a reliable and explainable manner.

---

## 🎯 Project Objective

Modern software systems generate a massive volume of logs across services, environments, and formats. Manual classification is error-prone and time-consuming. **LogWiseAI** solves this by:

- Detecting key log types (e.g., workflow errors, system events, deprecations)
- Supporting multi-source logs (e.g., Legacy CRM, Modern HR, Analytics)
- Combining traditional rule-based methods with advanced ML/NLP
- Enabling fallback strategies when confidence is low

---

## 🏭 Applicable Industries

This solution is valuable for:

- **Enterprise SaaS Platforms**
- **IT Operations (ITOps) and DevOps Teams**
- **Cybersecurity and Incident Monitoring**
- **Telecom and Network Management**
- **Fintech / Banking platforms with audit log pipelines**
- **Legacy-to-Modern software migration teams**

---

## 🧠 Classification Workflow

1. **Regex-based Classification** — Fast rule-based matches for known log patterns  
2. **BERT Classifier** — Embeds logs with `all-MiniLM-L6-v2` and classifies via a trained `scikit-learn` model  
3. **LLM Fallback (Ollama)** — Uses LLMs for contextual classification when regex and ML are uncertain  

![arch](https://github.com/user-attachments/assets/4e6aff3e-dd69-429d-b605-1c57fcc2e96c)

### Example Labels:
- `Workflow Error`
- `Deprecation Warning`
- `System Notification`
- `User Action`
- `Unclassified`

---

## ⚙️ Architecture

```
client → FastAPI (server.py)
           └── classify.py
                 ├── processor_regex.py
                 ├── processor_bert.py
                 └── processor_llm.py
```

---

## 🚀 How to Use

### 1. Run the API server
```bash
uvicorn server:app --reload
```

### 2. Upload a CSV file to `/classify/` endpoint:
- File format: `.csv`
- Required columns: `source`, `log_message`

```csv
source,log_message
ModernCRM,File uploaded successfully
LegacyCRM,The ReportGenerator module will be retired in version 4.0.
...
```

The endpoint will return the same CSV with an added `target_label` column.

---

## 🧪 Local Testing (CLI)
You can test logs manually by running:

```bash
python classify.py
```

Inside `classify.py`, modify or uncomment the test block to try sample logs.

---

## 📁 File Structure

```
classification-logs/
├── classify.py              # Core classification routing
├── server.py                # FastAPI endpoint
├── processor_bert.py        # BERT + scikit-learn model
├── processor_llm.py         # LLM classifier via Ollama
├── processor_regex.py       # Regex-based rule classifier
├── modelslog_classifier.joblib   # (Your pre-trained ML model)
└── resources/
    ├── output.csv           # Output CSV after classification
    └── test.csv             # Sample input logs
```

---

## 🧩 Environment & Dependencies

Install required packages:

```bash
pip install fastapi uvicorn pandas joblib sentence-transformers python-dotenv
```

Also make sure **Ollama** is running locally with a supported model (e.g., `llama3`):

```bash
ollama run llama3
```

---

## 🛠️ Customization

- **Add new log sources** in `classify_log()` in `classify.py`
- **Extend regex rules** in `processor_regex.py`
- **Retrain the BERT classifier** with more labeled data
- **Switch LLM model** in `.env` or directly in `processor_llm.py`

---

## 📌 Future Enhancements

- Confidence scores in CSV output  
- Streamlit dashboard or web frontend  
- Dockerization for deployment  
- Logs visualization and filtering
