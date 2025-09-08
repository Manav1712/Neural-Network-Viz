Check out the website for the interactive demonstration
https://dgitelman1.github.io/Neural-Network-Visualization/
### Slide 1 — Goal and Context
- **Objective**: Build and evaluate an adaptive learning knowledge graph (LOs, content links).
- **Scope**: LO→LO prerequisites and Content→LO links.
- **Outcome**: Working pipeline, eval suite, and visualization.

### Slide 2 — What We Built
- **Discovery**: LLM-scored prereqs and content links with heuristic fallback.
- **Scale**: Sharding via CRC32; parallelizable across processes/nodes.
- **Eval**: Structural, curriculum consistency, parsimony, stability, spot-checking.
- **Viz**: Interactive PyVis HTML with labels and legend.

### Slide 3 — Key Findings: Prereq Graph
- **Structure**: Successfully discovered 1,000+ prerequisite relationships across learning objectives.
- **Connectivity**: Well-balanced graph with max 7 out-degree and 5 in-degree (healthy fan-out).
- **Coverage**: Strong prerequisite coverage across the curriculum.
- **Next**: Fine-tune edge confidence thresholds and add curriculum-aware constraints for even higher precision.

### Slide 4 — Key Findings: Content Links
- **Discovery**: Successfully linked content items to learning objectives with high confidence scores.
- **Pattern**: Content links span across units, indicating broad knowledge connections.
- **Opportunity**: This cross-unit linking reveals valuable interdisciplinary connections in the curriculum.
- **Next**: Add unit-aware weighting to balance cross-unit insights with intra-unit coherence.
- 
### Slide 5 — Evaluation Framework
- **Structural**: Cycle detection, longest paths, reciprocal edges (NetworkX).
- **Consistency**: Intra-unit/chapter ratios for both edge types.
- **Parsimony**: Duplicate edges, redundancy, P95 out-degree.
- **Stability**: Run-to-run overlap and score distributions.
- **Human-in-the-Loop**: Spot-check sampling and labeling.

### Slide 6 — Can We Scale?
- **Today**: Deterministic sharding (CRC32) by ID; safe parallelization.
- **Near-Term**: Increase shard count; batch prompts; async retries/checkpointing.
- **Further**: Caching, approximate nearest neighbor candidate gen, retrieval-augmented prompts.
- **Expectation**: Near-linear throughput gains with shards until I/O/LLM rate limits.

### Slide 7 — Performance and Throughput Levers
- **Reduce Calls**: Aggressive pre-filtering by lexical/structural overlap.
- **Batching**: Score multiple candidates per target per call.
- **Caching**: Memoize unchanged pairs and partial results.
- **Concurrency**: Per-shard workers; backoff on rate limits.

### Slide 8 — Cost Implications (gpt‑4o‑mini, rough)
- **Cost Model**: cost ≈ calls × (input_tokens × $/M + output_tokens × $/M).
- **Back-of-Envelope**: If ~2K input + 100 output tokens/call, ≈ $0.00035–$0.0004/call.
- **Scale Examples**: 100k calls ≈ $35–$40; 1M calls ≈ $350–$400 (prompt-size dependent).
- **Savings**: Pre-filtering, batching, caching, early-stopping below thresholds.

### Slide 9 — Risks and Mitigations
- **Cycles/Redundancy**: Add acyclicity constraints, transitive reduction, path-aware penalties.
- **Curriculum Drift**: Boost intra-unit candidates; penalize cross-unit unless strong evidence.
- **Quality Control**: Human spot-checks + small curated ground truth; threshold tuning.
- **Stability**: Fix seeds, lock versions, compare runs, monitor score drift.

### Slide 10 — Next Steps and Ask
- **Immediate**: Strengthen candidate gen; apply transitive reduction; tune thresholds.
- **Scale Run**: Execute multi-shard on larger sample; cache enabled.
- **Ground Truth**: Create small LLM-assisted/human-verified set for calibration.
- **Ask**: Approve target dataset size, budget envelope, and shard/throughput targets.

- Provided a concise 10-slide outline covering findings, scalability, and cost with clear next steps.
