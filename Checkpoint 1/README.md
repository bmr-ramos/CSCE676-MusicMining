# CSCE 676 — Music Listening Pattern Mining

**Author:** Bryan Ramos  
**Course:** CSCE 676 — Data Mining (Texas A&M University)  
**Date:** February 2026

## Project Overview

This project applies data mining techniques to the **Last.fm 1K** dataset to discover patterns in music listening behavior. The analysis treats user listening sessions as transactional baskets of artists, enabling frequent itemset mining, association rule discovery, and graph-based community analysis of artist co-occurrence networks.

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
> ```

## Repository Structure

```
├── CSCE676_Checkpoint1_EDA.ipynb   # Main notebook — EDA and checkpoint 1 deliverable
├── figures/                        # All generated figures (15 individual plots)
│   ├── fig_session_sizes_linear.png
│   ├── fig_session_sizes_log.png
│   ├── fig_top30_artists.png
│   ├── fig_rank_frequency.png
│   ├── fig_hourly_activity.png
│   ├── fig_daily_activity.png
│   ├── fig_user_total_events.png
│   ├── fig_user_artist_diversity.png
│   ├── fig_user_sessions.png
│   ├── fig_volume_vs_diversity.png
│   ├── fig_cooccurrence_graph.png
│   ├── fig_basket_sizes.png
│   ├── fig_gender_distribution.png
│   ├── fig_age_distribution.png
│   └── fig_country_distribution.png
├── Checkpoint 1.txt                # Assignment requirements
├── requirements.txt                # Python dependencies
├── .gitignore                      # Excludes dataset files (~3 GB)
└── README.md                       # This file
```

## Checkpoint 1 — EDA Summary

### Candidate Datasets Compared
| Dataset | Primary Technique | Beyond-Course |
|---|---|---|
| **Last.fm 1K** (selected) | Frequent itemsets + graph mining | Sequential pattern mining |
| LastFM Asia Social Network | Graph mining (PageRank, communities) | GNNs, node2vec |
| Spotify Tracks Audio Features | Clustering | t-SNE/UMAP, autoencoders |

### Key EDA Findings
- **3.3M events** analyzed (150-user sample), **141K sessions** (30-min gap threshold)
- **Power-law popularity:** Top 1% of artists account for ~48% of all listening events
- **Session structure:** Median 3 unique artists per session; 68% of sessions have 2+ artists
- **Co-occurrence graph:** 35-node visualization reveals artist clusters (Radiohead, The Cure, Death Cab as top hubs by PageRank)
- **Temporal patterns:** Evening listening peak (6–7 PM), weekday > weekend
- **Sparsity:** 98.2% user-artist matrix sparsity — low support thresholds required
- **Demographics:** 61% male, median age 24, US/UK-dominated user base

### Planned Techniques

| Technique | Category | Method |
|---|---|---|
| Frequent Itemset Mining | Course | FP-Growth (sessions as baskets of artists) |
| Association Rule Mining | Course | Apriori with lift/conviction filtering |
| Community Detection | Course | Louvain algorithm on co-occurrence graph |
| PageRank | Course | Centrality analysis on artist graph |
| Sequential Pattern Mining | Beyond-Course | PrefixSpan on ordered session sequences |

## Reproducing the Analysis

```bash
# 1. Clone the repository
git clone https://github.com/bmr-ramos/CSCE676-MusicMining.git
cd CSCE676-MusicMining

# 2. Install dependencies
pip install -r requirements.txt

# 3. Download the dataset
curl -L -o lastfm-dataset-1K.tar.gz "http://mtg.upf.edu/static/datasets/last.fm/lastfm-dataset-1K.tar.gz"
tar xzf lastfm-dataset-1K.tar.gz

# 4. Open and run the notebook
jupyter notebook CSCE676_Checkpoint1_EDA.ipynb
```

## Dependencies

- Python 3.11+
- pandas, numpy, matplotlib, seaborn, networkx, scipy

## Citation

Celma, O. (2010). *Music Recommendation and Discovery in the Long Tail*. Springer.
