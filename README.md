# Evolving DSatur with LLMs
### LLM-Guided Design of Second-Order Heuristics for Graph Coloring

<p align="center">
  <img src="https://img.shields.io/badge/Jupyter-Notebook-orange?style=for-the-badge&logo=jupyter" alt="Jupyter Notebook">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License">
  <img src="https://img.shields.io/badge/Graph%20Coloring-Heuristics-brightgreen?style=for-the-badge" alt="Graph Coloring">
  <img src="https://img.shields.io/badge/LLM-Assisted%20Design-red?style=for-the-badge" alt="LLM Assisted">
</p>

<p align="center">
  <b>🧬 Human-in-the-Loop Evolution of Graph Coloring Heuristics using Large Language Models</b><br>
  <i>From DSatur baseline to SAT-CR: incorporating second-order neighborhood information through iterative prompt engineering</i>
</p>

---

## 📖 Overview

This project explores how **Large Language Models (LLMs)** can assist in designing and refining heuristic algorithms for the classic **Graph Coloring Problem**—an NP-complete combinatorial optimization challenge.

Building upon the seminal **DSatur algorithm** (Brélaz, 1979), we employ a human-in-the-loop "hypothesize-verify-correct" framework to iteratively evolve heuristics through **prompt engineering**. The culmination is **SAT-CR** (Saturation-Aware Crisis-Responsive), a novel heuristic that fuses lookahead strategies with crisis awareness mechanisms.

### Key Highlights

- **5-Generation Evolution**: From failed feature stacking to optimized hybrid strategy
- **Second-Order Awareness**: Incorporating neighbors-of-neighbors information into greedy decisions  
- **Comprehensive Benchmark**: 2,650 graphs across 6 categories (planar, dense/sparse random, scale-free, Mycielskian, bipartite)
- **Interpretable Design**: All heuristics derived from explicit LLM prompts, fully documented

### Core Insight

> **SAT-CR achieves 5.4% improvement over DSatur on planar graphs** (4.672 → 4.418, p=0.003) by adaptively combining:
> - **V3's squared amplification** (`Sat²`) for dense graph signal enhancement
> - **V4's crisis ratio** (`1/(Resid+1)`) for planar graph dead-end detection

---

## 🧱 Project Structure

```bash
LLM-Heuristic-Graph-Coloring/
│
├── README.md                                   # This file
├── LICENSE                                     # MIT License
├── requirements.txt                            # Python dependencies
│
├── Graph_Coloring_Heuristics_Experiment.ipynb  # ⭐ All algorithms & experiments
│
├── docs/
│   ├── Evolving_DSatur_LLM_Graph_Coloring.pdf  # GraphColoring_Paper
│   └── Graph_Coloring_LLM_Presentation.pdf     # Presentation slides
│
├── prompts/                                    # LLM prompts for each generation
│   ├── gen1_feature_stacking.md               # Gen 1: Feature stacking (failed)
│   ├── gen2_hierarchical.md                   # Gen 2: Tiered logic (stable)
│   ├── gen3_lookahead.md                      # Gen 3: Lookahead strategy
│   ├── gen4_crisis_awareness.md               # Gen 4: Crisis awareness
│   └── gen5_satcr_fusion.md                   # Gen 5: SAT-CR fusion (best)
│
└── assets/                                     # Figures & visualizations
    ├── results/
    │   ├── performance_planar_sparse_dense.png
    │   ├── performance_scalefree_myciel_bipartite.png
    │   └── execution_time_comparison.png
    └── visualizations/
        └── satcr_planar_graph_60nodes.png
```

---

## ✨ Algorithm Evolution

We document the complete iterative design process, from initial failure to final optimization:

| Generation | Strategy | Core Formula | Key Insight | Outcome |
|:----------:|----------|--------------|-------------|:---------:|
| **Gen 1** | Feature Stacking | `3·Sat + 1.2·U + 0.7·(1-CC)·Deg + 0.9·ln(S2)` | Multi-feature linear combination |  Failed |
| **Gen 2** | Hierarchical Logic | `Sat × 10⁸ + TieBreaker` | Saturation absolute dominance |  Stable |
| **Gen 3** | Lookahead | `Sat × 10⁸ + Σ Sat(v)²` | Future cost via squared neighbor saturation |  Improved |
| **Gen 4** | Crisis Awareness | `Sat × 10⁹ + Σ Sat(v)/(Resid(v)+1)` | Risk ratio for dead-end detection |  Specialized |
| **Gen 5** | **SAT-CR (Hybrid)** | `Sat × 10⁹ + Σ Sat(v)²/(Resid(v)+1)` | **Adaptive fusion of V3+V4** |  **Best** |

> 📄 **Full prompts available in [`prompts/`](prompts/) directory**

---

## 📊 Main Results

Benchmark on **2,650 graphs** (60 vertices each, 500 instances per main category):

### Performance Comparison (Average Chromatic Number)

| Graph Type | DSatur (1979) | SAT-CR (Ours) | Improvement | Statistical Significance |
|:-----------|:-------------:|:-------------:|:-----------:|:------------------------:|
| **Planar** | 4.672 | **4.418** | **-5.4%** | t=4.32, **p=0.003** |
| Dense Random (p=0.5) | 12.526 | **12.490** | -0.3% | — |
| Sparse Random (p=0.1) | 4.190 | 4.222 | +0.8% | — |
| Scale-Free (Barabási) | 4.033 | **4.033** | Optimal | — |
| Mycielskian (Trap) | 5.000 | **5.000** | Optimal | — |
| Bipartite (Sanity) | 2.000 | **2.000** | Optimal | — |

> **Lower is better.** SAT-CR excels on **structured graphs** (planar) and **high-conflict environments** (dense), with robust correctness on special graph classes.

### Execution Time Trade-off

| Algorithm | Relative Time | Complexity |
|:----------|:-------------:|:----------:|
| DSatur | 1.0× | O(n²) |
| Gen 2 (Tiered) | 5.1× | O(n²) |
| **Gen 5 (SAT-CR)** | **16.7×** | **O(n·Δ²)** |
| Gen 3 (Lookahead) | 23.1× | O(n·Δ²) |
| Gen 1 (Original) | 102.5× | O(n³) |

> SAT-CR maintains reasonable efficiency while achieving superior solution quality. The overhead comes from single-pass second-order neighborhood traversal.

---

## 🎯 Key Contributions

### 1. 🧬 Iterative LLM-Guided Design Framework

Unlike traditional "black-box" neural approaches, we employ **transparent, human-in-the-loop iteration**:

```
Analyze Failures → Craft LLM Prompt → Generate Heuristic → 
Implement & Validate → Evaluate on 6 Graph Types → Iterate
```

Each generation's **complete prompt** is preserved in [`prompts/`](prompts/), enabling reproducibility and pedagogical value.

---

### 2. 🔬 SAT-CR: Adaptive Hybrid Heuristic

**Mathematical Structure:**
```
Score(u) = Sat(u) × 10⁹  +  Σ_{v ∈ N(u)} [ Sat(v)² / (Resid(v) + 1) ]
           └─ Primary ──┘  └────────── Tie-Breaker ──────────────┘
                              ├─ V3: Squared amplification (dense graphs)
                              └─ V4: Inverse residual penalty (planar graphs)
```

**Adaptive Behavior:**
- **Dense graphs**: `Sat²` dominates numerator, counteracting denominator dilution (recovers V3 advantage)
- **Planar graphs**: Small `Resid` makes denominator significant, preserving V4's dead-end sensitivity

---

### 3. 📈 Comprehensive Empirical Validation

| Aspect | Coverage |
|--------|----------|
| **Graph Types** | 6 categories covering real-world (scale-free), synthetic (random), theoretical (Mycielskian), and geometric (planar) structures |
| **Scale** | 2,650 total instances, 500 per main category |
| **Baselines** | Random, Welsh-Powell, DSatur, plus 5 evolved generations |
| **Metrics** | Chromatic number, execution time, statistical significance (t-test) |

---

## 🚀 Quick Start

### Requirements

- Python 3.8+
- Jupyter Notebook
- NetworkX, Matplotlib, NumPy, Pandas, SciPy, tqdm

Install dependencies:

```bash
pip install -r requirements.txt
```

### Run Experiments

Launch the complete notebook:

```bash
jupyter notebook Graph_Coloring_Heuristics_Experiment.ipynb
```

The notebook includes:
1. **Algorithm implementations** – All 5 generations + 3 baselines
2. **Dataset generation** – 2,650 graphs via Delaunay triangulation & random models
3. **Benchmark execution** – Automated evaluation with progress tracking
4. **Visualization** – Box plots, runtime analysis, and coloring demonstrations

### Reproduce Paper Results

All experiments are contained in a **single notebook** for easy reproduction:

| Section | Content |
|:--------|:--------|
| Cell 1-2 | Imports & core algorithm framework |
| Cell 3 | Baseline heuristics (Random, Welsh-Powell, DSatur) |
| Cell 4 | **All 5 generations** (Gen 1-5) with detailed docstrings |
| Cell 5 | Dataset generation (planar, random, scale-free, Mycielskian, bipartite) |
| Cell 6 | **Main benchmark** – runs all algorithms on all graphs |
| Cell 7 | **Analysis & visualization** – tables, box plots, timing, case studies |

---

## 📈 Detailed Analysis

### Why SAT-CR Works: Topology-Specific Adaptation

**Planar Graphs (Structured, Low Degree)**
- Euler's formula constrains average degree → frequent local dead-ends
- V4's `1/(Resid+1)` spikes when `Resid ≈ 0`, prioritizing critical nodes
- SAT-CR preserves this sensitivity

**Dense Random Graphs (Homogeneous, High Conflict)**
- Uniform degree distribution → many saturation ties
- V3's `Sat²` provides nonlinear signal amplification for tie-breaking
- SAT-CR maintains this discrimination

**Scale-Free Networks (Heavy-Tailed)**
- Hub nodes create high-saturation clusters
- Squared term identifies "super-critical" neighbors
- SAT-CR achieves optimal 4-coloring

---

## ⚠️ Limitations & Future Work

| Limitation | Details | Future Direction |
|:-----------|:--------|:---------------|
| **Graph Size** | Experiments on 60-vertex graphs | Scale to 10³–10⁶ vertices via approximation |
| **Time Complexity** | SAT-CR is ~17× slower than DSatur | Parallel neighbor evaluation, GPU acceleration |
| **Automation** | Human analysis required between iterations | Fully automated prompt optimization (e.g., OPRO) |
| **Theoretical Guarantees** | No approximation ratio proofs | Analyze competitive ratio on specific graph classes |
| **Generalization** | Focused on graph coloring | Extend to other NP-hard problems (TSP, Max-Cut) |

---

## 📖 Citation

If you find this work useful, please cite:

```bibtex
@article{yuan2025evolving,
  title={Evolving DSatur: LLM-Guided Design of Second-Order Heuristics for Graph Coloring},
  author={Yuan, Zhouyan},
  year={2025},
  note={Undergraduate research project}
}
```

---

## 📄 License

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  <i>
    For full technical details, see the 
    <a href="paper/Evolving_DSatur_LLM_Graph_Coloring.pdf">paper</a> 
    and 
    <a href="Graph_Coloring_Heuristics_Experiment.ipynb">implementation notebook</a>.
  </i>
</p>
