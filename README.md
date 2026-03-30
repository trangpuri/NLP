# Recipe NLP Data Preparation Pipeline

This project is a critical part of a data preparation pipeline designed to clean, structure, and validate raw recipe datasets for AI training. Instead of building models directly, this pipeline focuses on ensuring high data quality through rigorous text normalization, structural validation, and NLP-based ingredient parsing.

## What the Code is Doing

The core script, `ingredient_parsing_m1.py`, executes several sequential data quality phases:
1. **Data Cleaning & Normalization:** Corrects encoding issues (mojibake) and normalizes whitespace across text fields.
2. **Quality & Validation:** Runs language detection to ensure English-only data, flags corrupted ingredients (HTML tags, tracking URLs, etc.), and checks for data completeness.
3. **Structured Parsing:** Normalizes recipe quantities (e.g., Unicode fractions to decimals, parsing ranges, extracting package sizes). It further uses NLP and Gemini AI to categorize the data into structured parts: `quantity`, `unit`, `name`, and `modifier`.

## Setup & Configuration

### 1. Where to Change the API Key

The advanced parsing utilizes the Gemini AI API. To provide your own key, open `ingredient_parsing_m1.py` and modify **Line 52**.

You can either set an environment variable `GEMINI_API_KEY` before running the script, or replace the default string directly in the code:

```python
# Change "YOUR_API_KEY_HERE" to your actual Google Gemini API key
GEMINI_API_KEY  = os.environ.get("GEMINI_API_KEY", "YOUR_API_KEY_HERE")
```

### 2. Input File Configuration

The pipeline reads raw recipe data from an Excel file. By default, the code is set up for a specific file path, which you'll need to update.

Open `ingredient_parsing_m1.py` and modify **Line 59**:

```python
# Replace with the path to your raw dataset
M1_INPUT    = "path/to/your/input_file.xlsx"
```

**Input Data Requirements:**
- The input must be a valid `.xlsx` Excel file.
- The file must contain a column named `ingredients` or `ingredient`.
- It optionally handles `title` and `directions` columns if available.

### 3. Dependencies

Ensure you have installed the required Python packages before execution:

```bash
pip install pandas openpyxl ftfy xlsxwriter langdetect ingredient-parser-nlp
```

## Running the Pipeline

To execute the data preparation pipeline, run:

```bash
python ingredient_parsing_m1.py
```

The script will produce structured, AI-ready datasets (e.g., `milestone1_Pizza_cleaned.xlsx`) in the same directory.
