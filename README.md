TMDB Movie Data Analysis

A movie data pipeline built with Python and Pandas. It pulls data for 18 blockbuster films from the TMDB API, cleans it, calculates financial and popularity KPIs, compares franchises and directors, and visualizes the results.

What's in here
tmdb-movalyzer/
├── notebooks/
│   └── tmdb_analysis.ipynb      # the full walkthrough, with explanations
├── data/
│   ├── raw/movies_raw.json      # untouched API responses
│   └── processed/movies_clean.csv
├── reports/
│   ├── TMDB_Movie_Analysis_Findings.pdf
│   └── figures/                 # charts used in the report
├── requirements.txt
└── README.md

Running it yourself
Clone the repo and create a virtual environment
bash
   git clone https://github.com/CasualLev/tmdb-movalyzer.git
   cd tmdb-movalyzer
   python -m venv .venv

Activate it with .venv\Scripts\Activate.ps1 on Windows or source .venv/bin/activate on macOS/Linux.

Install the dependencies
bash
   pip install -r requirements.txt
Add your TMDB API key. Create a free account at themoviedb.org, request an API key under Settings → API, and put it in a .env file in the project root:
   TMDB_API_KEY=your_key_here

The .env file is ignored by git, so the key never ends up on GitHub.

Open notebooks/tmdb_analysis.ipynb in VS Code or Jupyter, select the .venv kernel, and run all cells. No API key? The raw data is already in data/raw/movies_raw.json, so you can skip the fetch cell and load that file instead.
How it works
Extract. Each movie is requested from /movie/{id} with append_to_response=credits to get cast and crew in the same call. Invalid IDs (like 0 in the list) are skipped and logged instead of crashing the run.
Clean. Nested JSON fields are flattened into |-separated strings, budgets and revenues are converted to millions of USD, zeros and placeholder text are treated as missing, and only released movies are kept.
Analyze. Profit and ROI are calculated, movies are ranked with one reusable rank_movies() function, and franchises, standalone films and directors are compared with groupby.
Visualize. Five charts cover revenue vs. budget, ROI by genre, popularity vs. rating, yearly box office, and franchise vs. standalone.
Main findings
Every movie earned at least about 4x its budget, but a bigger budget didn't reliably mean more revenue.
Action and Science Fiction were the most common genres but had the lowest typical ROI.
Popularity and rating moved together fairly strongly (r = 0.74), with the Avengers films at the top of both.
Franchise and standalone movies performed similarly, but there were only two standalone films, so that comparison is weak.

The full write-up, including side findings and limitations, is in reports/TMDB_Movie_Analysis_Findings.pdf.

Limitations

This is a small, hand-picked set of blockbusters, so the results describe big hits, not movies in general. Revenues aren't adjusted for inflation, and TMDB's popularity score is a snapshot that changes over time.