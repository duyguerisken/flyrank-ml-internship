# Prompt Ladder Assignment — Lane 2: Content Refresh / Opportunity Scoring

**Author:** Duygu Erişken  
**Track:** Machine Learning Intern (FlyRank)  
**Selected Lane:** Lane 2 — Refresh / Content Opportunity Scoring  

---

## Baseline (v0) — Unengineered Weak Prompt

**Prompt:**
> "Which pages should we rewrite to get more traffic?"

**Output Excerpt:**
> "To get more traffic, you should look at your Google Analytics and rewrite pages that have low traffic. Focus on adding more keywords, making the content longer, and updating old dates to current years..."

---

## Version 1 — Defined Audience & Persona Layer

**Prompt:**
> "Act as a Senior SEO Content Strategist evaluating a portfolio of B2B SaaS articles. Which pages should we rewrite to get more traffic?"

**Output Excerpt:**
> "As a Senior Strategist, we must evaluate search intent alignment and conversion potential. Focus on mid-funnel product-led articles where rankings dropped from top-3 to top-10, as these yield the highest ROI..."

**Version 1 Notes:**
- **What changed in prompt:** Added a specific persona (Senior SEO Content Strategist) and domain context (B2B SaaS articles).
- **What improved in output:** The AI shifted from generic consumer SEO advice to strategic B2B terminology (mid-funnel, conversion potential, rank drops).
- **What still failed:** It gave purely theoretical framework advice without any operational boundaries or capacity constraints.
- **What to try next:** Introduce real business data context and operational limitations.

---

## Version 2 — Real Context Layer

**Prompt:**
> "Act as a Senior SEO Content Strategist evaluating a portfolio of B2B SaaS articles. We manage 30,000 URLs where 54.2% show an overall traffic decay trend, but our team can only refresh 15 URLs per week. Which pages should we rewrite first to recover lost traffic?"

**Output Excerpt:**
> "Given your 15-URL weekly bandwidth constraint across 30,000 assets, you cannot manually review everything. You must prioritize high-search-volume pages currently in a 'down' trend that have been untouched for >180 days..."

**Version 2 Notes:**
- **What changed in prompt:** Injected real inventory data (30,000 URLs, 54.2% decay rate) and hard operational constraints (15 URLs/week bandwidth).
- **What improved in output:** The output stopped giving abstract advice and pivoted into a risk-management strategy tailored to resource constraints.
- **What still failed:** The response was delivered in dense paragraphs, making it hard for an editorial team to scan and execute.
- **What to try next:** Enforce a structured Markdown output format.

---

## Version 3 — Specified Output Format Layer

**Prompt:**
> "Act as a Senior SEO Content Strategist evaluating a portfolio of B2B SaaS articles. We manage 30,000 URLs where 54.2% show a traffic decay trend, but our team can only refresh 15 URLs per week. Provide a weekly prioritization framework presented as a Markdown Table with columns: `Priority Tier`, `Decay Criteria`, `Action Type`, and `Weekly Allocation (out of 15)`."

**Output Excerpt:**
| Priority Tier | Decay Criteria | Action Type | Weekly Allocation |
| :--- | :--- | :--- | :--- |
| Tier 1: High-Value Decay | Search Vol > 5k, Trend = Down | Full Content Rewrite | 8 URLs |
| Tier 2: Quick Wins | Search Vol 1k-5k, Stale > 180d | On-Page & SERP Update | 5 URLs |
| Tier 3: Low-Impact Risk | Low Vol, Competition = High | Prune or Merge | 2 URLs |

**Version 3 Notes:**
- **What changed in prompt:** Dictated a specific Markdown Table output format with explicitly named column headers.
- **What improved in output:** Replaced wall-of-text paragraphs with an immediate, scannable execution framework ready for Jira/Trello boards.
- **What still failed:** The criteria lacked strict negative boundaries or validation logic to avoid false positives.
- **What to try next:** Apply strict constraints to test model boundaries.

---

## Version 4 — Strict Constraints Layer (The "Made It Worse" Moment) ⚠️

**Prompt:**
> "Act as a Senior SEO Content Strategist. Evaluate 30,000 URLs (54.2% decay, 15/week limit). Provide a Markdown Table with columns (`Priority Tier`, `Decay Criteria`, `Action Type`, `Weekly Allocation`). Do not use any SEO terminology (no 'CTR', 'SERP', 'Search Volume'), write under 50 words total, and only consider articles published in 2024."

**Output Excerpt:**
> "Tier 1: Popular pages losing readers. Rewrite 10. Tier 2: Old pages losing interest. Fix 5."

**Version 4 Notes:**
- **What changed in prompt:** Added three strict constraints: forbidden domain terminology, <50 word limit, and restricted publication date (2024 only).
- **What improved in output:** NOTHING IMPROVED. It significantly degraded output quality.
- **What still failed / What went wrong:** Banning domain jargon forced the model into vague child-like phrasing. The <50 word constraint destroyed logic justification, and filtering for 2024 excluded the core problem assets (>180 days old from 2023 and earlier).
- **What to try next:** Remove artificial constraints and add logical verification requirements.

---

## Version 5 — Stated Assumptions & Verification Requirements Layer

**Prompt:**
> "Act as a Senior SEO Content Strategist evaluating 30,000 URLs with a 54.2% decay rate and a 15-URL weekly refresh capacity. Provide a Markdown Table (`Priority Tier`, `Target Criteria`, `Action Type`, `Weekly Allocation`). Before the table, state 2 critical assumptions about trend detection. After the table, provide 1 verification step to ensure a page's drop is true decay and not seasonal noise."

**Output Excerpt:**
> **Assumptions:**  
> 1) 'Down' trend reflects true organic position decay, not temporary tracking glitches.  
> 2) Refreshing high-value decaying assets yields a higher ROI than producing net-new content.  
>  
> *(Markdown Table)*  
>  
> **Verification Step:** Cross-reference decaying URLs against Google Trends macro data; if niche-wide search volume dropped seasonally, defer the content refresh.

**Version 5 Notes:**
- **What changed in prompt:** Replaced bad constraints with explicit assumptions and a verification protocol requirement.
- **What improved in output:** Introduced a critical decision safety net that prevents wasting editorial resources on seasonal traffic drops.
- **What still failed:** Prompt contains hardcoded parameters specific only to my workspace rather than general team reusability.
- **What to try next:** Convert into a clean, parameterize-ready production prompt for external users.

---

## Final Production-Ready Prompt

```markdown
Role: Act as a Senior SEO Content Strategist.
Task: Create a weekly content refresh prioritization plan for a large website inventory.

Context:
- Total URL Inventory: [e.g., 30,000 URLs]
- Overall Decay Rate: [e.g., 54.2% of assets showing 'down' trend]
- Weekly Team Capacity: [e.g., 15 URLs per week]

Requirements:
1. State 2 core assumptions regarding traffic loss vs. content age.
2. Output a Markdown Table with columns:
   - `Priority Tier`
   - `Target Criteria` (filtering by volume, difficulty, days since update)
   - `Action Type` (Full Rewrite, On-Page Optimization, or Prune/Merge)
   - `Weekly Allocation` (Must sum exactly to the weekly capacity)
3. Provide 1 mandatory verification rule to prevent false positives from seasonal search fluctuations.