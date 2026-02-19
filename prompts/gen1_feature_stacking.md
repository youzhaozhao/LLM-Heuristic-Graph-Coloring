# Generation 1: Feature Stacking

## Iteration Background
Initial exploration phase. Goal: test whether combining multiple graph structural features (clustering coefficient, neighbor degree, second-order neighbors) can improve upon DSatur.

## Prompt to LLM

I am working on a graph theory research project comparing heuristic algorithms for the graph coloring problem. DSatur (prioritizing nodes with highest saturation) is known to perform well.

Please design **2 innovative node priority scoring functions** using graph theory concepts (clustering coefficient, average neighbor degree, second-order neighbor information, etc.).

Requirements:
- Function signature: `def func(node, graph, coloring)`
- Return a float score (higher = priority for coloring)
- Use NetworkX functions (e.g., `nx.clustering(graph, node)`)
- Design a complex rule considering "local topological structure"
- Handle potential division-by-zero errors

## Design Intention
- Explore rich feature engineering approach
- Test if more information leads to better decisions
- Establish baseline for subsequent iterations

## Outcome
**Failed**: Linear feature stacking diluted saturation's dominance. Higher computational cost (O(N³) for clustering) without quality improvement. Led to key insight: complexity ≠ effectiveness.