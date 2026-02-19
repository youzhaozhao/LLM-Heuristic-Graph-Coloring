# Generation 2: Hierarchical Logic

## Iteration Background
- Gen1 failed: no heuristic beat DSatur
- Root cause: introducing too many features diluted saturation's weight
- Key insight: "saturation" is the absolute core feature

## Prompt to LLM

I am conducting comparative research on graph coloring algorithms. Current results show that no heuristic beats classic DSatur (prioritizing highest saturation). This indicates saturation is the absolute core feature. Previous attempts failed because excessive noise diluted saturation's weight.

Please design a strongly improved scoring function (Python) targeting **marginal improvement over DSatur on dense or complex planar graphs**.

Core Design Logic:
1. **Tiered Logic**: Saturation must be first priority (give extreme weight, e.g., 1e6)
2. **Intelligent Tie-Breaking**: When saturation ties, don't just look at degree. Introduce a "future hazard" metric: if I pick this node, how many of its uncolored neighbors are "high-degree" or "highly connected"?
3. **Common Neighbors**: Consider connection density among uncolored neighbors (if my neighbors are mutually connected, they are hard to color, so I should handle them first)

Output Requirements:
- Function signature: `def heuristic_gpt_improved(node, graph, coloring)`
- Use NetworkX
- Efficient code, avoid unnecessary O(N³) computation

## Design Intention
- Restore saturation's absolute dominance via hierarchical structure
- Introduce "future hazard" for smarter tie-breaking
- Maintain computational efficiency

## Outcome
**Stable**: Matched DSatur performance with minor fluctuations. Limited improvement on planar graphs—tie-breaker based on high-degree neighbor counting lacked sufficient discrimination.