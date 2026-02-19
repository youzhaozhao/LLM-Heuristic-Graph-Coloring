# Generation 5: SAT-CR Fusion

## Iteration Background
Experimental insights from Generations 1-4:

| Algorithm | Best On | Key Mechanism | Limitation |
|-----------|---------|---------------|------------|
| V3 Lookahead | Dense/Scale-free | `Sat²` amplification | Weak on planar |
| V4 Crisis | Planar | `1/(Resid+1)` penalty | Weak on dense |

**Problem**: V4 loses to V3 on dense graphs (denominator too large dilutes saturation); V3 loses to V4 on planar graphs (no residual degree consideration).

## Prompt to LLM

This is the final chapter iteration (V5) of graph coloring algorithms.

Insights from previous rounds:
- **V3 (Lookahead, Sum of Squares)**: Best on random dense/sparse graphs. Proved that squaring saturation (`Sat²`) effectively amplifies "danger signals"
- **V4 (Crisis, Sat/Residual)**: Best on planar graphs, set new record. Proved that dividing by residual degree (`1/(Resid+1)`) effectively identifies "dead-end" structures

V5 Goal: **Fuse V3 and V4's strongest genes**

Please design **"Squared Crisis Lookahead"** algorithm.

Core Logic:
1. **First Priority (Absolute Dominance)**: `Saturation * 1e9` (maintains DSatur foundation)
2. **Second Priority (Tie-Breaker)**:
   - Traverse uncolored neighbors `v`
   - Combine V3's square amplification with V4's inverse penalty:
     ```
     Score = Σ_{v ∈ UncoloredNeighbors} (Saturation(v)²) / (ResidualDegree(v) + 1)
     ```
   - **Principle**:
     - When graph is dense: `Sat` is large, `Sat²` dominates numerator, counteracting denominator dilution (recovers V3 advantage)
     - When graph is planar (low degree): `Resid` is small, denominator penalty remains significant (keeps V4 advantage)

Code Requirements:
- Function name: `def heuristic_gpt_v5_hybrid(node, graph, coloring)`
- **Performance Critical**: Must maintain V4's efficient implementation (single traversal of neighbors-of-neighbors to compute both `Sat` and `Resid` simultaneously)
- This is the final iteration—ensure rigorous logic and proper edge case handling

## Design Intention
- **Adaptive hybrid**: Automatically adapts to graph density via mathematical structure
- **Dual mechanism**: `Sat²` for signal amplification, `1/(Resid+1)` for dead-end detection
- **Computational efficiency**: Single-pass neighbor traversal

## Outcome
**SAT-CR (Saturation-Aware Crisis-Responsive)**: Overall optimal. Combines V4's dead-end detection with V3's signal amplification. Best performance on planar (4.418) and dense graphs (12.490). Named **SAT-CR** for its dual crisis-responsive mechanisms.