# Retail Sales Analytics - Online Retail II

Questo progetto si occupa dell'analisi avanzata dei dati di vendita di un e-commerce globale, coprendo l'intero biennio dal 2009 al 2011. L'obiettivo è trasformare oltre 1 milione di transazioni grezze in informazioni strategiche per il business attraverso tecniche di Data Cleaning, Exploratory Data Analysis (EDA), calcolo dei KPI commerciali, Analisi delle Coorti e segmentazione dei clienti tramite modello RFM.

---
## 📝 Stato Avanzamento Lavori

### 🧼 Fase 1: Ispezione Iniziale e Data Cleaning (Completato ✅)
Nel notebook `01_data_cleaning.ipynb` è stata eseguita la prima sfoltitura del dataset:
- **Unione del Dataset:** Uniti i dati del biennio ottenendo una base di partenza di **1.067.371 righe**.
- **Gestione dei Valori Nulli:** Il **22.77%** delle transazioni totali è privo di `Customer ID` (acquisti da utenti anonimi). Rilevate anche 4.382 descrizioni mancanti.
- **Rimozione Duplicati:** Individuate e rimosse **34.335 righe completamente duplicate**, portando il dataset finale a **1.033.036 righe** pulite.
- **Analisi Geografica:** Oltre il **91% del mercato** si concentra nel Regno Unito (`United Kingdom`), seguito da `EIRE` (Irlanda) e Germania.

---
 

---

## 📁 Struttura del Progetto

```text
retail_sales_analytics/
├── .venv/                  # Ambiente virtuale Python
├── data/                   # Dataset originale (online_retail_II.xlsx)
├── output/                 # Grafici ed esportazioni finali
├── 01_data_cleaning.ipynb  # Pulizia dati e gestione anomalie
├── 02_eda.ipynb            # Analisi esplorativa e trend di vendita
├── 03_kpi_calculations.ipynb# Calcolo delle metriche di business (Fatturato, AOV, ecc.)
├── 04_rfm_and_cohorts.ipynb# Analisi della Retention (Coorti) e Segmentazione RFM
├── requirements.txt        # Librerie necessarie per il progetto
└── README.md               # Documentazione del progetto