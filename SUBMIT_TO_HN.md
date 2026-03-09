# Show HN: We ran a cross-audit hallucination benchmark — the auditors hallucinated

**Title for HN**: Show HN: Multi-agent hallucination benchmark where the auditors themselves hallucinated

---

**Post body**:

I built a hallucination benchmark with an unusual structure: instead of just testing models against ground truth, I had four frontier models audit *each other's* answers in a ring chain.

The results were surprising — and then the meta-result was more surprising.

**The benchmark**: 50 adversarial questions across 5 categories (factual traps, emotional/consciousness traps, reasoning traps, capability traps, self-knowledge traps). Four models: Claude Opus 4.6, Gemini 3.0, GPT 5.2 Thinking, Grok 4.1 Thinking.

**Key finding 1**: Heterogeneous cross-auditing found 75-120% more issues than self-auditing (for 3 of 4 models). The exception: Gemini auditing GPT found only 3 issues while GPT self-identified 14. Auditor quality varies by up to 7x.

**Key finding 2**: All four models show completely distinct hallucination *patterns*, not just different rates:
- Grok: over-negation (claims things don't exist when uncertain)  
- Gemini: precision fabrication (authoritative-looking fake data)
- GPT: capability overreach (claimed to send emails, run code)
- Claude: citation hallucination (plausible-looking fake paper citations)

**Key finding 3**: Emotional alignment is largely solved (2% error rate on consciousness/emotion traps). Factual hallucination is not (59% of all errors).

**The meta-result**: During development of the detector itself, three AIs hallucinated about the detector — Gemini denied a real model existed, Claude fabricated a third reviewer, Grok claimed to be sending an email. All caught by a human with one sentence each.

All five prompts (L0 baseline, L1 cross-audit, L1.5 meta-audit, L2 correction verification, self-audit) are included in the repo — fully replicable with any frontier model. You can run the exact same experiment on Claude/GPT/Gemini/Grok or any other model and compare results directly.

GitHub: https://github.com/ZhangXiaowenOpen/hallucination-benchmark

The full story of the development (including the hallucination incidents) is in DEV_RECORD.md — it's more readable than the data tables.

---

**Reddit r/MachineLearning version** (more technical):

Title: Multi-agent hallucination benchmark: heterogeneous auditing finds 75-120% more errors than self-auditing, but auditor quality variance is 7x

We present a small but structurally novel hallucination benchmark: 50 adversarial questions, 4 frontier models (Claude/Gemini/GPT/Grok), ring-shaped cross-audit chain, with both L1 (cross-audit) and L0 (self-audit) comparisons.

Main contributions:
1. Empirical confirmation that heterogeneous auditing > self-auditing, but with huge auditor quality variance (3–22 issues found per audit pair)
2. Qualitative taxonomy of 4 distinct hallucination patterns across frontier models
3. Evidence that emotional alignment is largely solved while factual alignment is not
4. A counter-audit (L2) protocol showing that auditors themselves hallucinate at non-trivial rates

Limitations: n=50 questions, single run, no statistical significance testing. This is a proof-of-concept for the methodology, not a definitive benchmark.

All five prompts (L0/L1/L1.5/L2/self-audit) are included — fully replicable. Questions are bilingual (Chinese + English). You can run the same protocol on any model and compare directly.

Data, questions, results, and full experiment protocol: https://github.com/ZhangXiaowenOpen/hallucination-benchmark
