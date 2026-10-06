# BLOCKER: SAMRI not a clean product candidate (license + weights)

**Date:** 2026-10-06 (PT)  
**Repo:** https://github.com/wangzhaomxy/SAMRI  
**Paper:** https://arxiv.org/html/2510.26635v2  
**Decision:** Stop before env pin / CPU smoke / OpenVINO / inventing weights. Docs-only draft PR.

## 1. Code license — conflict (verified)

| Source | Claimed / actual |
|--------|------------------|
| Root `LICENSE` file | Full **GNU GPL v3** text (674 lines) |
| GitHub API / UI | `GPL-3.0` (`spdx_id: GPL-3.0`) |
| `setup.py` `license=` | `Apache-2.0` |
| README “License” section | “Apache 2.0 License (or specify otherwise)” → points at the GPL-3 `LICENSE` file |
| Vendored `segment_anything/` | Meta SAM headers (“license found in the LICENSE file in the root”); Meta SAM upstream is Apache-2.0, but this tree’s root LICENSE is GPL-3 |

**Impact:** SPDX/GitHub say GPL-3; packaging/docs say Apache-2.0. Unresolved conflict blocks treating this as a clean, redistributable product baseline. GPL-3 copyleft alone is also incompatible with typical proprietary product packaging even if the Apache claims were authoritative.

## 2. Weights — host OK-ish, license not product-clean; download blocked here

| Item | Status |
|------|--------|
| Host | UQ eSpace record **UQ:cadac84**, DOI [10.48610/cadac84](https://doi.org/10.48610/cadac84); also indexed at [researchdata.edu.au/samri/3806572](https://researchdata.edu.au/samri/3806572) |
| Access metadata | Listed **Open**; four ~348 MB ZIPs: `samri_vitb_box`, `samri_vitb_box_zero`, `samri_vitb_bp`, `samri_vitb_bp_zero` |
| Stated licence | **OTHER** — links to UQ “license / reuse data agreement” (guide URL 404s; record still cites it) |
| UQ standard data reuse | UQ Library IP/copyright guidance: standard eSpace data licensing requires acknowledgment and **restricts reuse to non-commercial purposes** ([UQ Library](https://web.library.uq.edu.au/research-and-publish/publish/intellectual-property-and-copyright)) |
| Automated download (this box) | Direct ZIP URLs under `espace.library.uq.edu.au` return **HTTP 403** (CloudFront / SPA compatibility wall). Landing page also 403 to non-browser clients. |
| Meta SAM ViT-B (encoder only) | Separately available: `https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth` (HTTP 200, ~375 MB). Useful for encoder-only experiments, **not** a substitute for SAMRI fine-tuned decoder weights. |

**Impact:** Fine-tuned SAMRI checkpoints are not under a clear Apache/MIT/CC-BY-style grant suitable for commercial product redistribution. Host is institutional Open Access in name, but rights are “Other” / non-commercial-leaning UQ terms, and weights were not retrievable from this automation environment. Per DoD: **do not invent weights**.

## 3. What was *not* run (intentionally)

- No pinned env install beyond clone inspection  
- No reference CPU smoke (encoder embeddings / image-to-mask)  
- No OpenVINO export/compile for ViT-B encoder or decoder  
- No agreement metrics (cosine / Dice / IoU)  
- No latency / RSS bench (box-only would be non-target anyway)

Demo NIfTI samples exist under `user_data/Datasets/demoSample/` (shapes suitable later: single-slice MRI-like). They were not used for inference without licensed checkpoints.

## 4. Unblock criteria (for a future clean win)

1. Authors clarify and align **one** code license (prefer Apache-2.0 matching README/setup **or** explicit GPL-3 everywhere) and replace the conflicting `LICENSE` / GitHub SPDX.  
2. SAMRI checkpoints re-hosted or re-licensed under an explicit **commercial-friendly** OSS/data license (or a signed commercial grant), with stable machine-downloadable URLs.  
3. Then: pin env → CPU reference on demo/synthetic → OpenVINO encoder (priority) + decoder attempt → agreement → one-command bench → update this PR.

## 5. Path note (Labs)

Target silicon for encoder OV is Core Ultra (CPU/iGPU) and Xeon; this box benches would be **box-only / non-target**. M4 Pro Labs baseline (cited for future PR): 100 slices — 50.43 s image-to-embedding, 55.09 s image-to-mask, ~4.4 GB.
