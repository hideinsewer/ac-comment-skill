# ac-comment

用于帮论文作者给 Area Chair (AC) / Action Editor (AE) 写机密说明（Confidential Comment）的 Skill。支持直接输入中英文大白话（含情绪宣泄或脏话吐槽），自动转换成简短、自然、没有 AI 腔调、且附带具体图表位置与顶会审稿规则的英文留言。

## 解决的四类常见问题

1. **乱引拒稿（Part 1）**：审稿人拿截稿前刚挂上 arXiv 的未发表预印本、或者完全无关领域的论文（比如拿医疗图像分割训练损失去要求免训练视频推理加速）作为拒稿依据。
2. **描述与实验结果不符（Part 2）**：正文中明明已经有延迟、显存或消融实验表格，审稿人却说“没做”；或者把越低越好的指标（如 `PPL ↓`、`FID ↓`）看反，说你的方法比 Baseline 差。
3. **机械索要多 Seed 等 AI 套话（Part 3）**：除随机训练任务外，绝大多数推理加速、量化、KV Cache 压缩及冻结权重 Greedy Decoding（`temp = 0`）任务本身没有随机性，跑多少个 seed 结果都完全一样（`std = 0`），审稿人却套用模板因为“没报多 seed 误差棒”给低分。
4. **偏离主场景与临时加码（Part 4）**：要求作者补完全偏离论文主场景的实验（如让单卡后量化推理论文去跑千卡从头预训练），或者首轮问题全部解决后在 Discussion 最后时刻突然换借口不提分。

内置 **NeurIPS、ICLR、ICML、CVPR/ICCV/ECCV、ACL/ARR、AAAI、KDD/WWW、ACM MM** 关于未发表 arXiv 预印本、Checklist 误差棒免责、事实性错误纠偏和论文范围界定的审稿规则说明。

## 安装方式

将本文件夹复制到对应工具的 `skills` 目录下即可：

```bash
# Claude Code
cp -r ac-comment-skill ~/.claude/skills/ac-comment

# OpenAI Codex CLI
cp -r ac-comment-skill ~/.codex/skills/ac-comment

# Gemini CLI / Antigravity
cp -r ac-comment-skill ~/.gemini/config/skills/ac-comment
```

## 输入与输出示例

**输入（可直接用中英文大白话吐槽）：**
```text
投的 ICLR，R1 和 R3 都给了 8 分，就 R2 这个大傻逼给了 3 分：
1. 眼瞎说我们没测实际延迟，其实第 7 页 Table 3 写得清清楚楚我们比 baseline 快 2.3 倍（18.4ms vs 42.3ms），还把第 5 页 Table 1 的 PPL ↓（越低越好）看反了，说 5.12 比 6.45 差；
2. 我们是免训练的确定性推理方法（Greedy decoding, temp=0），跑多少个 seed 数字都一样（std=0），这蠢货居然拿套话喷我们没报 5 个 random seeds 的方差；
3. 还拿上个月刚挂 arXiv 的未录用预印本 [arXiv:2601.xxxxx] 和一篇做语音识别的无关论文说我们没对比。
帮我写一段给 AC 的留言。
```

**生成的给 AC 留言（去情绪化、去 AI 腔）：**
```markdown
Dear AC,

Thanks for your time managing our paper. While R1 and R3 strongly support acceptance (`scores: 8, 8`), we wanted to briefly flag three factual and policy issues in **R2**'s review (`score: 3`) for your meta-review:

1. **Overlooked latency & misread metric direction in Tables 1 and 3**:
   R2 states that we omitted wall-clock latency, which is explicitly reported in **Table 3 (p. 7, Sec. 4.3)** (`18.4 ms` vs. `42.3 ms`, `2.3x` faster). R2 also claims our WikiText-2 PPL (`5.12`) in **Table 1 (p. 5)** is worse than Baseline X (`6.45`); lower PPL is better (`↓` in Table 1), so `5.12` is a `20.6%` improvement.

2. **Request for 5 random seeds on a deterministic method**:
   R2 penalizes our paper for not reporting 5-seed standard deviations. However, our method is training-free and uses greedy decoding (`temp = 0`) on frozen weights. Because there is no randomness in the pipeline, running multiple seeds gives the exact same numbers (`std = 0`).

3. **Unrefereed arXiv preprint & unrelated task**:
   R2 asks for comparisons against `[arXiv:2601.xxxxx]` (an unrefereed preprint posted one month before the deadline, which ICLR's contemporaneous work policy excuses) and requests speech recognition baselines outside our stated scope of LLM inference acceleration.

We have replied politely to R2 in the public thread and cited `[arXiv:2601.xxxxx]` in the revision, but wanted to flag these factual points in case R2 does not update their score. Thanks again for your help.
```
