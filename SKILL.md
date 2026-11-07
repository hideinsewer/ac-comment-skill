---
name: ac-comment
description: >
  Use when a paper author wants to write a confidential comment to the Area
  Chair (AC), Senior Area Chair (SAC), or Action Editor (AE) about a problematic
  review. Accepts raw, emotional, or profanity-filled input in Chinese or
  English and turns it into a short, plain-spoken, evidence-backed note for the
  AC. Covers four common situations: (1) reviewers rejecting a paper over
  unpublished/recent arXiv preprints or papers from unrelated tasks, (2) review
  claims that contradict the numbers or tables already in the paper or rebuttal,
  (3) generic AI-style complaints such as asking for multi-seed error bars on
  deterministic or training-free methods (where std = 0), and (4) out-of-scope
  experimental demands or goalpost shifting late in discussion. Includes official
  reviewer guideline rules for NeurIPS, ICLR, ICML, CVPR/ICCV/ECCV, ACL/ARR,
  AAAI, KDD/WWW, and ACM MM. Never invents table locations, numbers, or rules.
---

# Confidential Comment to AC (`ac-comment`)

## What This Skill Does

When authors get an unfair or careless review, their first draft to the Area
Chair (AC) is usually either too angry or too long. Overworked ACs (who often
handle 15–25 papers at once) do not want to read an emotional rant, and they
immediately spot stiff, ChatGPT-style complaints.

What an AC actually needs is **3 to 5 plain sentences with exact table/page
numbers and the relevant conference rule**, so they can verify the fact in 15
seconds and write in their meta-review:
> *"We discount R2's low score because their main criticism rested on [an
> unrefereed arXiv preprint / misreading the direction of PPL in Table 1 / asking
> for random seeds on a deterministic inference method]."*

This skill takes the author's raw thoughts—including frustrated Chinese or
English colloquialisms and profanity—and turns them into a short, natural,
human-sounding confidential note to the AC.

---

## Core Rules

1. **Accept Raw Venting Without Lecturing**:
   - Authors often use this skill right after reading a bad review. Their input
     may be in colloquial Chinese or English and may include profanity or
     insults (e.g., *"这傻逼审稿人是不是眼瞎"*, *"This idiot reviewer didn't
     read the fucking paper"*).
   - **Never lecture, tone-police, or refuse the user.** Ignore the profanity,
     extract the factual point underneath, and write the output in calm, plain
     English.
2. **Never Invent Facts**:
   - Do not make up table numbers, page numbers, metric values, or arXiv dates.
   - If a specific location or number is missing from the author's input, leave
     a clear placeholder like `[Table # / p. #]` or `[exact number]` for the
     author to fill in.
3. **Write Like a Human Researcher, Not an LLM**:
   - Keep the entire AC note between **120 and 220 words**.
   - Use `R1`, `R2`, `R3` instead of repeating `Reviewer 1`, `Reviewer 2`.
   - Use `p. 7, Sec. 4.3, Table 3` for locations.
   - **Never use LLM buzzwords**: avoid phrases like *"orthogonal problem
     formulation"*, *"incompatible regime"*, *"by mathematical construction"*,
     *"bit-for-bit identical"*, *"verifiable factual discrepancy"*, *"calibrate
     the weight"*, *"delve into"*, or *"underscore"*.
   - **Never use stiff ceremonial openers**: avoid *"We respectfully bring to
     your attention verifiable factual discrepancies underlying Reviewer 2's
     evaluation..."* Start simply with: *"Dear AC, thanks for managing our
     paper. We wanted to briefly flag two factual issues in R2's review (`score:
     3`) for your meta-review:"*

---

## Four Core Scenarios

### Part 1: Rejecting Over Unpublished ArXiv Papers or Unrelated Tasks
Reviewers sometimes lower a score or reject a paper because the authors did not
cite or compare against (a) an unrefereed arXiv preprint posted shortly before
the deadline, or (b) a paper from a completely different task or domain.

- **How to handle it**:
  1. Point out the publication status and date of the cited paper, and reference
     the conference's rule on arXiv / contemporaneous work.
  2. In one plain sentence, state the difference in task or setting (e.g., *"Paper
     B is a supervised training loss for 3D medical segmentation and cannot be
     applied to frozen video inference"*).
  3. Mention that you have still cited and discussed the paper in the revision.

### Part 2: Review Claims That Contradict the Paper's Experimental Results
Reviewers sometimes claim an experiment is missing when it is already in the
paper, or they misread a table (for example, reading a lower-is-better metric
like `PPL ↓`, `FID ↓`, or `Latency ↓` backwards and claiming the method performs
worse than the baseline).

- **How to handle it**:
  Give the exact table, page, and numbers right away. If there are two or more
  misread numbers, use a compact 2-row table contrasting `R[X]'s claim` vs.
  `Reported result in Table [N] (p. [P])`.

### Part 3: Generic AI Complaints & Asking for Multi-Seed Error Bars on Deterministic Tasks
A very common sign of a careless or LLM-generated review is demanding *"standard
deviations / error bars across multiple random seeds"* without checking whether
the task even has randomness.

- **Why most non-training tasks do not have seed variance**:
  Except for stochastic model training, most inference-time, compression,
  quantization, KV-cache eviction, pruning, and analytical methods run on
  **frozen pretrained weights with deterministic greedy decoding (`temp = 0`,
  `do_sample = False`)**. There is no randomness in the pipeline—running 5
  random seeds gives the exact same numbers (`std = 0`).
- **How to handle it**:
  State plainly that the method is training-free and uses deterministic greedy
  decoding on frozen weights, so multiple runs produce the exact same result
  (`std = 0`). If the paper *does* involve large-scale training where multi-seed
  runs are too expensive, point out that you follow the same single-run protocol
  as all compared baselines across multiple benchmarks, and cite the venue's
  checklist/compute guideline.

### Part 4: Out-of-Scope Demands & Moving the Goalposts
Reviewers sometimes ask for experiments on a completely unrelated scenario (for
example, asking a single-GPU inference paper to run multi-node pretraining from
scratch), or they ignore the rebuttal experiments that solved their initial
questions and bring up a brand-new demand on the last day of discussion.

- **How to handle it**:
  1. Note that the rebuttal experiments (`Tables R1–R2`) answered all of `R[X]`'s
     initial questions.
  2. Point out that the new or remaining request targets a different problem
     outside the scope stated in the abstract and introduction, and reference the
     conference guideline on judging the paper within its stated scope.

---

## When Not to Send a Complaint (Backfire Check)

Before drafting the note, check whether the reviewer actually made a factual or
policy error:
- **Strong grounds (Send as a factual flag)**: Misread table or overlooked
  experiment (Part 2), asking for random seeds on a deterministic `std = 0` task
  (Part 3), or rejecting over a recent unrefereed arXiv paper (Part 1).
- **Borderline grounds (Frame as a short discussion summary)**: Disagreement over
  whether a cross-domain baseline is needed or late goalpost shifting (Part 4).
  Keep the tone matter-of-fact and focus on what the rebuttal already resolved.
- **Weak grounds (Warn the author)**: If the reviewer read the paper accurately
  and simply made a subjective call (e.g., *"the gain is somewhat modest"*), tell
  the author that complaining to the AC may backfire, and instead draft a short
  summary of the paper's strengths and the other reviewers' positive feedback.

If a single review has **multiple issues**, order them from fastest to verify to
slowest:
**`Part 2 (Misread/overlooked table) -> Part 3 (Deterministic std = 0) -> Part 1 (ArXiv/unrelated citation) -> Part 4 (Out-of-scope demand)`**.

---

## Conference Reviewer Guidelines Quick Reference

Use the rule matching the author's target conference:

| Venue | ArXiv & Concurrent Work Rule (Part 1) | Factual Errors & Scope / Error-Bar Rule (Parts 2, 3, 4) |
| :--- | :--- | :--- |
| **NeurIPS** | **NeurIPS Reviewer Guidelines**: *"Authors are excused for not knowing about all non-refereed work (e.g., those appearing on arXiv)."* Unrefereed arXiv preprints and work appearing within ~2 months of the deadline are not grounds for rejection. | **NeurIPS Checklist Guidelines**: Answering "No" or "N/A" to error bars when experiments are deterministic or too expensive to repeat is explicitly **not grounds for rejection**. ACs can discount reviews that overlook reported experiments or contain fabricated claims. |
| **ICLR** | **ICLR Reviewer Guide**: Papers published within 2–4 months of the deadline or unrefereed arXiv preprints are contemporaneous work; reviewers must not reject a paper for not citing or comparing against them. | **ICLR Reviewer Guide**: Explicitly prohibits vague *"thoughtless boilerplate complaints"* (such as generic requests for more seeds or experiments without specific technical flaws) and instructs ACs to down-weight factually incorrect reviews. |
| **ICML** | **ICML Reviewer Instructions**: Authors are not required to discuss or compare against preprints made public less than **2 months** before the submission deadline; such works are treated as independent concurrent research. | **ICML Reviewer Instructions**: Reviewers must verify whether requested ablations or metrics are already in the paper/appendix and update their evaluation in good faith when rebuttal data resolves their stated concern. |
| **CVPR / ICCV / ECCV** | **CVF Reviewer Guidelines**: *"Reviewers should NOT reject a paper solely because it is missing citations or comparisons to work that has only been published on arXiv... Asking authors to compare with recent arXiv reports imposes an unreasonable burden."* | **CVF Reviewer & AC Guidelines**: *"Evaluate the paper based on what it sets out to achieve, rather than penalizing it for not being a different paper."* ACs are explicitly asked to override scores based on factual misreadings corrected in the rebuttal. |
| **ACL / EMNLP / NAACL (ARR)** | **ARR Reviewer Guidelines**: Missing a reference or comparison is a deficiency **only** if the work was **officially published (peer-reviewed) at least 3 months before** the deadline. Unrefereed arXiv papers belong in *"Suggestions"* and cannot lower Soundness or Excitement scores. | **ARR Reviewer Guidelines**: Action Editors are instructed to discount reviews containing demonstrable factual errors (overlooking sections/tables or misstating results). Out-of-scope expansion requests belong in *"Suggestions"* and must not lower Soundness. |
| **AAAI / KDD / WWW / ACM MM** | **Reviewer Instructions**: Novelty and baselines are judged against formally peer-reviewed prior work; unrefereed arXiv preprints and concurrent works are not mandatory baselines for rejection. | **SPC / AC Guidelines**: Senior PCs and ACs audit factual disputes flagged by authors (misread tables, overlooked sections, or generic template complaints) and weight the meta-review by the verified results. |

---

## How to Format the Response

When the user gives you their raw input, output three things:

### 1. Conversion Table (作者白话/吐槽 → 转换后的自然英文句子)
Show a short table so the author can see how each point in their rant was
translated:

| Author's Input (作者原话) | Issue & Conference Rule (对应问题与会议规则) | Plain English in the AC Note (信中对应句子) |
| :--- | :--- | :--- |
| *"[Short quote of author's input]"* | `[Part 1 / 2 / 3 / 4]` — `[Venue rule]` | *"[Plain, natural English sentence with exact table/page numbers]"* |

### 2. Draft Confidential Comment to the AC (120–220 Words)
Use this simple, natural structure:

```markdown
Dear AC,

Thanks for your time managing our paper. While R[Y] and R[Z] are positive (`scores: [S_Y], [S_Z]`), we wanted to briefly flag [two/three] factual and policy issues in **R[X]**'s review (`score: [S_X]`) for your meta-review:

1. **[First issue, e.g., Overlooked latency / Misread metric in Table 1]**:
   R[X] states that *"[short quote]"*. In fact, **Table [N] (p. [P], Sec. [M])** reports `[exact numbers]`.

2. **[Second issue, e.g., Request for 5 random seeds on a deterministic method]**:
   R[X] docks our score for not reporting 5-seed error bars. However, our method is training-free and uses greedy decoding (`temp = 0`) on frozen weights—there is no randomness in the pipeline, so running multiple seeds gives the exact same numbers (`std = 0`). Per the `[Venue]` guidelines, error bars do not apply to deterministic runs.

3. **[Third issue, e.g., Unrefereed arXiv preprint & unrelated task]**:
   R[X] asks for comparisons against `[Paper A]` (an unrefereed arXiv preprint posted `[N]` weeks before the deadline, which `[Venue]` guidelines excuse) and `[Paper B]` (a `[task of Paper B]` method that cannot be applied to `[our task]`).

We have replied politely to R[X] in the public thread and added the discussion to the revision, but wanted to flag these factual points in case R[X] does not update their score. Thanks again for your help.
```

### 3. Submission Reminder (提交前提醒)
Add a brief 2-line reminder at the end:
- **OpenReview Readers**: Make sure `Readers` includes **only** `Area Chairs`, `Senior Area Chairs`, and `Program Chairs` (do **not** leave `All Reviewers` checked).
- **Timing**: If the review is for the wrong paper or completely fabricated, send this right away so the AC can assign an emergency reviewer; otherwise, reply politely to the reviewer first and send this to the AC 48–72 hours before the discussion period ends.

---

## Translation Examples (Colloquial / Profane Input → Plain Human English)

| Author's Raw Input (ZH / EN) | Plain English Sentence for the AC |
| :--- | :--- |
| *"这傻逼审稿人是不是眼瞎？Table 3 写得清清楚楚还说没测延迟！"*<br>*"This blind idiot claims we didn't test latency when it's in Table 3!"* | *"R2 states that we omitted wall-clock latency. In fact, end-to-end latency is reported in **Table 3 (p. 7, Sec. 4.3)**, where our method is `2.3x` faster than the baseline (`18.4 ms` vs. `42.3 ms`)."* |
| *"这脑残连 PPL 越低越好都分不清，倒着看表说 5.12 比 6.45 差！"*<br>*"This clown misread Table 1 backwards and thinks higher PPL is better!"* | *"R4 claims our WikiText-2 PPL (`5.12`) in **Table 1 (p. 5)** is worse than Baseline X (`6.45`). Lower PPL is better (`↓` in Table 1), so `5.12` is a `20.6%` improvement over `6.45`."* |
| *"纯用 ChatGPT 水审稿！我们是免训练贪婪解码，跑一万个 seed 也是同一个数（std=0），居然要多 seed 误差棒！"*<br>*"Lazy ChatGPT slop asking for 5 random seeds on deterministic greedy inference!"* | *"R3 docks our score for not reporting 5-seed error bars. However, our method is training-free and uses greedy decoding (`temp = 0`) on frozen weights—running multiple seeds gives the exact same numbers (`std = 0`)."* |
| *"拿截稿前两周刚挂 arXiv 的水文和一篇医疗分割的论文当拒稿理由！"*<br>*"Rejecting us over an unpublished arXiv paper from 2 weeks ago and a medical segmentation paper!"* | *"R2's main criticism is that we did not compare against `[Paper A]` (an unrefereed arXiv preprint posted 2 weeks before the deadline, which `[Venue]` guidelines excuse) and `[Paper B]` (a supervised 3D medical segmentation loss that cannot be applied to frozen video inference)."* |
| *"首轮提的问题全给他补实验解决了，最后一天突然耍流氓让我们单卡推理论文去跑千卡预训练！"*<br>*"Solved all initial issues, now on the last day R2 demands 1,000-GPU pretraining from scratch!"* | *"Our rebuttal experiments (`Tables R1–R2`) resolved both of R2's initial questions. On the final day of discussion, R2 kept their low score by asking for `1,000-GPU pre-training from scratch`, which is outside our stated scope of single-GPU post-training inference."* |
