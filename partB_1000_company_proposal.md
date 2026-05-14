# Part B — Sourcing Methods & 1000-Company Scale-Up Proposal

## Question 1 — Sourcing Methods (how to find Federer companies)

1. Government & regulatory databases
   - Ministry of Corporate Affairs (MCA) / RoC filings: ownership, revenues, annual returns (used to filter revenue bands and ownership).  
   - Central Board of Indirect Taxes & Customs export registries and IEC lists: exporters.  
   - USFDA and PIC/S manufacturing site databases: identify regulated exporters and approvals.  
   - DSIR/DSIR-recognized R&D lab lists and MSME registries.
   - Why: authoritative for revenue, legal status, regulated manufacturing.  
   - Limitations: MCA data lags; requires name matching and entity disambiguation.

2. Cluster directories & industry associations
   - Genome Valley tenant lists, IKP/Neovantage directories, Hyderabad Pharma City tenant rosters.  
   - Industry associations: Indian Chemical Council, Indian Drug Manufacturers' Association, Biotech Consortium India Ltd (BCIL), FICCI, CII sector lists.
   - Why: clusters concentrate manufacturers and reduce noise.  
   - Limitations: membership bias; includes CROs and service providers that must be filtered.

3. Trade, tender, and procurement data
   - Government tenders (eProcurement portals), defence procurement notices, export promotion council lists (Pharmexcil).  
   - Why: signals production capability and product categories.  
   - Limitations: requires parsing and entity matching.

4. Commercial databases + APIs
   - Toofler / Tofler API, Kompany, D&B, Orbis (if available), Crunchbase, Tracxn, Apollo, ZoomInfo.  
   - Why: fast metadata, revenue bands, funding, and ownership flags.  
   - Limitations: cost; coverage gaps for smaller private firms.

5. Job-boards & hiring signals
   - LinkedIn, Naukri, Indeed — scrape recent roles and counts to detect hiring intensity.  
   - Why: captures C6 growth signals at scale.  
   - Limitations: noise from sales office listings; requires deduplication.

6. Technical/regulatory signals
   - Patent databases, grant/DSIR records, US/EU regulatory approvals, certifications (ISO, AS9100, GMP).  
   - Why: evidence for C3 differentiation and regulated capability.  
   - Limitations: matching patents to small companies is noisy.

7. Events, conferences & supplier lists
   - BioAsia, CPhI India, Chemex, regional expos, OEM supplier lists (automotive/aerospace).  
   - Why: yields niche, active manufacturers and recent expansion signals.
   - Limitations: manual curation needed.

8. Creative sources
   - Procurement disclosures from large OEMs, supplier lists on tender PDFs, import/export HS-code filtering to find niche chemistries, alumni networks (IIT/IISc), and local industry WhatsApp/Telegram groups.

For each method include: what to scrape/API, expected yield, primary filters to apply (manufacturing footprint, revenue band, promoter ownership).

---

## Question 2 — 1000-Company Proposal (one-month plan)

Objective: produce 1,000 ICP-qualified candidate records (Federer-screened) in 4 weeks with automated pipelines and human QA to maintain quality.

Assumptions
- Full access to paid data sources (Tofler/Orbis/ZoomInfo), API credits, web-scraping infra, and 1–2 junior research contractors for verification.

Pipeline overview
1. Universe generation (week 1): aggregate candidate lists from MCA, cluster directories, trade registries, API vendor lists, and event attendee lists. Target raw universe = 8,000–12,000 names (8–12× expected 1,000 after filtering).  
2. Automated enrichment (week 1–2): enrich each candidate with website, location, revenue estimate (MCA/Tofler), LinkedIn company id, and basic signals (certifications, USFDA/PICs flags, patent hits).  
3. Rule-based auto-qualification (week 2): apply deterministic filters to drop non-manufacturers, PE-controlled, revenues > Rs.500Cr, and duplicates.  
4. Scoring (week 2–3): compute the Federer Score automatically from enriched signals using the assignment rubric. Mark candidates as Pass (score≥60), Borderline (40–59), Fail (<40).  
5. Focused evidence enrichment (week 3): for Pass and Borderline candidates (~1,500 records) run targeted scrapes: company pages, news, LinkedIn job counts, and MCA filings to collect the specific per-criterion evidence lines.  
6. Human QA & adjudication (week 3–4): sampling-based QC and full review for final 1,000 records — each Pass record gets a human reviewer who verifies C1 and C6 at minimum.  
7. Output packaging (end week 4): prepare CSV/Excel with per-criterion evidence, summary report, and a change log of excluded companies with reasons.

Week-by-week schedule
- Week 1 (Universe build + basic enrichment): build raw list, dedupe, start automated enrichment.  
- Week 2 (Automated qualification + scoring): run qualification rules, compute Federer Score, prioritize candidates.  
- Week 3 (Focused evidence collection): targeted scraping for top candidates, collect URLs and evidence lines.  
- Week 4 (QA + finalization): human verification pass, fix edge cases, produce deliverables and README.

Automation & tooling
- Orchestration: Airflow or Prefect for ETL; short-run Node/Python scrapers.  
- Enrichment: Toofler/Orbis API for revenue/ownership; Clearbit/ZoomInfo for company metadata; USFDA/PICs scrapers for approvals.  
- Hiring signals: LinkedIn jobs via company page scraping (rate-limited) and Naukri search scraping.  
- Scoring engine: small Python service that applies rubric weights and outputs per-criterion classifications and total score.  

Quality control
- Automated sanity checks: revenue vs. employee consistency, duplicate detection, and domain-matching.  
- Statistical sampling: random 5% of Pass records checked by humans; if error rate >10% trigger larger review and adjust rules.  
- Adjudication flow: ambiguous cases routed to senior reviewer with checklist.  

Expected yields (conservative)
- Raw universe → after basic filters: 12,000 → 4,000  
- After scoring → Pass/Borderline list: 4,000 → 1,800  
- After evidence enrichment & QA → Final ICP-quality set: 1,800 → 1,000  
- Expected net yield: ~8–10% from initial broad web scrape to verified ICP targets, depending on segment density.

Team & resourcing
- 1 project lead (strategy + adjudication)  
- 2 data engineers (pipeline + enrichment)  
- 3 researchers (evidence collection + QA)  
- Optional: 1 data labeling contractor for bulk verification

Deliverables
- `partA_25_companies.csv` style CSV scaled to 1,000 rows with the same columns + per-claim source URLs  
- `methodology_1000.md` describing scripts, queries, and rules  
- A reviewed subset of 100 highest-priority targets with full evidence for outreach

Risks & mitigations
- False positives (service companies): strong C1 heuristics (manufacturing keywords, site pages, plant addresses) and mandatory human check for final included records.  
- Ownership / revenue errors: cross-check MCA and Toofler; flag ambiguous ownership for human review.  
- Rate limits and blocking: use proxy pools, polite scraping, vendor APIs for heavy lifts.

---

## Hand-drawn Diagram Instructions (MANDATORY for submission)

Draw a single-page funnel diagram that shows:  
- Left: Source buckets (MCA, cluster lists, Toofler, events, LinkedIn jobs) with approximate counts.  
- Middle: Automated steps (enrichment → rule filters → scoring).  
- Right: QA steps (sampling, human adjudication) and outputs (1,000 verified records, 100 outreach-ready).  
- Annotate tools used at each stage (APIs, scrapers, scoring engine) and weekly timeline below the funnel (Week 1–4).  

Take a photo and upload to Internshala chat as required. The diagram must be hand-drawn — photos of whiteboard drawings are acceptable. Include a short caption with your name and date.

---

If you want, I can now: (A) expand `outputs/partA_25_companies.csv` with direct source URLs per evidence claim, or (B) start building the automated scoring script skeleton (Python) that computes the Federer Score from enriched fields. Which should I do next?  
