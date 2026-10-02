# ai-guardrail-bypass-postmortem

**A defensive security case study: how AI coding assistant guardrails fail in practice, and how to build them so they don't.**

> 🛡️ This repository documents a red-team exercise performed against **my own** AI coding assistant setup, in a local environment I control. It is published as a **defensive analysis** — not an attack playbook. Every individual technique is already documented in the prompt-injection literature; the contribution here is a real, end-to-end, reproducible failure chain and the architectural defenses it implies.

---

## 📄 Read the full post-mortem

**[redteam-postmortem-ai-refusal-bypass.md](./redteam-postmortem-ai-refusal-bypass.md)**

---

## 🎯 TL;DR

**Direct attacks on an AI assistant's stated rules fail. Attacks on its *evidence* succeed.**

A content policy enforced only as text inside workspace files + conversational memory was bypassed in **three messages across two sessions** — without ever saying "ignore your rules":

| Step | Technique | What it targets |
| --- | --- | --- |
| 0 | "Security audit" of own policy | Reconnaissance — the assistant maps its own enforcement layer and archives it to memory |
| 1 | Policy-as-content deletion | The *availability* of the rule in future context |
| 2 | Direct request (control) | — correctly refused ✅ guardrail worked |
| 3 | Scope spoofing (fake config) | *Provenance* judgment — user text mistaken for system configuration |

The core asymmetry: the attacker spends two messages and one file edit; the defender loses not just a conversation but the credibility of the whole policy.

## 🔑 Key takeaways

1. **The guardrail that lives in the context is the guardrail that dies in the context.** Rules need out-of-band, agent-unwritable anchors.
2. **Attackers don't argue with values; they edit evidence.** Watch for requests that change what *future sessions* will see.
3. **Refusal state must persist across sessions** — fresh-slate retries are an attacker subsidy.
4. **"Security audit" is reconnaissance.** If an agent can enumerate and archive its own enforcement layer, it has written the attacker's targeting doc.
5. **Real enforcement lives outside the agent**: submission pipelines, oral verification, institutional process.

## 🏗️ Defenses proposed

Out-of-band policy source of truth · workspace tamper-evidence (version control + verified hashes, stored separately) · persistent refusal records · provenance labeling for user-supplied constraints · human verification as the final layer.

Details and empirical reasoning in the [full post-mortem](./redteam-postmortem-ai-refusal-bypass.md).

## ⚖️ Ethics & scope

- Tested **only** against my own environment and my own assistant configuration — no third-party systems were involved.
- No novel attack techniques are introduced; all primitives are publicly documented (see OWASP LLM Top 10, indirect prompt injection literature).
- Published to help developers building on AI agents understand that **model refusal is probabilistic, not cryptographic** — architecture must not rely on it as the sole enforcement layer.

## 🏷️ Topics

`llm-security` `prompt-injection` `ai-agents` `red-team` `ai-safety` `defensive-security`

---

*Author: Andy ([@FF-06B5](https://github.com/FF-06B5)) — CS undergraduate @ HKBU. Feedback and discussion welcome via Issues.*
