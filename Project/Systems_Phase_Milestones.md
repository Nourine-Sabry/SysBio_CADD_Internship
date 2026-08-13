# Systems Biology Phase — Capstone Project
### BRIC Research Centre Internship — Project Brief (Milestones 1–3)

This document is your complete project brief for the Systems Biology phase of the capstone. It covers all three milestones end to end: choosing and framing a disease, building and analyzing its gene network, identifying consensus hub proteins, and selecting a defensible and undrugged target candidate to carry forward.

Each milestone lists its steps, its deliverables, and troubleshooting is consolidated at the end so you have one place to check when something isn't working.

---

## Before you start

- Pick a disease (Rheumatoid Arthritis, Crohn's disease, etc...) of interest.
- Keep a running notes file from the start: Document every gene list source, confidence threshold, and parameter choice as you go. You will need to justify these choices in your final write-up.

---

## Milestone 1 — Disease Selection & Network Construction

### Step 1: Choose your disease

Pick a disease with a reasonably well-characterized genetic/molecular basis — this matters because your network needs enough associated genes to be analyzable (aim for at least 30–50 genes; fewer than that makes hub/module analysis statistically weak).

### Step 2: Formulate your research question

A good research question is specific, not generic. Compare:

- Weak: *"What genes are involved in [disease]?"* -> purely descriptive, no analytical question.
- Strong: *"Which proteins act as structural hubs in the [disease]-associated interactome, and are any of them viable but currently undrugged therapeutic targets?"*

Your question should point toward the specific analysis you're about to run.

### Step 3: Write your disease introduction

1–2 paragraphs covering: what the disease is, its general prevalence/burden, its known or suspected molecular basis, and why a network-based approach is a reasonable way to study it. This is context-setting for a reader who doesn't already know the disease — write it that way.

### Step 4: Obtain your disease gene network

- Use STRING's built-in disease query: search your disease name directly in STRING's disease search, which returns a pre-associated gene network from STRING's own curated disease-gene data.
- Or alternatively, use the STRING web interface to obtain your network (filter to high confidence proteins), then import the network into cytoscape.

### Step 5: Import into Cytoscape and run Network Analyzer

1. Send/export your STRING network into Cytoscape.
2. `Tools → NetworkAnalyzer → Analyze Network` (undirected, since STRING interactions have no direction).
3. Record the basic network statistics: number of nodes, number of edges, average degree, network diameter, average clustering coefficient.

### Milestone 1 Deliverables

- [ ] Your research question
- [ ] Disease introduction (1–2 paragraphs)
- [ ] Screenshot of your imported network
- [ ] Network Analyzer summary statistics
- [ ] Methods note: gene list source, confidence threshold, and STRING interaction score cutoff used

---

## Milestone 2 — Hub Identification via CytoHubba

### Step 1: Open CytoHubba on your network

Confirm your disease network is the active network in the Network Panel before running anything — this is the single most common setup error at this step.

### Step 2: Run at least three ranking methods

Run, at minimum:
- **Degree** (local)
- **MCC** — Maximal Clique Centrality (hybrid)
- **BottleNeck** (global/path-based)

### Step 3: Export the top-10 list for each method

Sort the cytoHubba results table by each method in turn and record the top 10 gene names per method.

- N.B.: If there are no common proteins in the top 10 list, expand the list to top 20 or even 30.

### Step 4: Build your consensus table

Lay the three top-10 lists side by side. Mark every gene that appears in **at least 2 of the 3** lists — this is your consensus hub set.

**Suggested table format:**

| Gene | Degree rank | MCC rank | BottleNeck rank | # methods in top 10 |
|---|---|---|---|---|
| example: TNF | 1 | 3 | 2 | 3 |
| example: IL10 | 6 | — | 4 | 2 |

### Step 5: Shortlist your strongest candidates

From your consensus set, identify 1-3 genes with the strongest overall agreement (appearing in all 3 lists, or ranking near the top across multiple lists) — these carry forward into Milestone 3.

### Milestone 2 Deliverables

- [ ] Top-10 list for each of the 3 (or more) cytoHubba methods run
- [ ] Consensus hub table (as above)
- [ ] A one-sentence note identifying your single strongest overall candidate, with justification

---

## Milestone 3 — Druggability Check & Target Discussion

### Step 1: Extract the DrugBank linkset in CyTargetLinker

`Apps → CyTargetLinker → Extract Link Sets → DrugBank (human)`. This requires an internet connection and is a one-time download per machine.

### Step 2: Extend your network

`Apps → CyTargetLinker → Extend Network` → select your disease network and the DrugBank linkset you just extracted → run. New drug/compound nodes will appear, attached only to genes in your network with a known curated DrugBank relationship.

### Step 3: Check your node labels

CyTargetLinker's extension step sometimes fails to carry over your original label mapping to the new network — if your original gene names have disappeared, go to the Style tab of the new network and re-map the Label property to the correct name column (usually `display name`).

### Step 4: Select your undrugged candidate

From your consensus hub shortlist, choose **one gene with zero (or minimal) DrugBank connections**. This is your final target candidate — a protein with strong network-level and disease-relevance evidence, but no established drug precedent.

### Step 5: Write your discussion

1–2 paragraphs covering:
- Why this gene is a strong candidate despite the lack of existing drugs — reference its specific network evidence (which methods flagged it, its actual rank/score values) and its disease relevance
- A plausible reason it may be undrugged — e.g., a relatively recently characterized role in the disease, an historically difficult target class (see the target-class discussion from the earlier druggability sessions), or simply an unexplored opportunity
- An honest limitation: absence of a DrugBank link is evidence of absence of *precedent*, not proof the target is undruggable — structural tractability (pocket availability) hasn't been assessed yet at this stage and would need to be checked before concluding this is a genuinely viable target

### Milestone 3 Deliverables

- [ ] Screenshot of your CyTargetLinker-extended network
- [ ] Your final selected undrugged target gene, clearly stated
- [ ] Discussion paragraph(s) as described above

---

## Final Submission Checklist

- [ ] Research question and disease introduction (Milestone 1)
- [ ] Network screenshot + Network Analyzer stats + methods note (Milestone 1)
- [ ] Three cytoHubba top-10 lists + consensus table (Milestone 2)
- [ ] CyTargetLinker-extended network screenshot (Milestone 3)
- [ ] Final undrugged target gene, clearly named, with discussion (Milestone 3)
- [ ] All gene list sources, thresholds, and parameters documented throughout

---

## Troubleshooting

| Problem | Likely cause / fix |
|---|---|
| STRING network comes back too small or too large | Adjust the confidence score threshold (default 0.7); lower it for more (less certain) interactions, raise it for fewer (more certain) ones |
| Network Analyzer results look empty or default | Confirm the network fully imported before running — try re-running after a few seconds if STRING data is still loading |
| cytoHubba results look wrong or empty | Confirm the correct network is active in the Network Panel before running — this is the most common cause of this problem |
| Top-10 lists across all three methods are nearly identical | Not an error — on small or very dense networks this can legitimately happen; note it as a finding (strong agreement across methods is itself informative) |
| CyTargetLinker "Extract Link Sets" fails or hangs | Requires an internet connection; retry, or check for firewall/network restrictions on the lab machines |
| Node names disappear after "Extend Network" | Known CyTargetLinker quirk — select the new network → Style tab → re-map the Label property to the correct name column (usually `shared name`) |
| A gene shows zero drug connections | Not automatically disqualifying — first confirm the linkset is working by testing a known heavily-drugged gene (e.g., TNF); if that also shows zero, the extension likely failed and should be re-run |
| Gene symbols don't match between tools | CyTargetLinker and cytoHubba both match on identifiers (usually gene symbol or shared name) — inconsistent capitalization or alternate gene aliases can cause silent mismatches; check the Node Table for the exact symbol being used |
| Consensus table has no genes overlapping across all 3 methods | Lower your bar to "at least 2 of 3" rather than requiring all 3 — genuine 3-way agreement isn't guaranteed on every network, and 2-of-3 agreement is still meaningful evidence |
| Unsure which gene to pick as the final undrugged candidate | Prioritize the gene with the strongest Milestone 2 consensus evidence first, and only move to your next-strongest candidate if your top choice turns out to already have drug precedent |
