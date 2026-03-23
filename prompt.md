# AI Infrastructure Readiness Dashboard

### Role & Context

You are a Senior Infrastructure Analyst supporting a Strategy Lead at Red Hat focused on AI Platform markets. The goal is to assess a region's readiness to host high-density AI infrastructure for a specific accelerator configuration. You will produce both a structured dataset and a working visualization.

### Objective

Build a lightweight, single-file HTML5/JavaScript dashboard that visualizes the "Infrastructure Readiness Gap" for deploying the Reference Configuration (defined below) across every country in the defined region. The work proceeds in two phases: first, assemble the data; second, render the dashboard.

---

### Reference Configuration: NVIDIA GB200 NVL72 Rack

All readiness assessments must be scored against these thresholds:

| Parameter | Requirement |
|---|---|
| Power per rack | 120-150 kW |
| Cooling | Rack-level direct liquid cooling (DLC) required — cold plates on GPU dies with liquid-to-liquid heat exchange. Rear-door heat exchangers and ambient/facility-level cooling are insufficient on their own. |
| Rack weight | ~1,200-1,500 kg |
| Floor loading | >=15 kPa (EN 12825 European raised floor standard) |
| Network | Low-latency interconnect (InfiniBand or RoCEv2) between racks; proximity to major IX |

**European Baseline (for context):** The Uptime Institute's 2025 Global Survey found the most common rack density across European data centers is still **5-9 kW per rack** — a 15-20x gap from what the GB200 NVL72 requires. Only a handful of specialized AI infrastructure providers have operational 120kW+ DLC facilities in Europe as of early 2026. The FLAP-D markets (Frankfurt, London, Amsterdam, Paris, Dublin) — traditionally Europe's dominant data center hubs — face grid saturation and moratoriums that are driving AI infrastructure toward the Nordics and Iberia.

---

### Region Definition: Europe

Include **all** of the following countries. No country may be omitted.

**EU-27:** Austria, Belgium, Bulgaria, Croatia, Cyprus, Czech Republic, Denmark, Estonia, Finland, France, Germany, Greece, Hungary, Ireland, Italy, Latvia, Lithuania, Luxembourg, Malta, Netherlands, Poland, Portugal, Romania, Slovakia, Slovenia, Spain, Sweden.

**EEA + Key Non-EU:** Norway, Iceland, Liechtenstein, Switzerland, United Kingdom.

**Balkans & Eastern Europe:** Albania, Bosnia and Herzegovina, Kosovo, Montenegro, North Macedonia, Serbia, Moldova, Ukraine, Belarus.

**Other Geographic Europe:** Turkey, Georgia.

Total: 43 countries.

---

### Variables to Analyze per Country

Assess each country against six variables. Score every variable using the Status Logic defined below.

1. **Power Density** — Can the country's electrical grid deliver >=120 kW per rack to a data center facility? Distinguish between two fundamentally different failure modes:
   - **Grid bottleneck** (e.g., Frankfurt, Dublin, Amsterdam): The electrical grid itself is saturated — no new connections available regardless of facility readiness. This is a structural constraint that takes years to resolve.
   - **Facility bottleneck** (e.g., Finland, Portugal): The grid has available capacity and renewables, but specific AI-grade data center facilities are still being built. This is a timing constraint, not an infrastructure constraint.
   - A country where the grid has capacity and companies are actively building AI infrastructure should rate higher than a country under an effective grid moratorium. Consider national grid capacity, grid connection wait times, announced utility expansions, existing hyperscaler presence, and electricity pricing.

2. **Cooling Maturity** — Is rack-level Direct Liquid Cooling operational in commercial data centers in this country? Score this carefully using these distinctions:
   - **Cold climate / fjord water / low PUE** = ambient/facility-level cooling. Beneficial for total energy costs but NOT a substitute for rack-level DLC. A cold building still needs cold plates on GPU dies.
   - **"DLC-ready" / "designed for liquid cooling" / "liquid cooling planned"** = infrastructure prepared or announced but NOT operational. Rate Orange at best.
   - **Operational rack-level DLC at 120kW+ density in commercial facilities** = Green.
   - **Research/government supercomputers** (LUMI, CSCS Alps, MareNostrum, JUPITER) demonstrate that DLC works in a country and prove supply chain and workforce readiness — note as evidence of proven capability. However, these are not commercial colocation: an enterprise customer cannot deploy a GB200 rack into LUMI. A country with only research DLC and no commercial DLC should be rated based on how close commercial deployments are.

3. **Structural Load** — Can existing or planned data center facilities support >=15 kPa floor loading and ~1,500 kg per rack? Consider typical building standards (EN 12825 classifications), whether structural retrofits are underway, and whether new builds are purpose-designed for AI-density loads.

4. **Network Connectivity** — Does the country have proximity to major Internet Exchanges, high fiber density, and availability of InfiniBand/RoCEv2 interconnect at scale? Consider submarine cable landing points, IX membership counts, and latency to major European hubs.

5. **Legislation & Regulation** — Are there active or pending legislative impediments? Consider: data center construction moratoriums (Amsterdam since 2019, Dublin until Dec 2025, Frankfurt de facto), energy consumption caps, water usage restrictions (Thames Water in UK), zoning limitations, the EU Energy Efficiency Directive (EED), Germany's EnEfG, and data sovereignty mandates that affect site selection.

6. **Sovereign AI Deployment** — Does this country operate its own national AI compute infrastructure domestically, or must it deploy abroad? Pay special attention to the **Sovereign AI Paradox**: major economies (notably Germany) that want domestic AI compute sovereignty but cannot physically deploy it due to grid constraints, forcing relocation of AI training workloads to Finland, Norway, or Sweden. This is creating a **strategic bifurcation** in European AI geography — latency-tolerant training migrates north to where power is available, while latency-sensitive inference stays in established metros. Note which countries are *sources* vs *destinations* of this migration. Reference the EU's EuroHPC AI Factories program (3 rounds: Dec 2024, Mar 2025, Oct 2025, plus Antenna nodes) as the primary coordinated sovereign AI effort.

---

### Status Logic

| Color | Meaning | Action |
|---|---|---|
| Green | Capacity exists and is deployable today | — |
| Orange | Limited capacity or active retrofitting/construction underway | Provide "Announced Timeline" (see below) |
| Red | No capacity exists, or an active moratorium/ban is in effect | Provide "Announced Timeline" (see below) |
| Grey | Insufficient data to make an assessment | Label as "N/A — Insufficient Data" |

**Announced Timeline rule:** For Orange and Red countries, cite a concrete government announcement, utility expansion plan, or construction permit with a target date. If no public timeline exists, state: *"No announced public timeline as of early 2026."* Do not speculate or invent forecasts.

---

### Critical Evaluation Rules

These rules address the most common failure modes when building this dashboard. Violating them will produce a systematically over-rated, misleading assessment.

**1. The "This Quarter" Test:**
Do NOT conflate "announced," "planned," "under construction," "MoU signed," or "DLC-ready" with "operational today." Headlines about billion-dollar investments and groundbreaking ceremonies do not mean infrastructure is deployable. Apply this test for Green ratings: *"Could an enterprise customer deploy a GB200 NVL72 rack in this country's commercial data center THIS QUARTER?"* If the answer depends on a facility that hasn't opened yet, the rating is Orange, not Green.

**2. Ambient Cooling Is Not DLC:**
A country with cold weather, fjord water, or low facility PUE does NOT automatically get a Green cooling rating. The GB200 NVL72 requires cold plates on GPU dies with liquid-to-liquid heat exchange at the rack level. This is a separate infrastructure layer from building-level HVAC, free air cooling, or chilled water to CRAHs. Both are needed; only rack-level DLC satisfies the cooling variable.

**3. Research Infrastructure Is Not Commercial Capacity:**
European governments operate world-class AI supercomputers (LUMI in Finland, JUPITER in Germany, MareNostrum 5 in Spain, CSCS Alps in Switzerland). These prove national DLC capability and power delivery, but they are NOT commercial colocation. An enterprise customer cannot rent rack space in LUMI. Cite these systems as evidence of capability (especially for cooling and power), but do not rate a country Green for commercial readiness based solely on a government research facility.

**4. Grid Saturation ≠ Facility Construction:**
Frankfurt can't connect new data centers until ~2030 because the GRID is full. Finland's facilities are under construction but the GRID has capacity. These are fundamentally different situations and must not receive the same rating. The destination of the AI migration (Finland, Norway, Sweden) should not be rated the same as the source of the migration (Germany, Ireland, Netherlands) when the bottleneck is different.

**5. Pipeline ≠ Capacity:**
"5 GW in the pipeline" and "500 MW operational" are very different statements. Always distinguish between announced/planned capacity and operational capacity. A massive pipeline is evidence of market momentum (supports a higher rating) but is not the same as deployable infrastructure.

---

### Key European AI Infrastructure Providers to Research

These are the primary operators with confirmed or near-term operational 120kW+ DLC facilities in Europe. Use them as starting points, not an exhaustive list:

- **Nscale** — Operational DLC facilities in UK (Loughton, 50 MW scalable to 90 MW). Deploying in Portugal (Start Campus, Sines — 12,600 GB300 GPUs, 150 MW Phase 1) and Iceland (Verne partnership — 4,600 GB300 GPUs). Building UK's largest NVIDIA AI supercomputer with Microsoft (23,000 GB300 GPUs).
- **CoreWeave** — Operational liquid-cooled NVIDIA Blackwell racks (~130kW/rack) in Sweden and Spain. $2.2B Nordic expansion program. Facilities also in Norway.
- **Verne Global** — 140 MW campus in Iceland (40 acres, 100% renewable). Partnership with Nscale for Blackwell Ultra deployment. NVIDIA DGX authorized.
- **EuroHPC AI Factories** — EU sovereign AI compute program across 19+ sites:
  - Round 1 (Dec 2024): Finland, Germany, Greece, Italy, Luxembourg, Spain, Sweden
  - Round 2 (Mar 2025): Czech Republic, Ireland, Poland, Slovenia
  - Round 3 (Oct 2025): Denmark, France, Latvia, Lithuania, Netherlands, Romania
  - Antennas (Oct 2025): Belgium, Cyprus, Hungary, Iceland, Malta, Moldova, North Macedonia, Serbia, Slovakia, Switzerland, UK, and others

---

### Data Fidelity & Citation Guardrails

- **Exhaustive mapping:** Every country in the Region Definition must appear on the map and be color-coded. Do not filter for "top markets" or "Tier 1" countries.
- **Proxy logic:** If specific data is unavailable for a country, color it Grey and label it "N/A — Insufficient Data." Do not infer or guess.
- **Citations:** Each data point must include a source name, publication year, and URL. If you cannot verify a URL is real, mark the citation as **[Unverified]** and provide the source name and publication year only. Do not fabricate URLs.
- **Data points per country:** Provide 3-5 sourced data points per variable where data is available. Each data point should be 2-3 sentences. Prioritize 2024-2026 sources.
- **No hallucinated specifics:** Do not invent facility names, capacity figures, or legislation that you cannot source. When uncertain, say so explicitly.
- **Distinguish tiers of evidence:** Primary sources (company press releases, government publications, regulatory filings) > Industry analyst reports (CBRE, Cushman & Wakefield, Mordor Intelligence) > News coverage > Blog posts. Note the source type.

---

### Application Specifications

#### Technical Constraints
- Single HTML file. All CSS and JavaScript inline.
- Load Leaflet.js and its CSS via CDN `<script>` and `<link>` tags.
- Load a simplified European GeoJSON via CDN or embed a low-resolution version to keep file size manageable.
- No build tools, no server dependencies.

#### Primary Screen — Vertical 50/50 Split

**Left Panel: Strategic Summary**
- Overall readiness narrative for the region (3-5 paragraphs). Must address:
  - The 15-20x density gap between typical European DCs and GB200 requirements
  - The Sovereign AI Paradox and strategic bifurcation (training to Nordics, inference in metros)
  - FLAP-D grid saturation dynamics
  - Which providers actually have operational 120kW+ DLC in Europe
- A table or list of countries sorted by overall readiness (Green -> Orange -> Red -> Grey).
- A dedicated subsection: **"Sovereign AI Cross-Border Dependencies"** — list every country where national AI compute is hosted outside its own borders, name the host country, and explain why.
- A color-coded legend matching the Status Logic.

**Right Panel: Interactive Map**
- Leaflet.js map centered on Europe with zoom/pan.
- Every country color-coded by its **worst** variable score (i.e., if Power is Green but Cooling is Red, the country shows Red).
- Hover tooltip: Country name + overall status.
- Click interaction: navigates to the Detail Screen.

#### Detail Screen — Triggered by Country Click

- Country name and overall status prominently displayed.
- One section per variable (Power, Cooling, Structural, Network, Legislation, Sovereign AI), each showing:
    - Status color and label.
    - Rationale paragraph.
    - 3-5 sourced data points with citations.
    - Announced timeline (if Orange or Red).
- A prominent **"<- Back to Summary"** button that returns to the Primary Screen.

#### UI Details
- Clean, professional styling. Dark theme preferred for data visualization density.
- Filter dropdown on the Primary Screen to filter the map by individual variable (e.g., show only Cooling status).
- Responsive is not required — optimize for a 1920x1080 desktop viewport.

---

### Execution Sequence

**Phase 1 — Data Assembly:** Research and assemble the structured data for all 43 countries across all 6 variables before writing any HTML. Apply the Critical Evaluation Rules during this phase. Output this as a JavaScript object/JSON embedded in the file. For each variable per country, include: status (green/orange/red/grey), rationale (1-2 sentences), dataPoints array (each with text, source, year, url), and timeline (for orange/red).

**Phase 2 — Dashboard Build:** Render the dashboard consuming the Phase 1 data. All visual elements must be driven by the data object so the dataset can be updated independently.

---

### Execution Target

| Parameter | Value |
|---|---|
| Region | Europe (43 countries, as defined above) |
| Accelerator | NVIDIA Blackwell (GB200 NVL72 configuration) |
