
# NSAI_mineral_exploration

---

## **Overview**

**NSAI_mineral_exploration** is a reproducible, notebook-based implementation of a **Neuro-Symbolic AI (NSAI)** framework for mineral prospectivity analysis. The workflow integrates multi-source geochemical observations, spatial geological information, and domain knowledge derived from ore deposit models to support anomaly detection and prospectivity mapping, with a primary focus on **Cu–Mo mineral systems**.

The repository emphasizes **transparent methodology**, **spatially robust evaluation**, and **explicit integration of geological knowledge** into data-driven models.

---

## **Methodological Framework**

The implemented workflow consists of the following stages:

1. **Data preparation and spatial alignment**

   Geochemical sample points are projected into a metric coordinate reference system and aggregated into spatially consistent sites to reduce the influence of duplicate or closely spaced samples.

2. **Geological feature construction**

   Geological map-unit polygons are spatially joined to sites, and distance-based features are derived from structural line layers (e.g., faults or contacts) to capture spatial geological context.

3. **Knowledge representation using language-model embeddings**

   Textual descriptions of ore deposit models, stored as structured JSON documents, are transformed into dense vector representations using large language model embeddings.

4. **Knowledge-derived prior construction**

   Similarity between sample-associated text and deposit-model embeddings is quantified to generate continuous prior features that encode geological plausibility rather than deterministic rules.

5. **Predictive modeling and evaluation**

   Baseline models using geochemical and spatial features are compared with knowledge-augmented models that incorporate embedding-based priors. Model performance is evaluated using **spatially aware cross-validation** to mitigate spatial leakage.

---

## **Repository Structure**

This repository follows a **notebook-centered design** intended to maximize transparency and reproducibility.

- The core workflow is implemented in a single refactored Jupyter notebook  
  (`MIEX_NSAI_refactored.ipynb`).

- Supporting files include:
  - `README.md` for methodological documentation,
  - `requirements.txt` for environment specification, and
  - `.gitignore` for excluding data, caches, and environment-specific files.

- User-provided datasets (e.g., geochemical CSV files, geological shapefiles, and deposit-model JSONs) are expected to reside in a local `data/` directory and are **not tracked** by version control.

---

## **Data Requirements**

The workflow expects the following user-provided inputs. All file paths are configurable within the notebook.

- **Geochemical sample data**

  A CSV file containing sample locations (latitude/longitude) and geochemical measurements.

- **Geological map data**

  Shapefiles including:

  - one polygon layer representing geological map units  
  - optional line layers representing structural features (e.g., faults, contacts)

- **Ore deposit model knowledge**

  A collection of JSON files encoding ore deposit model descriptions, including lithology, alteration, mineralogy, and geochemical characteristics.

Large datasets and proprietary data are excluded from version control.

---

## **Knowledge Integration Strategy**

Geological knowledge is incorporated using a **soft neuro-symbolic approach**. Deposit-model knowledge is encoded as **continuous, embedding-based similarity priors** rather than hard logical constraints. This design:

- preserves uncertainty inherent in geological interpretation,  
- allows direct integration with standard machine-learning models, and  
- supports comparison between data-driven and knowledge-augmented approaches.

---

## **Modeling and Evaluation**

- Spatial features and geochemical variables are used to construct **baseline models**.
- Knowledge-augmented models additionally include **embedding-derived prior features**.
- Model evaluation is performed using **GroupKFold-based spatial cross-validation** to reduce the impact of spatial autocorrelation.
- Target definitions (e.g., anomaly thresholds or prospectivity labels) are **study-specific** and must be defined by the user.

---

## **Reproducibility**

- All spatial computations are performed in a **metric coordinate reference system**.
- Embedding generation supports **local caching** to ensure reproducibility and cost control.
- API credentials for language-model embeddings are managed via **environment variables** and are not stored in the repository.

---

## **Scope and Limitations**

- The current implementation is designed for **regional-scale mineral prospectivity analysis**.
- The workflow is demonstrated for **Cu–Mo systems** but can be adapted to other deposit types by modifying input data and deposit-model descriptions.
- The framework does **not** enforce formal logical rules and should not be interpreted as a rule-based expert system.

---

## **Intended Use**

This repository is intended for:

- methodological research in mineral prospectivity and mineral informatics,  
- reproducible supplements to peer-reviewed publications,  
- exploratory studies combining geological knowledge with machine learning.

It is **not intended** for operational or production-level exploration decision-making.

---

## **Citation**

If you use this repository or adapt the workflow in academic work, please cite the associated publication and the geological data sources used to construct the input datasets and deposit-model knowledge.
