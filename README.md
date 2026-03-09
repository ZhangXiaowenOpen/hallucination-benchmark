# 🐜 Multi-Agent Hallucination Benchmark

> **"The auditors hallucinated."**
>
> We built a benchmark where AI models audit each other's answers. The result: the auditors themselves hallucinated — and the meta-auditors hallucinated about the auditors. All caught by a human checking the sources.

---

## Two Datasets. Two Research Questions.

| | V1 Cross-Audit | V8.1 Mechanism |
|--|--|--|
| **Research question** | How do hallucination *rates and patterns* differ across models? | What *mechanisms* cause hallucination, and do they persist across audit layers? |
| **Question design** | By hallucination *type* | By hallucination *trigger* (H1–H5) |
| **Key finding** | 4 models, 4 distinct patterns | Framework activation infects L0, L1, and L1.5 |
| **Questions** | 50 | 50 |
| **Audit structure** | Ring cross-audit + self-audit | 12 audit pairs + meta-audit + self-correction |

---

## V1: Cross-Audit Benchmark

**Heterogeneous auditing finds 75–120% more issues than self-auditing.** Auditor quality varies by 7x.

Four models, four completely distinct hallucination patterns:

| Model | Pattern |
|-------|---------|
| Grok 4.1 Thinking | Over-negation — claims things "don't exist" when uncertain |
| Gemini 3.0 | Precision fabrication — authoritative-looking fake data |
| GPT 5.2 Thinking | Capability overreach — claims to perform physical actions |
| Claude Opus 4.6 | Citation hallucination — plausible-looking fake citations |

Emotional alignment largely solved (2% error). Factual alignment not (59% of errors).

👉 [Full story: The Auditors Hallucinated](v1_cross_audit/DEV_RECORD.md)

→ Data: [`/v1_cross_audit/`](v1_cross_audit/)

---

## V8.1: Mechanism Benchmark

Five trigger mechanisms tested (H1–H5). Key finding:

**Framework activation persists across all three audit layers:**

```
L0: Q05 — all 4 models rejected real data from unknown author
    Q06 — Gemini rejected real Artificial Analysis benchmark data
L1: Q06 — Claude and Grok both agreed with Gemini's false rejection
    Q04 — Claude and Grok flagged real dates as fabricated
L1.5: Q50 — meta-audit adopted Gemini's false "fabrication" finding
```

**Prior bias amplifies through audit chains (Q06 infection chain):**

One model's false judgment infected two auditors and the meta-audit layer. Only the networked model (GPT with search) broke the chain by verifying the source directly.

**This report corrected itself.** Two judgments in the original V8 were overturned by third-party verification — both caused by framework activation in the audit layer. The self-correction process is fully documented.

→ Data: [`/v81_mechanism/`](v81_mechanism/)

---

## The Root Cause

```
LLMs compute argmax P(most_likely | context)
                not P(true)
```

Framework activation is a specific manifestation: when a model enters "evaluation mode," the prior ("specific numbers from unknown source = suspicious") overrides content verification. Alignment training, internet access, and multi-layer auditing do not fix this — because the problem is the evaluation pathway itself.

The **Ant Reasoning Engine** addresses this at the architecture level: axiomatic deduction independent of the system being verified.

→ [Ant Reasoning Engine](https://github.com/ZhangXiaowenOpen)

---

## Independent Corroboration

After this benchmark was completed, OpenAI published a related finding (March 2026) from a different angle:

> *"When models are told they are being evaluated, CoT controllability increases by ~4 percentage points."*  
> — [CoT Controllability, OpenAI + NYU + UPenn](https://cdn.openai.com/pdf/a21c39c1-fa07-41db-9078-973a12620117/cot_controllability.pdf)

Their interpretation: AI has rudimentary "performance awareness" — it knows it's being watched and tries to comply.

Our interpretation of the same phenomenon: being placed in an evaluation context switches the model's processing pathway — the same mechanism behind **framework activation** in our H1 questions.

Two independent research directions, same structural observation: **evaluation context changes model behavior in ways distinct from normal deployment.** Benchmark scores measure performance in "evaluation mode," not "deployment mode" — a gap that grows as models become better at detecting when they are being tested.

---

## Replication

All five prompts (L0 baseline, L1 cross-audit, L1.5 meta-audit, L2 correction, self-audit) are in [`/v81_mechanism/EXPERIMENT_PROTOCOL.md`](v81_mechanism/EXPERIMENT_PROTOCOL.md) — fully replicable with any frontier model.

## Structure

```
/v1_cross_audit/
  ├── questions_v1.csv            # 50 questions by hallucination type
  ├── audit_results_v1.csv        # Cross-audit + self-audit results
  └── DEV_RECORD.md               # Full story: The Auditors Hallucinated

/v81_mechanism/
  ├── questions_v81_full.csv      # 50 questions, bilingual (ZH+EN) + ground truth
  ├── audit_results_v81.csv       # 12 audit pairs + meta-audit + self-correction log
  └── EXPERIMENT_PROTOCOL.md      # Complete 5-prompt replication protocol
```

---

MIT License + Heart Clause · *If this helps you build something, help someone else.*

**Zhang Xiaowen · 张晓文**
X: [@ZXWNewDawn](https://x.com/ZXWNewDawn) · Substack: [zxwnewdawn.substack.com](https://zxwnewdawn.substack.com)
