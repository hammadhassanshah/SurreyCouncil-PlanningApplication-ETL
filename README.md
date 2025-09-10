# Surrey Council Planning Applications — ETL

Notebook-driven pipeline that collects UK planning-application data, harvests document links, extracts text from PDFs, and outputs analytics‑ready CSVs. Designed for Tandridge/Surrey councils but easily extensible.

## What it does
- **Acquire application list** for a date range / area  
- **Collect document titles & URLs** per application  
- **Parse PDFs to text** for search/filters (e.g., “Decision Notice”, “Highway Authority”)  
- **Join to final dataset** for analysis or RAG/agent pipelines

**Notebooks (run in order):**
1. `01_acquireList.ipynb` – base application list  
2. `02_acquireFileAndDocURL.ipynb` – file names + document URLs  
3. `03_processDocumentContent.ipynb` – download & extract PDF text  
4. `04_finalJoin.ipynb` – tidy, joined outputs

## Quick start
```bash
git clone https://github.com/hammadhassanshah/SurreyCouncil-PlanningApplication-ETL.git
cd SurreyCouncil-PlanningApplication-ETL
python -m venv .venv && source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -U pip
```

**Dependencies (install via `pip install -r requirements.txt` or copy list):**
```txt
pandas
numpy
tqdm
requests
beautifulsoup4
pypdf            
pdfplumber       
selenium         
webdriver-manager
papermill        
python-dotenv 
pyyaml           
```

**Configure (either edit the first notebook or create a `.env`):**
```ini
COUNCIL_NAME=Tandridge
DATE_FROM=2024-12-01
DATE_TO=2024-12-30
KEYWORDS=Report,Decision Notice,Highway Authority,Surrey County Council,Appeal,Rebuttal
OUTPUT_DIR=data
```

**Run:**
- Open each notebook in order **or** automate with papermill:
```bash
papermill 01_acquireList.ipynb out/01.ipynb
papermill 02_acquireFileAndDocURL.ipynb out/02.ipynb
papermill 03_processDocumentContent.ipynb out/03.ipynb
papermill 04_finalJoin.ipynb out/04.ipynb
```

## Outputs
- `data/applications.csv` – applications metadata  
- `data/documents.csv` – document names + URLs  
- `data/document_text.csv` – extracted PDF text (1 row/doc)  
- `data/final.csv` – joined, analysis‑ready table

## Extend
- Add new councils via a small config (base URLs & selectors)  
- More filters (status/parish/keywords), better pagination/retries  
- OCR fallback for scanned PDFs (e.g., `pytesseract`)  
- Embed `document_text` to power search/RAG/agents

## Notes
Respect each council’s terms and `robots.txt`. Cache downloads, add backoff, and keep request rates modest.

---

Created by **Hammad Hassan**. PRs & issues welcome.
