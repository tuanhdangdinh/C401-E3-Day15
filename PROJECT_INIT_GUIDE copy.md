# Project Initialization Guide
## LexAI — Legal Contract Review Multi-Agent System
### Enterprise Readiness Review | Phase 1 Final Day

---

## 0. Project Identity Card

| Field | Detail |
|---|---|
| **Project Name** | LexAI — AI-Powered Legal Contract Intelligence |
| **Topic Type** | Phase 1 Product (Scenario B — Legal/Enterprise) |
| **Agent Architecture** | Multi-Agent Level 3 |
| **Target Users** | Junior lawyers, legal associates, in-house legal teams |
| **Core Problem** | 3–8 hours per contract on first-pass review; hallucination risk; $166M/yr wasted on repetitive legal labor |

---

## 1. Worksheet 0 — Learning Timeline & Project Lock-In

**What the agent is:**
A 4-agent pipeline that automates the full first-pass review lifecycle of a legal contract.

**Agent roster:**

| Agent | Role | Model Tier |
|---|---|---|
| Extraction Agent | Parse PDF/DOCX, extract dates, amounts, parties | Claude Haiku (cheap) |
| Risk Scoring Agent | Compare clauses vs. company Playbook, flag violations | Claude Sonnet |
| Redline Drafter Agent | Rewrite risky clauses in counter-language | Claude Opus (expensive, critical) |
| Citation Verifier Agent | Cross-check all legal citations via Westlaw/LexisNexis | Claude Haiku + external API |

**Why this topic is production-worthy:**
- Clear user (junior lawyer), clear input (PDF contract), clear output (risk report + redlined doc)
- Quantifiable pain: 3–8h saved per contract, $166M/yr labor target
- Real hallucination stakes = forces honest discussion of HITL and reliability

---

## 2. Worksheet 1 — Enterprise Deployment Clinic

### Data Sensitivity Assessment

| Data Type | Sensitivity | Can Leave Org? |
|---|---|---|
| Contract text (client deals) | **Very High** | No |
| Company Playbook rules | High | No |
| Legal citations (case law) | Low-Medium | Yes |
| Redline outputs | High | Controlled |

### 3 Biggest Enterprise Constraints

1. **Data sovereignty** — Contract content contains trade secrets, M&A details, loan terms. Cannot be sent to public cloud LLM endpoints without legal risk.
2. **Hallucination liability** — A fabricated clause or fake case citation = lawyer disciplinary action + multi-million dollar malpractice. Every AI output needs verifiable sourcing.
3. **Human-in-the-loop mandate** — For contracts >$500K value, no AI output can be final without a licensed attorney's approval signature.

### Deployment Decision: **Hybrid**

| Layer | Where | Why |
|---|---|---|
| Document ingestion & chunking | On-premise | Contracts never leave internal network |
| Extraction & Risk Scoring (Haiku/Sonnet) | Private cloud (AWS GovCloud / Azure Confidential) | BAA agreement, isolated VPC |
| Redline Drafting (Opus) | Private cloud, encrypted session | High-value reasoning, needs full context |
| Citation verification | Public cloud + Westlaw API | Case law is public record |
| HITL review UI | On-premise | Audit trail stays internal |

**Two reasons for Hybrid over pure Cloud:**
1. Legal privilege — attorney-client privileged documents cannot transit public endpoints without waiving privilege
2. Audit trail — regulators (Bar associations, SEC for M&A) require immutable on-site logs

---

## 3. Worksheet 2 — Cost Anatomy Lab

### Traffic Assumptions (MVP)

| Metric | Estimate |
|---|---|
| Users/day | 50 lawyers |
| Contracts reviewed/day | 30 contracts |
| Avg contract length | 80 pages ≈ 120K tokens |
| Peak (M&A season) | 3x normal = 90 contracts/day |

### Cost Breakdown Per Contract

| Layer | Cost Driver | Est. Cost/Contract |
|---|---|---|
| Extraction (Haiku, 120K tokens in) | Token cost | ~$0.04 |
| Risk Scoring (Sonnet, 120K in + 2K out) | Token cost | ~$0.43 |
| Redline Drafting (Opus, 40K in + 8K out) | Token cost | ~$2.10 |
| Citation Verification (Haiku + Westlaw API) | Token + API call | ~$0.80 |
| Storage (contract + audit log) | S3/blob, 3 years retention | ~$0.01 |
| HITL review platform (amortized) | Engineering cost | ~$0.15 |
| **Total per contract** | | **~$3.53** |

### MVP Monthly Cost (30 contracts/day × 22 workdays)

| Tier | Contracts/Month | LLM Cost | Infra | Total |
|---|---|---|---|---|
| **MVP** | 660 | ~$2,330 | ~$1,200 | **~$3,530/mo** |
| **Growth (5x)** | 3,300 | ~$11,650 | ~$3,500 | **~$15,150/mo** |
| **Scale (10x)** | 6,600 | ~$18,000* | ~$6,000 | **~$24,000/mo** |

*10x benefits from volume discounts and model routing savings.

**Hidden costs most teams forget:**
- Westlaw/Lexis API access: ~$2,000/mo base fee
- Human reviewer salary (HITL): 1 senior lawyer × 20% time = ~$8,000/mo
- Prompt engineering maintenance when model versions update
- Security audit (SOC2 compliance): $15–30K one-time + $5K/yr

---

## 4. Worksheet 3 — Cost Optimization Debate

### 3 Chosen Strategies

**Strategy 1: Model Routing (Do immediately)**
- Route by task complexity: Extraction → Haiku, Risk scoring → Sonnet, Redline → Opus only when risk score > threshold 7/10
- Savings: **65–70% token cost reduction** (confirmed in project spec)
- Trade-off: Slightly longer pipeline, need routing logic
- When: Day 1, before any real traffic

**Strategy 2: Semantic Caching (Do at Growth stage)**
- Cache extraction results for identical/near-identical contract clauses (boilerplate terms appear in 60%+ of contracts)
- Use vector similarity (cosine > 0.95) to serve cached risk scores without re-calling LLM
- Savings: ~30% of Sonnet calls eliminated at scale
- Trade-off: Cache invalidation when Playbook rules update
- When: After 1,000+ contracts processed (enough data to justify cache infra)

**Strategy 3: Hierarchical Chunking + Selective Inference (Do immediately)**
- Chunk contracts by article/section structure, not fixed token windows
- Skip sections flagged as "standard boilerplate" after first 100 contracts (learn what is safe to fast-path)
- Savings: Reduces avg input tokens by ~40% per contract
- Trade-off: Risk of missing hidden non-standard clause in boilerplate section — mitigated by random audit sampling (5%)
- When: From MVP launch

---

## 5. Worksheet 4 — Scaling & Reliability Tabletop

### 3 Failure Scenarios

| Scenario | User Impact | Short-term Response | Long-term Solution |
|---|---|---|---|
| **Traffic spike** (M&A season, 10x load) | Queue builds, lawyers wait >5min | Return async job ID, notify via email when done | Job queue (SQS/Redis), autoscaling Opus inference pods |
| **Claude API timeout / provider outage** | Review pipeline stalls mid-contract | Retry 3x with exponential backoff; fallback to Sonnet for Redline if Opus unavailable | Multi-provider fallback (Anthropic → Azure OpenAI o3), circuit breaker pattern |
| **Hallucinated citation passes through** | Lawyer submits brief with fake case law → disciplinary action | Citation Verifier blocks output until Westlaw confirms all citations | Hard gate: pipeline cannot emit Redline without 100% citation verification pass |

### Monitoring Metrics

- `p99 latency per contract review` — SLA: <3 min for <50 page contract
- `hallucination_block_rate` — Citation Verifier rejections per day (alert if >5%)
- `HITL_queue_depth` — Contracts awaiting human approval (alert if >10)
- `LLM_cost_per_contract` — Alert if exceeds $6 (2x baseline = model route failure)
- `cache_hit_rate` — Should be >25% at Growth stage

### Fallback Proposal

```
If Opus unavailable:
  → Sonnet for Redline + mandatory HITL for all contracts (not just >$500K)

If Westlaw API down:
  → Block citation output, flag contract as "Pending Citation Verification"
  → Lawyer notified, can choose to proceed at own risk (logged)

If full LLM outage >15min:
  → Route to async queue, promise SLA of 4 hours
  → Do NOT degrade to rule-based fallback for legal risk scoring (too high stakes)
```

---

## 6. Worksheet 5 — Skills Map & Phase 2 Track

### Team Self-Assessment Grid

| Track | Skills Required | LexAI Fit |
|---|---|---|
| **AI Engineering** | Multi-agent orchestration, RAG, prompt engineering, eval pipelines | **Primary fit** — 4-agent pipeline needs deep LLM engineering |
| **Infra/Data/Ops** | Vector DBs, deployment, monitoring, FinOps | **Secondary fit** — Hybrid deployment complexity |
| **Business/Product** | ROI modeling, enterprise sales, compliance | Strong supporting role |

**Recommended Phase 2 Track: AI Engineering** with Infra support role.

**Skills gap to fill:**
1. LangGraph or custom orchestration framework for multi-agent state management
2. Legal domain RAG — chunking law documents differs from general text
3. Evaluation framework for hallucination detection (not just citation checking)

---

## 7. Afternoon Sprint — 7-Block Proposal (Slide Structure)

### Slide 1: Project Overview
- **Name:** LexAI — Legal Contract Intelligence
- **User:** Junior legal associates at mid-size law firms (50–200 lawyers)
- **Problem:** $166M/yr industry-wide wasted on first-pass contract review; 3–8h per contract
- **Solution:** 4-agent pipeline that reviews, scores risk, drafts redlines, and verifies citations in <3 minutes
- **Hook for investors:** We are the only solution with a Citation Verifier that provides a hard legal audit trail — eliminating the #1 liability of legal AI

### Slide 2: Enterprise Context & Constraints
- Data: Attorney-client privileged contracts, M&A documents, loan agreements
- Key constraints: Data sovereignty, hallucination liability, mandatory HITL >$500K
- Why this is hard: Not a chatbot — it needs to be **right**, not just helpful

### Slide 3: Architecture — Hybrid Deployment

```
[Law Firm On-Prem]          [Private Cloud (AWS GovCloud)]
   PDF Intake          →     Extraction Agent (Haiku)
   Playbook Store      →     Risk Scoring Agent (Sonnet)
   HITL Review UI      ←     Redline Drafter (Opus) [only if risk > 7]
   Audit Log DB        ←     Citation Verifier (Haiku + Westlaw API)
```

### Slide 4: Cost Anatomy
- MVP: **$3,530/month** (660 contracts, 50 users)
- Growth (5x): **$15,150/month**
- Revenue model: $150/contract reviewed → MVP breaks even at 24 contracts/month
- **ROI to law firm: 3–6x** (replace 2–3h junior lawyer time per contract @ $300/h billing rate)

### Slide 5: Cost Optimization Plan

| Strategy | Savings | When |
|---|---|---|
| Model Routing | 65–70% | Day 1 |
| Hierarchical Chunking | 40% token reduction | Day 1 |
| Semantic Caching | 30% Sonnet calls | After 1K contracts |

### Slide 6: Reliability & Scaling
- Async queue for traffic spikes (M&A season)
- Hard citation gate — no output without Westlaw confirmation
- HITL escalation path for all contracts >$500K
- Multi-provider fallback: Anthropic → Azure OpenAI

### Slide 7: Track Recommendation & Next Steps
- **Phase 2 Track: AI Engineering**
- Next 3 milestones:
  1. Build evaluation harness for hallucination detection (Week 1–2)
  2. Integrate Westlaw API sandbox (Week 3–4)
  3. Deploy MVP to 1 pilot law firm with 5 real contracts (Month 2)
- **Ask (for investors):** $250K seed to build production MVP + first enterprise pilot

---

## 8. Scoring Checklist (Pre-Presentation Audit)

Before presenting, verify you can answer these with confidence:

- [ ] **Enterprise context (15pts):** Can you name the 3 biggest constraints and why each is non-negotiable?
- [ ] **Architecture (20pts):** Can you defend Hybrid over pure Cloud with 2 specific legal reasons?
- [ ] **Cost analysis (20pts):** Can you show cost breakdown by component at both MVP and Growth? Do you include HITL labor?
- [ ] **Optimization (15pts):** Can you explain the trade-off of semantic caching (cache invalidation when Playbook updates)?
- [ ] **Reliability (15pts):** What happens when Westlaw API goes down? What happens during M&A season peak?
- [ ] **Phase 2 track (10pts):** Why AI Engineering and not Infra? What specific skills does the team need to acquire?
- [ ] **Presentation (5pts):** Under 5 minutes, 1 concrete question you expect and how you'd answer it

---

## 9. Investor Pitch Frame (for Business Plan)

> "We are not building a legal chatbot. We are building the **compliance infrastructure** that every law firm will be legally required to have an audit trail for, as AI regulation tightens. Our Citation Verifier is the moat — no competitor has a hard-gate hallucination block with external legal source verification. First-mover into 5 mid-size firms gives us the training data to make that moat wider every quarter."

**Key investor metrics to have ready:**

| Metric | Value |
|---|---|
| TAM | $23B global legal tech market, $8B contract review segment |
| Payback period for law firm | 4–8 months |
| Net ROI to customer | 3–6x in Year 1 |
| Unit economics | $3.53 cost → $150 charge → **42x gross margin per contract** |
| Time savings | 2–3 hours per lawyer per week |
| Labor cost replaced | Up to $166M/yr at scale |
