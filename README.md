# CSCE 676 — Music Listening Pattern Mining (MusicMining)

**Author:** Bryan Ramos  
**Course:** CSCE 676 — Data Mining and Analysis (Texas A&M University, Spring 2026)  
**GitHub:** [bmr-ramos/CSCE676-MusicMining](https://github.com/bmr-ramos/CSCE676-MusicMining)

## Project Overview

This project applies data mining techniques to the **Last.fm 1K** dataset to investigate **artist community structure** in music co-listening behavior. The central question is: *Do music listening sessions reveal coherent artist communities, and what distinguishes the artists that bridge them?*

Three complementary mining perspectives are layered onto a shared community framework:
- **Community detection + PageRank** (course) — identify artist clusters and cross-community bridge artists
- **Association rule mining** (course) — characterize within- vs. cross-community co-listening patterns
- **Sequential pattern mining** (beyond-course) — reveal temporal dynamics of how listeners navigate across communities

## Dataset

**Last.fm Dataset 1K** (Celma, 2010)  
- **Source:** [ocelma.net](http://ocelma.net/MusicRecommendationDataset/lastfm-1K.html)
- **Size:** 19.15M listening events, 992 users, ~177K unique artists, ~960K unique tracks
- **Format:** Tab-separated listening histories with timestamps + user demographic profiles
- **License:** Non-commercial research use

> **Note:** The dataset is too large for GitHub (~2.4 GB). Download it directly:
> ```bash
> curl -L -o lastfm-dataset-1K.tar.gz "http://mtg.upf.edu/static/datasets/last.fm/lastfm-dataset-1K.tar.gz"
> tar xzf lastfm-dataset-1K.tar.gz
> mv lastfm-dataset-1K "Checkpoint 1/lastfm-dataset-1K"
> ```

## Repository Structure

```
├── README.md                           # This file
├── .gitignore                          # Excludes dataset files, venvs, OS files
├── Checkpoint 1/                       # EDA and dataset exploration
│   ├── CSCE676_Checkpoint1_EDA.ipynb   # Main notebook — EDA deliverable
│   ├── Checkpoint 1.txt                # Assignment requirements
│   ├── README.md                       # Checkpoint 1 details
│   ├── requirements.txt                # Python dependencies
│   └── figures/                        # Generated EDA figures (15 plots)
├── Checkpoint 2/                       # Research question formation
│   ├── CSCE676_Checkpoint2_RQ.ipynb    # Main notebook — RQ deliverable
│   ├── Checkpoint 2.txt                # Assignment requirements
│   └── figures/                        # Generated RQ figures
└── Checkpoint 1/lastfm-dataset-1K/     # Dataset (not tracked — download separately)
```

## Checkpoint Summaries

### Checkpoint 1 — Exploratory Data Analysis

Compared three candidate datasets, selected Last.fm 1K, and performed comprehensive EDA on a 150-user sample (3.3M events, 141K sessions).

**Key findings:**
- **Power-law popularity:** Top 1% of artists account for ~48% of all listening events
- **Session structure:** Median 3 unique artists/session; 68% have 2+ artists
- **Co-occurrence graph:** Artist clusters visible; Radiohead, The Cure, Death Cab as top hubs
- **Sparsity:** 98.2% user-artist matrix sparsity — low support thresholds required
- **Temporal patterns:** Evening peak (6–7 PM), weekday > weekend

### Checkpoint 2 — Research Question Formation

Defined three research questions unified under a single overarching investigation of artist community structure:

| RQ | Role | Method | Category |
|---|---|---|---|
| **RQ1:** Association rules across support thresholds | Community characterization | FP-Growth, Apriori | Course |
| **RQ2:** Community structure and PageRank bridges | **Centerpiece** | Louvain, PageRank | Course |
| **RQ3:** Sequential patterns within/across communities | Temporal dynamics | PrefixSpan | Beyond-course |

**Feasibility confirmed:** All methods tested — 725 itemsets + 838 rules, 4 communities with modularity 0.233 (z=7.10 vs. random), 33 sequential patterns.

## Reproducing the Analysis

```bash
# 1. Clone the repository
git clone https://github.com/bmr-ramos/CSCE676-MusicMining.git
cd CSCE676-MusicMining

# 2. Install dependencies
pip install -r "Checkpoint 1/requirements.txt"
pip install mlxtend prefixspan python-louvain  # Additional Checkpoint 2 packages

# 3. Download the dataset
curl -L -o lastfm-dataset-1K.tar.gz "http://mtg.upf.edu/static/datasets/last.fm/lastfm-dataset-1K.tar.gz"
tar xzf lastfm-dataset-1K.tar.gz
mv lastfm-dataset-1K "Checkpoint 1/lastfm-dataset-1K"

# 4. Run notebooks
jupyter notebook "Checkpoint 1/CSCE676_Checkpoint1_EDA.ipynb"
jupyter notebook "Checkpoint 2/CSCE676_Checkpoint2_RQ.ipynb"
```

## Dependencies

- Python 3.11+
- pandas, numpy, matplotlib, seaborn, networkx, scipy
- mlxtend, prefixspan, python-louvain (Checkpoint 2+)

## References

- Celma, O. (2010). *Music Recommendation and Discovery in the Long Tail*. Springer.
- Pei, J. et al. (2001). PrefixSpan: Mining Sequential Patterns Efficiently by Prefix-Projected Pattern Growth. *ICDE 2001*.
- Blondel, V. D. et al. (2008). Fast unfolding of communities in large networks. *JSMTE*, P10008.
