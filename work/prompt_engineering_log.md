# FL-01 Prompt Engineering Iteration Log — Lane 2: Content Refresh

**Author:** Duygu Erişken  
**Track:** Machine Learning Intern (FlyRank)  
**Selected Task:** Content Refresh & Decay Opportunity Scoring  

---

## 1. Naive Version (v0) — Initial Baseline

**Prompt:**
> "Which pages should we rewrite to get more traffic?"

**Output Excerpt:**
> "To get more traffic, look at Google Analytics and rewrite low-traffic pages. Add relevant keywords, make the content longer, and update old dates to current years..."

**Observed Output Note:**
- **What changed:** Initial benchmark baseline without prompt techniques.
- **Why it failed:** Generates generic, high-level advice with no actionable criteria or prioritization.

---

## 2. Version 1 — Technique: Role Assignment

**Prompt:**
> "You are an Expert B2B Content Strategist specializing in technical SaaS SEO and content decay recovery. Which pages should we rewrite to get more traffic?"

**Output Excerpt:**
> "As a Senior B2B Strategist, simple keyword stuffing won't recover dropped ranks. Focus on realigning search intent for mid-funnel product-led articles where rankings dropped from top-3 to top-10, as these yield the highest ROI..."

**Observed Output Note:**
- **What changed:** Shifted tone from generic advice to strategic B2B domain language by assigning the persona of an Expert B2B Content Strategist.
- **Observed difference:** The model introduced intent alignment and rank drop recovery concepts instead of naive keyword additions.

---

## 3. Version 2 — Technique: Context and Motivation

**Prompt:**
> "You are an Expert B2B Content Strategist specializing in technical SaaS SEO. We manage a portfolio of 30,000 URLs. This specific blog post on B2B SaaS onboarding dropped from Rank 2 to Rank 11 in organic search over the last 6 months, losing 65% of its traffic. Our goal is to recover top-3 rankings with minimal editorial rework. Analyze this blog post and suggest specific refresh actions."

**Output Excerpt:**
> "Since this post already proved it can reach Rank 2, a complete rewrite is unnecessary and risky. Focus strictly on lost intent anchors: identify 2-3 subtopics competitors added, update stale 2023 statistics, and add a summary table at the top to target featured snippets..."

**Observed Output Note:**
- **What changed:** Added specific operational metrics (30k URLs, Rank 2->11 drop, 65% loss) and business motivation (minimal editorial rework).
- **Observed difference:** The model stopped recommending full rewrites and switched to a surgical, low-effort refresh strategy to preserve existing equity.

---

## 4. Version 3 — Technique: Few-Shot Examples

**Prompt:**
> "You are an Expert B2B Content Strategist specializing in technical SaaS SEO. We manage a portfolio of 30,000 URLs. A post dropped from Rank 2 to Rank 11 (65% traffic loss). Suggest prioritized refresh actions.
> 
> Here is an example of the expected output format:
> Input: Rank 3 to 12 drop on 'SaaS Pricing'
> Output:
> | Action | Impact | Effort | Justification |
> | :--- | :--- | :--- | :--- |
> | Add Tier Comparison Table | High | Low | Featured snippet target |"

**Output Excerpt:**
| Action | Impact | Effort | Justification |
| :--- | :--- | :--- | :--- |
| Update Stale 2023 Data Points | High | Low | Restores factual freshness signal |
| Insert H2 for 'Onboarding Workflows' | High | Medium | Closes competitive content gap |
| Add Internal Links to Product Page | Medium | Low | Boosts internal PageRank flow |

**Observed Output Note:**
- **What changed:** Injected a concrete input-output example pair to demonstrate the preferred output schema.
- **Observed difference:** The response immediately adopted the tabular structure, eliminating conversational filler and standardizing impact/effort ratings.

---

## 5. Version 4 — Technique: Output Structure (XML Enclosure)

**Prompt:**
> "You are an Expert B2B Content Strategist.
> 
> <context>
> Portfolio: 30,000 URLs. Target: B2B SaaS onboarding post dropped from Rank 2 to Rank 11 (65% traffic loss). Goal: Top-3 recovery with minimal rework.
> </context>
> 
> <example>
> Input: Rank 3 to 12 drop on 'SaaS Pricing'
> Output:
> | Action | Impact | Effort | Justification |
> | :--- | :--- | :--- | :--- |
> | Add Tier Comparison Table | High | Low | Featured snippet target |
> </example>
> 
> <instructions>
> Enclose your initial diagnosis inside <analysis> tags and your final prioritized action table inside <action_plan> tags.
> </instructions>"

**Output Excerpt:**
> <analysis>
> The page suffers from content staleness and intent drift. Competitors have added visual workflow diagrams and updated metric benchmarks.
> </analysis>
> 
> <action_plan>
> | Action | Impact | Effort | Justification |
> | :--- | :--- | :--- | :--- |
> | Add Visual Workflow Diagram | High | Medium | Matches competitor SERP intent |
> </action_plan>

**Observed Output Note:**
- **What changed:** Used Anthropic-style XML tags to separate context, examples, instructions, and target output blocks.
- **Observed difference:** Completely eliminated structural ambiguity; analysis and action items were strictly compartmentalized for easy programmatic parsing.

---

## 6. Version 5 — Technique: Step Decomposition (Chain-of-Thought)

**Prompt:**
> "You are an Expert B2B Content Strategist specializing in technical SaaS SEO and content decay recovery.
> 
> <context>
> Portfolio: 30,000 URLs. Target: B2B SaaS onboarding post dropped from Rank 2 to Rank 11 (65% traffic loss). Goal: Top-3 recovery with minimal editorial rework.
> </context>
> 
> <example>
> Input: Rank 3 to 12 drop on 'SaaS Pricing'
> Output:
> | Action | Impact | Effort | Justification |
> | :--- | :--- | :--- | :--- |
> | Add Tier Comparison Table | High | Low | Featured snippet target |
> </example>
> 
> <instructions>
> Follow these steps sequentially:
> Step 1: Identify the primary search intent shift causing the drop.
> Step 2: Pinpoint 2-3 critical content gaps compared to top SERP competitors.
> Step 3: Generate a 3-step prioritized refresh execution plan in a Markdown Table.
> 
> Show your step-by-step reasoning inside <thinking> tags before generating <analysis> and <action_plan> blocks.
> </instructions>"

**Output Excerpt:**
> <thinking>
> Step 1: Diagnosing the Intent Shift... Onboarding-related queries have been migrating from informational toward commercial-investigative / template-driven intent...
> Step 2: Content Gaps vs. Top SERP Competitors... Top 3 competitors win on structured, extractable frameworks, visual proof, and fresh benchmark data...
> Step 3: Prioritization Logic... Prioritize actions by impact-to-effort ratio, favoring structural/schema fixes first...
> </thinking>
> 
> <analysis>
> Intent Shift: The query cluster has drifted from informational to a hybrid commercial-investigative intent...
> Critical Content Gaps: No scannable framework, missing visual proof, stale data...
> </analysis>
> 
> <action_plan>
> | Priority | Action | Impact | Effort | Justification |
> | :--- | :--- | :--- | :--- | :--- |
> | 1 | Restructure into a numbered "Onboarding Framework" | High | Low | Directly targets featured snippet / AI Overview extraction |
> | 2 | Refresh stats with current-year benchmark data | Medium | Low | Restores freshness/E-E-A-T signals |
> | 3 | Add 2–3 annotated product screenshots | High | Medium | Builds first-hand proof of expertise |
> </action_plan>

**Observed Output Note:**
- **What changed:** Forced step-by-step decomposition with explicit `<thinking>` tags.
- **Observed difference:** The model's recommendations became noticeably more logical and hierarchical because it was forced to evaluate root causes before committing to action items.

---

## 7. Cross-Model Comparison (Claude 3.5 Sonnet vs. ChatGPT GPT-4o)

The final Version 5 prompt was executed on both **Claude 3.5 Sonnet** and **ChatGPT (GPT-4o)** using identical parameters.

| Metric | Claude 3.5 Sonnet | ChatGPT (GPT-4o) |
| :--- | :--- | :--- |
| **Tone** | Highly analytical, professional B2B strategist tone deeply attuned to SEO signals (AI Overviews, SERP reclassifications, E-E-A-T). | Professional and structured, but leaned toward general content advice rather than deep technical SEO mechanics. |
| **Accuracy & Constraints** | Followed XML tag requirements (`<thinking>`, `<analysis>`, `<action_plan>`) 100% perfectly. | Refused/skipped `<thinking>` tags due to system safety policies ("Sorry, I can't provide my private reasoning...") and skipped `<analysis>` tags. |
| **Structure** | Strictly compartmentalized the output inside requested XML tags without conversational clutter. | Generated `<action_plan>` correctly, but added extra conversational preambles and summary text outside tags. |
| **Failure Points** | None; adhered strictly to all structural, formatting, and reasoning instructions. | Failed compliance on `<thinking>` and `<analysis>` tags due to model system constraints on internal thinking outputs. |

---

## 8. Final Reusable Production Template

```markdown
Role: Act as a Senior SEO Content Strategist specializing in B2B SaaS content decay recovery.

<context>
- Inventory Size: [Insert e.g., 30,000 URLs]
- Target Asset: [Insert URL or Topic]
- Performance Delta: [Insert e.g., Dropped from Rank X to Rank Y, Z% traffic loss]
- Resource Constraint: [Insert e.g., Minimal editorial rework]
</context>

<example>
Input: Rank 3 to 12 drop on 'SaaS Pricing'
Output:
| Action | Impact | Effort | Justification |
| :--- | :--- | :--- | :--- |
| Add Tier Comparison Table | High | Low | Featured snippet target |
</example>

<instructions>
Execute the evaluation using step decomposition:
1. Identify primary search intent shift.
2. Audit content gaps against top-ranking competitors.
3. Create a 3-item prioritized action matrix in Markdown Table format.

Output requirements:
- Place step-by-step evaluation inside <thinking> tags.
- Place root cause diagnosis inside <analysis> tags.
- Place final execution matrix inside <action_plan> tags.
</instructions>