# Spotify-Data-Analysis

Exploratory data analysis (EDA) of Spotify track and artist data using Python. The notebook cleans the data, engineers a few features, explores relationships between audio features and popularity, tracks trends over time, and builds simple rule-based playlists.

## Project Objective

Uncover insights about song attributes, artists, and genres, including:

- Which audio features (danceability, energy, tempo, acousticness, etc.) relate to song popularity
- Which artists and genres are the most popular
- How songs and their popularity have changed over the years
- Which feature ranges suit particular playlist styles (party, chill)

## Repository Structure

```
.
├── spotify_analysis.ipynb   # Main analysis notebook
├── tracks.csv               # Track-level data (not included, see Dataset)
├── artists.csv              # Artist-level data (not included, see Dataset)
└── README.md
```

## Dataset

The notebook expects two CSV files in the same folder as the notebook:

| File | Description | Columns used |
|------|-------------|--------------|
| `tracks.csv` | One row per track | `name`, `artists`, `id_artists`, `release_date`, `duration_ms`, `popularity`, `danceability`, `energy`, `loudness`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo` |
| `artists.csv` | One row per artist | `id`, `name`, `followers`, `genres` |

Both files come from publicly available Spotify metadata (the Spotify Web API or datasets built from it, such as the Spotify dataset on Kaggle). Download them and place them next to the notebook.

Audio feature definitions:

- **Popularity**: 0 to 100 score of how popular a track is
- **Danceability, Energy, Acousticness, Valence, Speechiness, Instrumentalness, Liveness**: scored 0 to 1
- **Loudness**: overall loudness in dB
- **Tempo**: speed in BPM

## Requirements

- Python 3.9+
- pandas 2.0+ (the notebook uses `to_datetime(..., format='mixed')`)
- numpy
- matplotlib
- seaborn
- jupyter

```bash
pip install numpy pandas matplotlib seaborn jupyter
```

## How to Run

```bash
git clone <your-repo-url>
cd <your-repo-folder>
# place tracks.csv and artists.csv in this folder
jupyter notebook spotify_analysis.ipynb
```

Run the cells from top to bottom. Some cells modify the data in place (`set_index`, `drop`), so restart the kernel and run all cells instead of re-running those cells individually.

## What the Notebook Does

### 1. Data Preparation
- Loads `tracks.csv` and `artists.csv`
- Inspects the data with `head()`, `shape`, `info()` and `isnull().sum()`
- Drops rows with a missing track `name`, and artists with a missing `followers` or `name`
- Counts duplicate rows in both tables
- **Feature engineering**
  - `year`: extracted from `release_date`
  - `energy_level`: energy binned into Low (0 to 0.3), Medium (0.3 to 0.7) and High (0.7 to 1)
  - `artist_rank`: artists ranked by average track popularity
  - `duration`: track length converted from milliseconds to seconds
  - `genres_list`: the artists' genre strings split into individual genres
  - `id_artists_clean`: first artist ID of each track, used to merge tracks with artists

### 2. Exploratory Data Analysis
- Descriptive statistics with `describe()`
- Correlation matrix of the audio features and popularity
- Sorted correlation of each feature with popularity
- Tempo distribution (histogram and boxplot)
- Energy vs. danceability scatter plot, colored by popularity

### 3. Visualizations
- **Bar charts**: top 10 artists by average popularity, top genres by total followers, genres by duration, and genres by popularity
- **Heatmap**: feature correlation matrix
- **Histograms**: tempo distribution, number of songs per year
- **Boxplot**: tempo
- **Scatter / regression plots**: energy vs. danceability, loudness vs. energy, popularity vs. acousticness (regression plots use a 0.4% random sample)

### 4. Trend Analysis
- Average song popularity by release year (line plot)
- Number of songs released per year

### 5. Playlist Suggestions
Rule-based filters for example playlists:

| Playlist | Rules |
|----------|-------|
| High Energy Party | danceability > 0.7, energy > 0.7, tempo > 110 BPM |
| Chill Acoustic | acousticness > 0.6, energy < 0.4 |

### 6. Popularity Lookups
- Lowest-popularity tracks
- Tracks with popularity above 90 (the most popular songs)

## Known Limitations

- `artists` in `tracks.csv` is stored as a text list, so collaborations are grouped as separate "artists" in the artist-level analysis.
- Genre-level charts after the tracks/artists merge group by the full genre list string, not by individual genre.
- The merge uses only the first artist of each track, and the ID cleaning step may not match multi-artist tracks.
- Duplicate rows are counted but not removed.
- Features are not normalized or standardized.
- Outlier detection is limited to a tempo boxplot.

## Possible Future Improvements

- Remove duplicates and normalize the audio features
- Add outlier detection (IQR or z-score) for loudness, tempo and energy
- Plot trends in danceability, energy and tempo over time
- Analyze genre popularity by year
- Filter the top-artist chart to artists with a minimum number of tracks
- Use a clustering or ML model to build playlists instead of fixed thresholds

## Tools Used

Python, pandas, NumPy, Matplotlib, Seaborn, Jupyter Notebook
