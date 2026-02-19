# Generation 4: Crisis Awareness

## Iteration Background
- Gen3 (lookahead) beat DSatur on planar graphs but improvement marginal on dense graphs
- High computational cost of Gen3
- Goal: identify "neighbors with no way out" precisely without excessive calculation

## Prompt to LLM

I am conducting the ultimate iteration (V4) of graph coloring algorithms.

Current Status:
- V3 (lookahead based on sum of squared neighbor saturations) beats DSatur on planar graphs
- But on dense graphs, V3's improvement is marginal and computational cost is too high

V4 Goal:
I need a **"Crisis-Aware DSatur"**—a scoring function that precisely identifies nodes whose "neighbors are about to be cornered," while avoiding V3's over-computation.

Core Graph Theory Logic:
1. **First Priority (Absolute Dominance)**: Saturation—this is the baseline
2. **Second Priority (Tie-Breaker)**: Introduce "Neighbor Crisis Score"
   - For current node `u`, traverse each uncolored neighbor `v`
   - Compute `v`'s "crisis degree":
     ```
     Crisis(v) = Saturation(v) / (ResidualDegree(v) + 1)
     ```
     where `ResidualDegree(v)` is `v`'s current count of uncolored neighbors
   - **Intuition**: If a neighbor has high saturation and few remaining uncolored neighbors (low residual degree), it's about to deadlock. Prioritizing current node `u` can fix `v`'s constraints early, preventing backtracking

Code Requirements:
- Function name: `def heuristic_gpt_v4_crisis(node, graph, coloring)`
- Performance optimization: Use set operations (e.g., `len(set(graph.neighbors(v)) - set(coloring.keys()))`) or traversal optimization when computing `ResidualDegree`—must be faster than V3
- Formula: `Score = Saturation * 1e9 + Sum(Crisis(v) for v in UncoloredNeighbors)`

## Design Intention
- Shift from "absolute danger" (saturation) to "relative risk" (saturation/residual)
- Detect local dead-ends in sparse graphs (planar)
- Optimize computational efficiency

## Outcome
**Specialized**: Excellent on planar graphs (4.424, best result), but regressed on sparse random graphs. Denominator (residual degree) introduced noise where no geometric dead-ends exist. Revealed need for adaptive hybrid approach.