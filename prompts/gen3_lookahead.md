# Generation 3: Lookahead Strategy

## Iteration Background
- Gen2 (tiered logic) performed stably, matching DSatur
- Goal: push physical limits by introducing "future state" information
- Challenge: go beyond static features to dynamic lookahead

## Prompt to LLM

I am conducting final optimization for graph coloring algorithms (V3 version). Your previous version (V2, tiered logic) performed very stably, already matching DSatur.

Now I want to challenge the limit. Please design a heuristic rule based on **"Future Connectivity Impact"**.

Core Logic:
1. **Absolute Defense**: Keep `primary = saturation * 1e8` as our safety net against losing to DSatur
2. **New Tie-Breaking Idea (Look-ahead)**:
   - Don't just count neighbor degrees
   - **Key Metric**: For current node `u`, compute its uncolored neighbor set `N(u)`
   - For each uncolored neighbor `v ∈ N(u)`, compute `v`'s current saturation `Sat(v)`
   - **Intuition**: If my neighbor `v` already has high saturation (running out of color choices), I must handle myself quickly—otherwise after I take a color, `v` may be deadlocked
3. **Scoring Formula**: `Score = Primary + Σ_{v ∈ UncoloredNeighbors} (Sat(v)^2)` (Squaring amplifies weight of "extremely dangerous" neighbors)

Code Requirements:
- Function name: `def heuristic_gpt_v3_lookahead(node, graph, coloring)`
- Use NetworkX
- Note: Computing neighbor saturation requires traversing neighbors-of-neighbors—slightly higher complexity but worth it for accuracy. Write efficiently.

## Design Intention
- Transition from static hierarchy to dynamic lookahead
- Nonlinear amplification (squaring) widens weight gaps between high/low saturation neighbors
- Resembles one-step lookahead in dynamic programming

## Outcome
**Success**: Robust improvement over DSatur. Achieved optimal 4-coloring on scale-free networks. Significant gains on planar graphs (4.654→4.480). Validated effectiveness of second-order neighborhood information.