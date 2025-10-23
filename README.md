# Optimal Flight Route Finder (Optimal Flight Route Finder)

**Short name:** `flight-route-optimix` — Project to compute and recommend optimal flight routes based on real-world airline data and user preferences.

---

## 🔍 Overview

`Optimal Flight Route Finder` is a research/engineering project that models the global flight network as a graph and computes best routes between airports using classical graph algorithms (Dijkstra, A*), with options for multi-criteria optimization (cost, duration, number of layovers). The project demonstrates how MeTTa can be used for flexible graph representation and algorithm implementation alongside a Python-based data pipeline and optional web UI.

---

## ✨ Key Features

* Shortest-path routing (Dijkstra, A*) between origin and destination airports.
* Multi-criteria route ranking: minimize **cost**, **total duration**, or **layovers**.
* Support for user preferences and constraints (preferred airlines, max layovers, departure/arrival windows).
* Import and process real-world datasets (OpenFlights or similar).
* Graph representation using MeTTa for symbolic rules and Python for numeric computation.
* Example CLI and starter web UI (Flask/React) for interactive queries.

---

## 🛠️ Tech stack

* Core: **Python 3.10+**
* Graph & algorithms: `networkx` (or custom implementations), `heapq` for priority queues
* MeTTa: for symbolic graph representation and flexible rule-based reasoning
* Data storage: CSV / JSON for datasets; optional **MongoDB** or **Firebase** for persistence
* Optional UI: **Flask** backend + basic **React** frontend (or plain HTML templates)

---

## 📁 Repository structure (suggested)

```
flight-route-optimix/
├─ data/
│  ├─ airports.csv         # airport metadata (IATA, ICAO, lat, lon, city, country)
│  ├─ airlines.csv         # airline metadata
│  └─ routes.csv           # route edges (source, destination, airline, duration, cost)
├─ metta/
│  └─ graph.meta           # MeTTa graphs & rules
├─ src/
│  ├─ data_loader.py       # dataset parsing and normalization
│  ├─ graph_builder.py     # build graph structures (NetworkX & MeTTa)
│  ├─ algorithms.py        # Dijkstra, A*, multi-criteria ranking
│  ├─ api.py               # Flask API for queries
│  └─ cli.py               # Command-line interface
├─ web/
│  ├─ frontend/            # React app or static pages
├─ notebooks/
│  └─ analysis.ipynb       # exploratory analysis and visualizations
├─ README.md
└─ requirements.txt
```

---

## ⚙️ Installation

1. Clone the repo:

```bash
git clone https://github.com/<your-username>/flight-route-optimix.git
cd flight-route-optimix
```

2. Create a virtual environment and install dependencies:

```bash
python -m venv venv
source venv/bin/activate   # macOS / Linux
venv\Scripts\activate    # Windows
pip install -r requirements.txt
```

3. (Optional) Install MeTTa following its official instructions and place any `.meta` files under `metta/`.

---

## 🚀 Quick start (CLI)

Example command to find the cheapest route from DEL to LHR:

```bash
python src/cli.py --origin DEL --destination LHR --optimize cost --max-layovers 2
```

Expected output: ordered route options with total cost, duration, layovers, and step-by-step flight legs.

---

## 🔎 Algorithm notes

* **Dijkstra**: used for single-criterion shortest-path on a weighted graph (weight = chosen metric, e.g., duration or cost).
* **A***: uses great-circle distance (haversine) as heuristic to speed up search for distance/duration optimization.
* **Multi-criteria**: implement Pareto-front filtering or weighted-sum scoring to rank candidate paths when optimizing for multiple objectives.
* **Layover modeling**: layover time is computed using scheduled arrival/departure times in the dataset; enforce minimum connection time (MCT) when building feasible paths.

---

## 🧠 MeTTa integration (suggested)

* Store symbolic facts like `(airport DEL)`, `(route DEL LHR airline delta duration 350 cost 120)`.
* Write rule templates to expand edges or infer alternative connections (e.g., transits via hub airports).
* Use MeTTa to express high-level preference rules (e.g., prefer alliances, avoid red-eye flights) and let Python evaluate numeric scores.

---

## 🧪 Data sources

* **OpenFlights**: airports, airlines, and routes datasets (CSV) — good for offline prototypes.
* **AviationEdge / OpenSky / Official APIs**: for more up-to-date schedules and live data (may require API keys).

> Note: Check terms of service of any dataset/API used.

---

## ✅ Usage examples

1. **Minimal example (Python)**

```python
from src.data_loader import load_airports, load_routes
from src.graph_builder import build_graph
from src.algorithms import find_best_routes

airports = load_airports('data/airports.csv')
routes = load_routes('data/routes.csv')
G = build_graph(airports, routes)

results = find_best_routes(G, origin='DEL', destination='LHR', optimize='cost', max_layovers=2)
for r in results[:3]:
    print(r)
```

2. **API request (Flask)**

```
GET /route?origin=DEL&destination=LHR&optimize=duration&max_layovers=1
```

---

## 🧩 Extensibility ideas

* Add live pricing and seat-availability by integrating airline/OTA APIs.
* Add user accounts, saved preferences, and history.
* Visualize routes on a world map using `folium` or D3.js.
* Integrate delay/real-time tracking from OpenSky for robust ETAs.

---

## 🤝 Contribution

Contributions are welcome. Please open issues or pull requests. Follow these steps:

1. Fork the repo
2. Create a branch: `feature/my-feature`
3. Commit and push
4. Open a PR with description and tests/examples

---

## 📄 License

This project is released under the **MIT License**. See `LICENSE` for details.

---

## 📞 Contact

Project owner: Deepak Gupta

If you want the README tailored for a specific audience (hackathon submission, GitHub portfolio, or college project report), tell me and I'll adapt it quickly.
