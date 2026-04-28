# Data

This project uses the **Last.fm Dataset 1K** (Celma, 2010) — 19.15 M timestamped listening events from 992 users, plus user demographic profiles.

The raw archive is **~672 MB compressed / ~2.4 GB unpacked**, so it is not committed to this repository.

## Download

From the project root:

```bash
curl -L -o data/lastfm-dataset-1K.tar.gz \
  "http://mtg.upf.edu/static/datasets/last.fm/lastfm-dataset-1K.tar.gz"
tar xzf data/lastfm-dataset-1K.tar.gz -C data/
```

After extraction, the layout should be:

```
data/
├── lastfm-dataset-1K.tar.gz
└── lastfm-dataset-1K/
    ├── README.txt
    ├── userid-profile.tsv
    └── userid-timestamp-artid-artname-traid-traname.tsv
```

All three notebooks (`main_notebook.ipynb`, `checkpoints/checkpoint_1.ipynb`, `checkpoints/checkpoint_2.ipynb`) reference this layout via a `DATA_DIR` variable near the top of their data-loading cell — no further configuration is needed.

## Source & license

- **Original page:** http://ocelma.net/MusicRecommendationDataset/lastfm-1K.html
- **Mirror:** http://mtg.upf.edu/static/datasets/last.fm/lastfm-dataset-1K.tar.gz
- **License:** Non-commercial research use (per the dataset's published terms).
- **Citation:** Celma, Ò. (2010). *Music Recommendation and Discovery in the Long Tail*. Springer.
