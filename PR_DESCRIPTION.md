## Summary

Feasibility gate for **SAMRI** (frozen SAM ViT-B encoder + fine-tuned decoder) toward OpenVINO on Core Ultra / Xeon.

**Outcome: BLOCKED before smoke / OpenVINO.** Docs-only branch — see [`BLOCKER.md`](./BLOCKER.md).

## Verified licenses / weights

| Asset | Finding |
|-------|---------|
| Code | Root `LICENSE` = **GPL-3.0**; GitHub SPDX = **GPL-3.0**; `setup.py` + README claim **Apache-2.0** → **conflict** |
| SAMRI weights | UQ eSpace [UQ:cadac84](https://espace.library.uq.edu.au/view/UQ:cadac84) / DOI [10.48610/cadac84](https://doi.org/10.48610/cadac84); licence **OTHER** (UQ reuse agreement; standard UQ data terms lean **non-commercial**); ZIP downloads **403** from this environment |
| Meta SAM ViT-B | Public FB URL reachable; encoder-only, not SAMRI decoder |

## Work not executed (by design)

- CPU reference inference / OpenVINO encoder+decoder / agreement / one-command bench — skipped per DoD when license or weights block a clean product win. Weights were **not** invented.

## M4 Pro baseline (Labs; for later comparison)

- 100 slices: **50.43 s** image-to-embedding, **55.09 s** image-to-mask, **~4.4 GB**  
- Box benches (if added later) must be labeled **box-only / non-target**. Core Ultra / Xeon iron is out of scope for this agent.

## Test plan

- [ ] Resolve code license conflict with upstream  
- [ ] Obtain commercial-clean, downloadable SAMRI checkpoints  
- [ ] Pin env; CPU smoke on `user_data/Datasets/demoSample/`  
- [ ] OpenVINO ViT-B encoder (+ decoder attempt); `--device CPU|GPU|NPU`  
- [ ] Cosine / Dice·IoU agreement vs torch  
- [ ] One-command latency mean/p50/p90 + peak RSS  

## Draft only

Fork-only draft PR. **Do not merge. Do not push to upstream.**
