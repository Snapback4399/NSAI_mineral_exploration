# MIEX Cu–Mo Prospectivity (Notebook-only)

This repository contains a single, refactored Jupyter notebook that reproduces the core workflow for **Cu–Mo anomaly / prospectivity modeling** with:
- multi-source spatial feature engineering (map units, distances to structural line features)
- optional **knowledge-derived priors** from deposit-model JSONs using text embeddings
- spatial cross-validation evaluation (GroupKFold)

> The notebook is structured as functions + a `run_pipeline()` entry point so you can reuse parts without copy/paste.

## Quickstart

### 1) Create environment
```bash
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
```

### 2) Put your data in `data/`
Expected (paths are configurable at the top of the notebook):
- `data/Cumo.csv` (sample points with lat/lon + geochemical columns)
- `data/geology/` (shapefiles; one polygon layer for map units, optional line layers)
- `data/deposit_models_json/` (deposit-model JSONs; e.g., 87 models)

### 3) Configure embeddings (optional)
If you enable `add_knowledge_priors()`, set credentials via env vars.

**Azure OpenAI**
```bash
export AZURE_OPENAI_API_KEY="..."
export AZURE_OPENAI_ENDPOINT="https://<resource>.openai.azure.com"
export AZURE_OPENAI_EMBED_DEPLOYMENT="text-embedding-3-large"  # your deployment name
```

**OpenAI**
```bash
export OPENAI_API_KEY="..."
```

### 4) Run
Open the notebook:
```bash
jupyter lab
```
Run cells top-to-bottom. The only required edits are in the **CONFIG** cell and the **label definition** cell.

## What’s inside the notebook

- **Setup & CONFIG**: paths + parameters
- **Utilities**: I/O, CRS helpers, distance queries
- **Geology loader**: polygon map units + optional line layers
- **Points → sites**: merge duplicates into robust sites
- **Spatial features**: map-unit join + distance-to-line features
- **Knowledge priors (optional)**: embed deposit-model JSON text; compute similarity priors
- **Modeling template**: spatial CV baseline vs. knowledge-augmented

## Recommended repo layout (still notebook-only)
```
.
├── MIEX_apt8_c2_refactored_notebook_only.ipynb
├── README.md
├── requirements.txt
├── .gitignore
└── data/                   # not committed
```

## Suggested `.gitignore`
At minimum, ignore:
- `data/`
- `cache/`
- `.venv/`
- notebook checkpoints

## Reproducibility notes
- Keep your **CRS** consistent across layers (the notebook projects to a metric CRS).
- If you use embeddings, enable caching (`cache/embeddings/`) to avoid repeated API calls.
- For spatial CV, define a **block/group column** (e.g., 500 m bins) to prevent spatial leakage.

## Citation / Acknowledgement
If you publish results based on this code, cite your data sources (USGS/IGS etc.) and the deposit-model literature used to produce the JSON models.
