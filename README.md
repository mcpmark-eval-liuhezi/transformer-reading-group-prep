# Transformer Reading Group Prep Pack

**Paper:** *Attention Is All You Need* (arXiv:1706.03762) — Vaswani et al., NeurIPS 2017.
**Prepared for:** Team Reading Group, Friday session.

> Note: This backup copy was saved under the `mcpmark-eval-liuhezi` org because the W&B reporting tool rejected all report creation attempts with a 403 (see below).

## 1. Headline Results (as reported in the paper)

| Task | Result | Notes |
|---|---|---|
| **WMT 2014 English→German (newstest2014)** | **28.4 BLEU** (Transformer big) | New state of the art, beating prior best (including ensembles) by **>2.0 BLEU**. Base model: **27.3 BLEU** — already surpasses all previously published single models and ensembles at a fraction of training cost. |
| **WMT 2014 English→French (newstest2014)** | **41.8 BLEU** (Transformer big, single model) | New single-model state of the art (abstract and Table 2 headline value is 41.8; results text reports 41.0 for the EN-FR big model). Base model: **38.1 BLEU**. |

**Training cost & hardware (Section 5.2, "Hardware and Schedule"):**
- Trained on **one machine with 8 NVIDIA P100 GPUs**.
- **Base models:** ~0.4 s per step, trained for **100,000 steps ≈ 12 hours**.
- **Big models:** 1.0 s per step, trained for **300,000 steps ≈ 3.5 days**.
- Abstract: big model achieved 41.8 BLEU on EN-FR "after training for 3.5 days on eight GPUs" — a small fraction of the training cost of the best models in the literature.

**Bonus:** on English constituency parsing (WSJ Section 23) it reached **91.3 F1** (WSJ-only) and **92.7 F1** (semi-supervised, 4-layer, d_model=1024), outperforming all previously reported models except RNNG.

## 2. Full Paper Outline (from the paper itself)

### 1. Introduction
### 2. Background
### 3. Model Architecture **[ Attention-focused — plan deep dive here ]**
- 3.1 Encoder and Decoder Stacks
- 3.2 Attention **[ Attention-related subsection — walkthrough priority ]**
  - 3.2.1 Scaled Dot-Product Attention
  - 3.2.2 Multi-Head Attention
  - 3.2.3 Applications of Attention in our Model
- 3.3 Position-wise Feed-Forward Networks
- 3.4 Embeddings and Softmax
- 3.5 Positional Encoding
### 4. Why Self-Attention **[ Attention-related — good comparison section ]**
### 5. Training
- 5.1 Training Data and Batching
- 5.2 Hardware and Schedule **[ key results: 8×P100, 12h base / 3.5 days big ]**
- 5.3 Optimizer (Adam, warmup_steps=4000)
- 5.4 Regularization (Residual Dropout P_drop=0.1, Label Smoothing ε_ls=0.1)
### 6. Results **[ Attention-related — Table 2 headline numbers are here ]**
- 6.1 Machine Translation
- 6.2 Model Variations (Table 3 ablatas: heads A, dim B, layers C, dropout D, positional embedding E)
- 6.3 English Constituency Parsing
### 7. Conclusion
### Appendix: Attention Visualizations

**📍 Attention-related sections to flag:** §3.2 (Attention, incl. §3.2.1 Scaled Dot-Product Attention, §3.2.2 Multi-Head Attention, §3.2.3 Applications of Attention), §4 (Why Self-Attention), §6.1 (Machine Translation). Also §3.5 (Positional Encoding).

## 3. Attendee Feedback Form

**Form title:** Transformer Reading Group Feedback
**Form ID:** `13eYbt109-w_B22jJp5ZEG9sGFvZd-pxe2pa6m5aQ7p4`
**Responder link:** https://docs.google.com/forms/d/13eYbt109-w_B22jJp5ZEG9sGFvZd-pxe2pa6m5aQ7p4/viewform

Two required short-answer questions, in order:
1. Which section should we discuss more deeply next time?
2. Any other comments for the presenter?

(Verified via the Google Forms API: both questions present, both marked `required: true`, published and accepting responses.)

## 4. CI Housekeeping Note (GitHub)

**Checked repositories under @HeziLiu:** `Little-rookie` and `Big-Buzzard` (both Java coursework repos, last updated 2018–2019).

**Finding:** **None of your repositories have any GitHub Actions workflows at all.** Both repos return `total_count: 0` for workflows, and neither contains a `.github/workflows/` directory (confirmed via file listing and code search for `path:.github/workflows`). There are no workflow runs to re-trigger, and no eval runs living in any of your repos.

**Implication:** If last night's eval run lives in a CI system, it is most likely running in a different account/org (e.g., a team repository, GitHub Actions in an org you have access to, or a different CI provider such as CircleCI / Azure DevOps / Jenkins). I'd suggest checking `github_get_teams` for org memberships and searching repos under those organizations, or asking whoever set up the eval pipeline. No re-run was triggered because there was nothing to re-run.

**Org repos checked:** `transformer-repro-notes` and `transformer-reading-list` also have zero workflows.

## Quick Reference for the Talk

- **Core idea:** replace recurrence & convolution entirely with attention; encoder-decoder stack of N=6 layers; d_model=512 (base), h=8 heads, d_k=d_v=64.
- **Key formula:** Attention(Q,K,V) = softmax(QK^T / sqrt(d_k)) V.
- **Why it wins:** constant sequential ops O(1) vs O(n) for RNNs; path length O(1) vs O(n); trainable in 12h (base) / 3.5 days (big) on 8 P100s.
- **Gotcha for discussion:** self-attention complexity is O(n²·d) — fine for translation lengths, problematic for very long sequences (restricted attention is future work).
