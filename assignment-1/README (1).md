# Rental Agreement Metadata Extractor — No API Key Required

Extracts structured metadata from rental agreements (`.docx` or `.png`) using a **100% local, open-source AI model** — no API keys, no accounts, no paid services.

---

## How It Works

```
.docx file  →  python-docx  →  plain text  ┐
                                            ├─→  Qwen2.5-1.5B-Instruct (local LLM)  →  JSON
.png file   →  Tesseract OCR →  plain text  ┘
```

| Component | Tool | Cost |
|---|---|---|
| DOCX reading | `python-docx` | Free |
| Image OCR | `pytesseract` + Tesseract | Free |
| AI extraction | `Qwen2.5-1.5B-Instruct` via HuggingFace | Free |
| GPU | Google Colab T4 | Free |

---

## Google Drive Structure Required

```
MyDrive/
├── train.csv
├── test.csv
├── train/
│   ├── 6683127-House-Rental-Contract-GERALDINE-GALINATO-v2-Page-1.docx
│   ├── 24158401-Rental-Agreement.png
│   └── ... (all training files)
└── test/
    ├── 24158401-Rental-Agreement.png
    ├── 95980236-Rental-Agreement.png
    └── ... (all test files)
```

---

## Running in Google Colab

### 1. Set Runtime to GPU
`Runtime → Change runtime type → T4 GPU`  
*(Required — model needs ~3GB VRAM)*

### 2. Upload the notebook
Upload `rental_extractor_no_api_key.ipynb` to Colab

### 3. Run all cells top to bottom
- Cell 1: Installs Tesseract + Python packages
- Cell 2: Mounts Drive, sets paths
- Cell 3: Downloads & loads Qwen2.5 model (~3GB, done once)
- Cell 4: Loads all utilities
- Cell 5: Defines `extract_metadata()`
- Cell 6-7: Evaluates on training set, prints recall
- Cell 8-9: Predicts on test set, saves `test_predictions.csv`
- Cell 10-11: Optional Flask REST API

---

## Fields Extracted

| Field | Example Output |
|---|---|
| `Aggrement Value` | `12000` |
| `Aggrement Start Date` | `"15.12.2012"` |
| `Aggrement End Date` | `"14.11.2013"` |
| `Renewal Notice (Days)` | `30` |
| `Party One` | `"V.K.NATARAJ"` |
| `Party Two` | `"SRI VYSHNAVI DAIRY SPECIALITIES Pvt Ltd"` |

---

## Test Set Predictions

| File Name | Value | Start | End | Notice | Party One | Party Two |
|---|---|---|---|---|---|---|
| 24158401-Rental-Agreement | 12000 | 01.04.2008 | 31.03.2009 | 60 | Hanumaiah | Vishal Bhardwaj |
| 95980236-Rental-Agreement | 9000 | 01.04.2010 | 31.03.2011 | 30 | S.Sakunthala | V.V.Ravi Kian |
| 156155545-Rental-Agreement-Kns-Home | 12000 | 15.12.2012 | 14.11.2013 | 30 | V.K.NATARAJ | VYSHNAVI DAIRY SPECIALITIES Pvt Ltd |
| 228094620-Rental-Agreement | 15000 | 07.07.2013 | 06.06.2014 | 30 | KAPIL MEHROTRA | B.Kishore |

---

## REST API Usage (Optional — Step 9)

After running the Flask cell:

```bash
# Health check
curl http://localhost:5000/health

# Extract from a DOCX file
curl -X POST http://localhost:5000/extract \
     -F "file=@your_agreement.docx"

# Extract from a PNG image
curl -X POST http://localhost:5000/extract \
     -F "file=@your_agreement.png"
```

**Response:**
```json
{
  "Aggrement Value": 12000,
  "Aggrement Start Date": "15.12.2012",
  "Aggrement End Date": "14.11.2013",
  "Renewal Notice (Days)": 30,
  "Party One": "V.K.NATARAJ",
  "Party Two": "SRI VYSHNAVI DAIRY SPECIALITIES Pvt Ltd"
}
```

---

## Recall Metric

**Recall per field** = exact matches / total documents

The notebook prints this automatically for both training and test sets after running inference.
