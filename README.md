# 🚗 Cairo Smart City Transportation Network Optimization

**CSE112 — Design & Analysis of Algorithms**
Alamein International University · Faculty of Computer Science & Engineering

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Algorithms Implemented](#algorithms-implemented)
- [Setup & Installation](#setup--installation)
- [Running on Google Colab](#running-on-google-colab)
- [Usage](#usage)
- [Phase Breakdown](#phase-breakdown)
- [Technical Report Summary](#technical-report-summary)
- [Bonus Features](#bonus-features)
- [Team](#team)

---

## 🌍 Project Overview

This project implements a **comprehensive transportation optimization system** for the **Greater Cairo metropolitan area**. Using real geographic and traffic data for Cairo's 15 districts and 10 key facilities, the system applies advanced algorithms from CSE112 to solve real-world urban mobility problems:

- Finding shortest routes between neighborhoods
- Designing cost-efficient road infrastructure
- Routing emergency vehicles to hospitals
- Optimizing public bus and metro schedules
- Predicting traffic congestion using machine learning

The system covers a network of **25 nodes** (districts + facilities), **27 existing roads**, and **15 potential new roads** with real-world traffic flow data across four time periods (morning peak, afternoon, evening peak, night).

---

## ✨ Features

| Feature | Algorithm | Status |
|---|---|---|
| Infrastructure network design | Kruskal's MST | ✅ Phase 2 |
| Standard route planning | Dijkstra's | ✅ Phase 2 |
| Emergency vehicle routing | A\* Search | ✅ Phase 2 |
| Rush-hour aware routing | Time-variant Dijkstra | ✅ Phase 2 |
| Transit schedule optimization | Dynamic Programming | ✅ Phase 2 |
| Road maintenance allocation | DP Knapsack | ✅ Phase 2 |
| Traffic signal optimization | Greedy Algorithm | ✅ Phase 2 |
| ML traffic congestion forecast | scikit-learn | 🔄 Bonus |
| Dijkstra vs A\* race animation | Side-by-side visualizer | 🔄 Bonus |
| Live web demo | Deployed on Vercel | 🔄 Bonus |
| Docker containerization | Docker | 🔄 Bonus |

---

## 📁 Project Structure

```
cairo_transport/
│
├── src/
│   ├── __init__.py              # Public API exports
│   ├── data_structures.py       # Node, Edge, TrafficData, CairoTransportGraph
│   ├── data_loader.py           # Parses all project data → builds the graph
│   ├── public_transport.py      # MetroLine, BusRoute, TransportDemand
│   ├── algorithms/
│   │   ├── mst.py               # Kruskal's MST with critical facility constraints
│   │   ├── dijkstra.py          # Standard + time-variant Dijkstra
│   │   ├── astar.py             # A* with Haversine heuristic
│   │   ├── dynamic_programming.py  # DP scheduling + maintenance allocation
│   │   └── greedy.py            # Traffic signal optimizer
│   └── ml/
│       └── traffic_predictor.py # scikit-learn congestion forecasting (Bonus)
│
├── tests/
│   ├── test_phase1.py           # Data structures & graph tests
│   └── test_algorithms.py      # Algorithm correctness tests
│
├── demo/
│   └── app.py                  # Interactive web demo (Flask / Streamlit)
│
├── report/
│   └── technical_report.pdf    # Full technical report
│
├── notebooks/
│   └── CSE112_Phase1_Colab.ipynb  # Google Colab notebook
│
├── Dockerfile                  # Container setup (Bonus)
├── requirements.txt            # Python dependencies
└── README.md
```

---

## 📊 Dataset

All data is sourced from the **CSE112 Project Provided Data** for the Greater Cairo metropolitan area.

### Districts (15 nodes)

| ID | Name | Population | Type |
|---|---|---|---|
| 1 | Maadi | 250,000 | Residential |
| 2 | Nasr City | 500,000 | Mixed |
| 3 | Downtown Cairo | 100,000 | Business |
| 4 | New Cairo | 300,000 | Residential |
| 5 | Heliopolis | 200,000 | Mixed |
| 7 | 6th October City | 400,000 | Mixed |
| 8 | Giza | 550,000 | Mixed |
| 13 | New Administrative Capital | 50,000 | Government |
| ... | *(15 districts total)* | | |

### Key Facilities (10 nodes)

| ID | Name | Type |
|---|---|---|
| F1 | Cairo International Airport | Airport |
| F2 | Ramses Railway Station | Transit Hub |
| F9 | Qasr El Aini Hospital | Medical ⚠️ Critical |
| F10 | Maadi Military Hospital | Medical ⚠️ Critical |
| F7 | Smart Village | Business |
| ... | *(10 facilities total)* | |

### Road Network

- **27 existing roads** with distance, capacity, condition rating (1–10), and traffic flow data
- **15 potential new roads** with construction cost (Million EGP)
- **Traffic data** per road: morning peak / afternoon / evening peak / night (vehicles/hour)

### Public Transportation

- **3 metro lines** (Lines 1, 2, 3) serving 3.5M+ daily passengers combined
- **10 bus routes** (B1–B10) with assigned fleet sizes
- **17 origin-destination demand pairs**

---

## ⚙️ Algorithms Implemented

### A. Minimum Spanning Tree — Kruskal's Algorithm

Designs a cost-efficient road network connecting all 25 nodes while:
- Minimizing total construction and maintenance cost
- Guaranteeing connectivity for **critical facilities** (hospitals, airport, railway station, government capital)
- Considering both existing roads and potential new constructions

**Complexity:** O(E log E) for sorting + O(E α(V)) for Union-Find operations

### B. Shortest Path — Dijkstra's Algorithm

Finds optimal routes between any two locations using:
- **Basic mode:** edge weight = road distance
- **Condition-adjusted mode:** penalises poor-quality roads — weight = distance × (11 - condition) / 10
- **Time-variant mode:** accounts for rush-hour congestion — weight scales with flow/capacity ratio

**Complexity:** O((V + E) log V) with a binary heap priority queue

### C. Shortest Path — A\* Search

Used for **emergency vehicle routing** to medical facilities with:
- Haversine geodetic distance as the admissible heuristic
- Priority preemption system at major intersections
- Guaranteed to find the optimal path faster than Dijkstra in practice

**Complexity:** O(E log V) with geographic heuristic

### D. Dynamic Programming — Transit Scheduling

Optimizes the allocation of buses and metro trains across routes by:
- Modelling the problem as a bounded knapsack (maximize daily passengers subject to fleet constraints)
- Using memoization to cache sub-problem solutions
- Producing a schedule that maximizes coverage and minimizes travel time

**Complexity:** O(routes × fleet_size)

### E. Dynamic Programming — Road Maintenance

Allocates a fixed maintenance budget across roads with poor condition ratings:
- Selects the optimal subset of roads to repair within budget
- Weights by population served and current condition score
- Uses bottom-up DP table construction

### F. Greedy — Traffic Signal Optimization

At each major intersection, greedily assigns green-light time proportional to incoming traffic volume:
- Processes intersections sorted by total congestion (highest first)
- Emergency preemption overrides during high-congestion periods
- Analyzes both optimal and suboptimal cases in the Cairo context

---

## 🛠️ Setup & Installation

### Prerequisites

- Python 3.10+
- pip

### Install dependencies

```bash
git clone https://github.com/your-username/cairo-transport.git
cd cairo-transport
pip install -r requirements.txt
```

### requirements.txt

```
numpy>=1.24
pandas>=2.0
scikit-learn>=1.3
matplotlib>=3.7
networkx>=3.1
pytest>=7.4
flask>=3.0          # for demo
```

### Run verification

```bash
python -c "from src import build_cairo_graph; g = build_cairo_graph(); print(g.summary())"
```

Expected output:
```
========================================================
  Cairo Transportation Network — Graph Summary
========================================================
  Nodes      : 25 (15 districts, 10 facilities)
  Edges      : 42 total
    Existing : 27
    Potential: 15

  Critical nodes (guaranteed connectivity):
    [F9]  Qasr El Aini Hospital
    [F10] Maadi Military Hospital
    [F2]  Ramses Railway Station
    [F1]  Cairo International Airport
    [13]  New Administrative Capital
========================================================
```

### Run all tests

```bash
pytest tests/ -v
```

---

## 🟡 Running on Google Colab

Open [Google Colab](https://colab.research.google.com) and run these cells in order:

**Cell 1 — Setup**
```python
!git clone https://github.com/your-username/cairo-transport.git
%cd cairo-transport
!pip install -r requirements.txt -q
```

**Cell 2 — Build and inspect the graph**
```python
import sys
sys.path.insert(0, '/content/cairo-transport')

from src import build_cairo_graph
graph = build_cairo_graph()
print(graph.summary())
```

**Cell 3 — Congestion analysis**
```python
print("Top 5 Most Congested Roads — Morning Rush Hour")
for edge, ratio in graph.most_congested_roads("morning", top_n=5):
    src = graph.get_node(edge.from_id).name
    dst = graph.get_node(edge.to_id).name
    print(f"  {src} → {dst}: {ratio:.1%}  [{edge.congestion_level('morning')}]")
```

**Cell 4 — Run tests**
```python
!pytest tests/test_phase1.py -v
```

> 💡 **Tip:** If you see `ModuleNotFoundError: No module named 'src'`, re-run Cell 1 to ensure the path is set correctly.

---

## 🚀 Usage

### Build the graph

```python
from src import build_cairo_graph

# Full graph (existing + potential roads)
graph = build_cairo_graph()

# Existing roads only
graph = build_cairo_graph(include_potential=False)
```

### Find shortest path (Dijkstra)

```python
from src.algorithms.dijkstra import dijkstra

path, cost = dijkstra(graph, start="1", end="13")  # Maadi → New Admin Capital
print(f"Route: {' → '.join(path)}")
print(f"Cost : {cost:.2f} km")
```

### Route an emergency vehicle (A\*)

```python
from src.algorithms.astar import astar

path, cost = astar(graph, start="7", goal="F9")   # 6th Oct City → Qasr El Aini Hospital
print(f"Emergency route: {' → '.join(path)}")
```

### Time-dependent routing

```python
from src.algorithms.dijkstra import dijkstra_time_dependent

path, cost = dijkstra_time_dependent(graph, start="2", end="3", period="morning")
print(f"Morning rush route (Nasr City → Downtown): {' → '.join(path)}")
```

### Run MST network design

```python
from src.algorithms.mst import kruskal_mst

mst_edges, total_cost = kruskal_mst(graph)
print(f"MST connects all 25 nodes with {len(mst_edges)} edges")
print(f"Total cost: {total_cost:.2f}")
```

### Optimize transit schedule (DP)

```python
from src.algorithms.dynamic_programming import optimize_transit_schedule
from src import load_public_transport

metro, buses, demand = load_public_transport()
schedule = optimize_transit_schedule(buses, total_fleet=264)
print(schedule)
```

---

## 📅 Phase Breakdown

| Phase | Description | Status |
|---|---|---|
| **Phase 1** | Data structures & graph representation | ✅ Complete |
| **Phase 2** | Core algorithm implementations (MST, Dijkstra, A\*, DP, Greedy) | 🔄 In progress |
| **Phase 3** | Visualization & interactive UI demo | 🔄 In progress |
| **Phase 4** | Bonus features (ML, race animation, deployment, Docker) | 🔄 Planned |
| **Phase 5** | Technical report & theoretical analysis | 🔄 In progress |

---

## 📝 Technical Report Summary

The full technical report (PDF) covers:

1. **System Architecture** — graph representation design decisions, data structure choices
2. **Algorithm Analysis** — time/space complexity for all 6 algorithm implementations
3. **Performance Evaluation** — benchmarks comparing Dijkstra vs A\* on emergency scenarios
4. **Theoretical Deep Dive** — selected algorithm: **Dijkstra's Algorithm**
   - Formal proof of correctness (greedy safe-edge theorem)
   - Detailed O((V+E) log V) complexity derivation
   - Modifications made for time-varying Cairo traffic
   - Comparison with A\*, Bellman-Ford, and Floyd-Warshall
5. **Challenges & Solutions** — handling the disconnected New Administrative Capital, critical facility constraints in MST
6. **Future Work** — real-time data integration, reinforcement learning for signal control

---

## 🎁 Bonus Features

### ML Traffic Prediction (2 marks)

Trains a **Random Forest Regressor** on the temporal traffic flow data to predict congestion levels for any road at any time period.

```python
from src.ml.traffic_predictor import TrafficPredictor

predictor = TrafficPredictor()
predictor.train(graph)
predicted_flow = predictor.predict(road_id="3-5", period="morning")
```

### Dijkstra vs A\* Race Visualizer (2 marks)

An animated side-by-side comparison showing nodes explored by each algorithm in real time, demonstrating how A\*'s heuristic dramatically reduces the search space for emergency routing.

### Live Web Deployment (3 marks)

The demo is deployed at: **[https://cairo-transport.vercel.app](https://cairo-transport.vercel.app)**

### Docker Container (3 marks)

```bash
docker build -t cairo-transport .
docker run -p 5000:5000 cairo-transport
```

---

## 👥 Team

| Name | Student ID | Role |
|---|---|---|
| *(Your Name)* | *(ID)* | Full Stack |

**Course:** CSE112 — Design & Analysis of Algorithms
**Instructor:** Eng. Ahmed M. Yahia
**University:** Alamein International University

---

## 📜 License

This project was developed for academic purposes as part of the CSE112 course at Alamein International University.
