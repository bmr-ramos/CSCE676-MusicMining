# Music Listening Pattern Mining (MusicMining)

> Mining 19 million Last.fm listening events to discover **artist communities** and the **bridge artists** that connect them — a unified data-mining lens on how music taste is organized.

**Author:** Bryan Ramos

**Course:** CSCE 676 — Data Mining and Analysis (Texas A&M University, Spring 2026)

**GitHub:** https://github.com/bmr-ramos/CSCE676-MusicMining

🎥 **Project video (2 min):** https://www.youtube.com/watch?v=64UBabpWFNI

👉 **Start here:** [`main_notebook.ipynb`](main_notebook.ipynb)

---

## Project overview

Music streaming platforms have rich data on what people *listen to*, but a much harder question is what artists *belong together* in listeners' minds. This project answers that by treating each user's listening session as a "transaction" of artists and applying three complementary mining techniques — community detection, association rules, and sequential pattern mining — to a single shared map of artist communities. The unifying insight is that the same handful of artists keeps surfacing as **bridges** across all three lenses, suggesting a robust, interpretable foundation for community-aware music discovery and recommendation.

---

## Research questions

A single overarching question drives the project:

> **Do music listening sessions reveal coherent artist communities, and what distinguishes the artists that bridge them?**

It decomposes into:

| RQ | Focus | Algorithm(s) | Category |
|---|---|---|---|
| **RQ1** | What within- vs. cross-community co-listening rules emerge? | FP-Growth, Apriori | Course |
| **RQ2** | **(Centerpiece)** Are communities non-random, and which artists bridge them? | Louvain, PageRank | Course |
| **RQ3** | Do sequential patterns reveal structure unordered itemsets miss? | PrefixSpan | **Beyond-course** |

---

## Headline results

| | |
|---|---|
| Listening events analyzed | **3.3 M** (150-user sample of 19.15 M total) |
| Sessions reconstructed | **141,616** (96,269 with 2+ artists) |
| Artist communities (Louvain) | **4** |
| Modularity vs. random baseline | **0.233** vs. **0.191 ± 0.006** → **z = 7.10** |
| Frequent itemsets (FP-Growth, sup ≥ 0.005) | **725** |
| Association rules (lift ≥ 1.0) | **838**; top lift ≈ **63.8** |
| Sequential patterns (PrefixSpan) | **33** |
| Top bridge artists (PageRank, all 4 communities) | **Radiohead, Death Cab For Cutie, The Cure, U2, Metallica** |

The full analysis, figures, and discussion are in [`main_notebook.ipynb`](main_notebook.ipynb).

---

## Data

**Last.fm Dataset 1K** (Celma, 2010) — 19.15 M timestamped listening events, 992 users, ~177 K unique artists, ~960 K unique tracks, plus user demographic profiles.

- **Source:** http://ocelma.net/MusicRecommendationDataset/lastfm-1K.html
- **Mirror used:** http://mtg.upf.edu/static/datasets/last.fm/lastfm-dataset-1K.tar.gz
- **Size:** ~672 MB compressed, ~2.4 GB unpacked — **not committed to this repo**.
- **License:** Non-commercial research use.

### Downloading the data

```bash
curl -L -o data/lastfm-dataset-1K.tar.gz \
  "http://mtg.upf.edu/static/datasets/last.fm/lastfm-dataset-1K.tar.gz"
tar xzf data/lastfm-dataset-1K.tar.gz -C data/
```

Full instructions: [`data/README.md`](data/README.md).

### Preprocessing (in-notebook)

1. **Sample** 150 users with a deterministic seed (≈ 3.3 M events, keeps mining tractable while preserving long-tail structure).
2. **Sort** events chronologically per user.
3. **Segment** into sessions with a 30-minute inactivity gap (standard MIR threshold).
4. **Build** per-session artist baskets — the transactions consumed by all three mining methods.

These steps run from a single cell in each notebook.

---

## How to reproduce

The project was developed locally in **VS Code** with **Python 3.14.0** and a project-local `.venv`. To reproduce end-to-end:

```bash
# 1. Clone
git clone https://github.com/bmr-ramos/CSCE676-MusicMining.git
cd CSCE676-MusicMining

# 2. (Optional) Create a virtual environment
python -m venv .venv
source .venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Download the data
curl -L -o data/lastfm-dataset-1K.tar.gz \
  "http://mtg.upf.edu/static/datasets/last.fm/lastfm-dataset-1K.tar.gz"
tar xzf data/lastfm-dataset-1K.tar.gz -C data/

# 5. Open the main notebook
jupyter notebook main_notebook.ipynb
```

**Suggested order if you want the full progression:**
1. [`main_notebook.ipynb`](main_notebook.ipynb) — curated final analysis (start here)
2. [`checkpoints/checkpoint_1.ipynb`](checkpoints/checkpoint_1.ipynb) — dataset selection & EDA
3. [`checkpoints/checkpoint_2.ipynb`](checkpoints/checkpoint_2.ipynb) — research-question formation & method feasibility

The two checkpoint notebooks document the development history; `main_notebook.ipynb` is the consolidated story.

---

## Key dependencies

- **Python 3.14.0** (developed in VS Code with a local `.venv`)
- `pandas==2.3.3`, `numpy==2.3.5`, `scipy==1.16.3`
- `matplotlib==3.10.8`, `seaborn==0.13.2`
- `networkx==3.6.1`, `python-louvain==0.16` *(Louvain community detection)*
- `mlxtend==0.24.0` *(FP-Growth, Apriori, association rules)*
- `prefixspan==0.5.2` *(beyond-course: sequential pattern mining)*

Full pinned list in [`requirements.txt`](requirements.txt).

---

## Repository structure

```
CSCE676-MusicMining/
├── README.md                       ← this file
├── main_notebook.ipynb             ← 👉 curated final deliverable (start here)
├── requirements.txt                ← pinned Python dependencies
├── .gitignore
│
├── checkpoints/                    ← progression of work over the semester
│   ├── checkpoint_1.ipynb          ← dataset selection + EDA
│   ├── checkpoint_2.ipynb          ← research-question formation + method feasibility
│   ├── checkpoint_1_brief.txt      ← Checkpoint 1 assignment brief (context)
│   └── checkpoint_2_brief.txt      ← Checkpoint 2 assignment brief (context)
│
├── data/                           ← raw data (gitignored — see data/README.md)
│   └── README.md                   ← download instructions
│
└── assets/                         ← saved figures from each checkpoint
    ├── figures_checkpoint_1/       ← 15 EDA plots
    └── figures_checkpoint_2/       ← 3 RQ-formation plots
```

> All analysis code lives inline in the notebooks — there is no separate `src/` package to import. If the project grows past a single notebook, helpers will be lifted out into `src/`.

---

## References

- Blondel, V. D., Guillaume, J.-L., Lambiotte, R., & Lefebvre, E. (2008). Fast unfolding of communities in large networks. *Journal of Statistical Mechanics*, P10008.
- Celma, Ò. (2010). *Music Recommendation and Discovery in the Long Tail*. Springer.
- Pei, J., Han, J., Mortazavi-Asl, B., Pinto, H., Chen, Q., Dayal, U., & Hsu, M.-C. (2001). PrefixSpan: Mining sequential patterns efficiently by prefix-projected pattern growth. *ICDE 2001*.
- Han, J., Pei, J., & Yin, Y. (2000). Mining frequent patterns without candidate generation. *SIGMOD 2000*. *(FP-Growth)*

---

*This repo is a portfolio piece. If you're a recruiter or hiring manager — thanks for visiting! The 2-minute project video at the top is the fastest tour, and `main_notebook.ipynb` is the deep dive.*
