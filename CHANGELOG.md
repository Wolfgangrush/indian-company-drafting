# Changelog

All notable changes to the `indian-company-drafting` plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/) and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [0.2.3-alpha] — 2026-05-25

### Explicit per-agent invocation of `pair_md_to_docx.sh`

v0.2.2 documented the output-pairing rule in `_drafting_common/SKILL.md` and relied on every agent picking up the rule by reference. v0.2.3 makes the invocation EXPLICIT in each agent's prompt — Reader, Format, Drafter, Verifier, Refiner, Overseer — so the pairing happens deterministically rather than depending on inherited-rule compliance.

### Changed

- **Reader prompt** — after writing `case-facts.md`, explicit `pair_md_to_docx.sh case-facts.md` invocation appended.
- **Format prompt** — after writing `format-shell.md`, explicit invocation appended.
- **Drafter prompt** — explicit invocation appended (Drafter already had a pandoc command from v0.2.1; the helper invocation is now also documented as the canonical path).
- **Verifier prompt** — after writing `verification-report.md`, explicit invocation appended.
- **Refiner prompt** — after writing `draft-v2.md`, explicit invocation appended.
- **Overseer prompt** — after writing `opposing-notes.md` and `final-draft.md`, two explicit invocations appended.

### Why the change

User feedback 2026-05-25: relying on each agent to inherit the rule from `_drafting_common` is not robust enough. The Drafter has the pandoc command spelled out and it works; the other 5 agents had only the inherited rule. Explicit per-agent invocation makes the pairing deterministic. Once every agent reliably outputs both `.md` and `.docx`, a pipeline run on any forum becomes itself a calibration probe — the advocate visually inspects the rendered `.docx` from each stage and identifies any per-forum formatting gaps, without needing 14 separate gold-standard pleadings upfront.

---

## [0.2.2-alpha] — 2026-05-24

### Output-pairing discipline — every `.md` paired with `.docx`

Advocates do not natively read Markdown. Every pipeline output artifact (case-facts.md from Reader, format-shell.md from Format, draft-v1.md from Drafter, verification-report.md from Verifier, draft-v2.md from Refiner, opposing-notes.md + final-draft.md from Overseer) is now paired with a corresponding `.docx` rendered using the same locked Word styles in the shipped reference.docx.

### Added

- **`pair_md_to_docx.sh`** — helper script in `skills/<base>/` that every agent calls after writing a `.md` output. Wraps the two-step pandoc + fix_docx_tables.py pipeline so every agent produces a paired `.docx` without re-implementing the conversion logic.
- **OUTPUT-PAIRING DISCIPLINE** section in `_drafting_common/SKILL.md` documenting the per-agent output-pairing map (Reader → case-facts.{md,docx}; Format → format-shell.{md,docx}; Drafter → draft-v1.{md,docx}; Verifier → verification-report.{md,docx}; Refiner → draft-v2.{md,docx}; Overseer → opposing-notes.{md,docx} + final-draft.{md,docx}).

### Why the change

User feedback from the 2026-05-24 EPFO test demonstrated that the QC pipeline output (`verification-report.md`, `opposing-notes.md`) was not accessible to the advocate in their normal Word workflow. The advocate explicitly stated: "every note … needs to be docx too." v0.2.2 closes this gap.

### Clarification — per-court formatting

v0.2.1 propagated a single High Courts of India pleading-style reference.docx across all 14 plugins. The structural styling (TNR 14pt 1.5 spacing 4cm-left margin Heading 1/2/3/4) is broadly defensible for pleading-style plugins (HC / SC / Tax / Rent / MACT / Banking / Company / Consumer / Labour / Family / IP / District Court) because the court-specific differences (cause-title text, annexure prefix, statutory opening, AOR Certificate language) live in the case-type SKILL.md (Drafter content) not the reference.docx (style template). For SC the universal style is correct as the SC Registry mandate matches the HC convention (A4 + TNR 14pt + 1.5 spacing + 4cm left margin). Court-specific content (P-1/P-2 annexure prefix instead of ANNEXURE-A; SYNOPSIS + LIST OF DATES instead of just INDEX; AOR Certificate verbatim) is rendered by the Drafter from the case-type skill. Per-bench fine-tuning (e.g., Delhi HC double-spacing under Original Side Rules 2018; Punjab & Haryana watermarked paper) is achieved by supplying a case-folder reference.docx override.

For the two TRANSACTIONAL plugins (indian-contracts-drafting-litigation + indian-property-drafting-litigation), v0.2.1 wrongly applied the pleading-style reference.docx. Those two plugins now ship a transactional-instrument variant (TNR 12pt single-spaced, no spaced section headers, no underline on headings) under their own v0.2.2 release.

---

## [0.2.1-alpha] — 2026-05-24

### Filing-grade render-defect repair + pipeline-optionality

The v0.1.0 render path produced filing-grade Markdown but the pandoc → `.docx` conversion failed Bombay HC / equivalent Registry expectations on multiple counts (title not bold, section headers left-aligned, Index table column-headers wrapping vertically, party block leaking onto cover pages, ~6,200-word bloat). This release repairs the render path, calibrated against an actual filed High Courts of India Second Appeal pleading the author supplied as the filing-grade reference. Inherits the v0.2.1 fixes from `indian-hc-drafting-litigation`.

### Added

- **Pre-customised `reference.docx`** in the plugin's base-skill folder with locked Word styles (TNR 14pt body, 1.5 line spacing, 4cm left / 2.5cm right-top-bottom margins, Heading 1 bold centered, Heading 2 bold + UNDERLINED + centered + letter-spacing for the spaced `F A C T S` effect, Heading 3 bold + UNDERLINED + centered for unspaced section headers, Heading 4 bold + UNDERLINED + left for `MOST RESPECTFULLY SHEWETH:` style anchors, fixed table layout).
- **`build_reference_docx.py`** — reproducible python-docx build script for the shipped reference.docx.
- **`fix_docx_tables.py`** — post-pandoc Python script that forces column widths on every table (5-col 8/8/60/14/10; 4-col 10/10/65/15; 3-col 10/75/15; 2-col 18/82). Locks first-row bold + centered + vertically-centered cells. Drafter runs this as the final post-pandoc step.
- **MARKDOWN HEADING DISCIPLINE** in the Drafter prompt + base SKILL.md (Heading 1 / Heading 2 / Heading 3 / Heading 4 mapping for court header / spaced section headers / unspaced section headers / left-anchored headings).
- **VERBOSITY DISCIPLINE** with per-case-type word-count targets and hard ceilings.
- **PIPELINE-OPTIONALITY** — Verifier / Refiner / Overseer now OPTIONAL QC layers. Default exit point is after Stage 3 (Drafter).
- **COVER-PAGE DISCIPLINE** — INDEX / SYNOPSIS / LIST OF ANNEXURES each begin on `\newpage` with short cause-title only.
- **Bold-number paragraph convention** — Facts and Grounds paragraphs use `**1.** **2.** **3.**`.
- **Inline-bold highlighting convention** for property descriptors / annexure markers / key terms within Facts narrative.

### Changed

- **Drafter pandoc command** is now TWO steps (pandoc → .docx, then `fix_docx_tables.py`). Step 2 is non-negotiable; skipping it reproduces the v0.2.0 stacking-column defect.

### Cost / token-budget note

Running the full 6-agent pipeline burns approximately 600K tokens per draft, which can exhaust an advocate's Claude session limit. v0.2.1 makes Stages 4–6 OPTIONAL so a baseline Reader → Format → Drafter run (~280K tokens) is sufficient for routine pleadings. The optional QC stages remain available for high-stakes matters.

---

## [0.1.0-alpha] — 2026-05-16 (initial release)

### Added

- **Plugin scaffolding** — `.claude-plugin/plugin.json` manifest · MIT `LICENSE` · `NOTICE.md` provenance and privilege statement · `.gitignore` · this `CHANGELOG.md` · comprehensive `README.md`.
- **Six-agent drafting pipeline** — Reader → Format → Drafter → Verifier → Refiner → Overseer. Each agent is a markdown file under `agents/<name>/<name>.md` with YAML frontmatter declaring `name`, `description`, and `allowed-tools`.
- **Shared infrastructure skills:**
  - `_drafting_common` — anti-pollution rules, encoding standards, language conventions, AI-style-marker blacklist, privacy firewall protocol, citation discipline, and statutory currency rules (Companies Act 2013 — with the *Tata Consultancy Services / Cyrus Investments* line on Section 241-242 scope — IBC 2016 — with the *Innoventive Industries* / *Mobilox Innovations* / *Swiss Ribbons* / *Lalit Kumar Jain* line on admission, pre-existing dispute, constitutional validity and Part III operation — NCLT Rules 2016, NCLAT Rules 2016, IBBI (CIRP for Corporate Persons) Regulations 2016, IBBI (Application to Adjudicating Authority) Rules 2016, IBBI (Liquidation Process) Regulations 2016, Companies (Compromises, Arrangements and Amalgamations) Rules 2016, NCLT (Procedure for Reduction of Share Capital) Rules 2016, Companies (Removal of Names of Companies from the Register of Companies) Rules 2016, Companies (Inspection, Investigation and Inquiry) Rules 2014, Secretarial Standards SS-1 / SS-2 issued by ICSI under Section 118(10) of the Companies Act 2013, Bharatiya Nagarik Suraksha Sanhita 2023, Bharatiya Sakshya Adhiniyam 2023, Limitation Act 1963, CPC 1908, and applicable State Court-Fees Acts).
  - `_company_drafting_base` — universal Indian company-law-tribunal pleading skeleton (Cause Title with correct NCLT bench / NCLAT nomenclature · Parties block · Statutory Opening invoking the operative section of the Companies Act 2013 or the IBC 2016 · Prelude · Facts · Grounds · Prayer · Verification · Affidavit-in-support · Index · List of Documents · accompanying applications).
- **Ten case-type skill scaffolds:**
  - `nclt-section-241-242-petition-draft` — Petition under Sections 241 and 242 of the Companies Act 2013 (oppression and mismanagement; Section 244 threshold for maintainability)
  - `nclt-section-245-class-action-draft` — Application under Section 245 of the Companies Act 2013 (class action by members / depositors)
  - `nclt-scheme-of-arrangement-draft` — Application under Sections 230 to 232 of the Companies Act 2013 (compromise / arrangement / amalgamation / merger / demerger scheme)
  - `nclt-section-66-reduction-of-capital-draft` — Application under Section 66 of the Companies Act 2013 (reduction of share capital with NCLT confirmation)
  - `nclt-section-252-revival-struck-off-company-draft` — Appeal / Application under Section 252 of the Companies Act 2013 (restoration of a company whose name has been struck off the Register of Companies by the Registrar of Companies under Section 248)
  - `nclt-section-213-investigation-draft` — Application under Section 213 of the Companies Act 2013 (NCLT-ordered investigation into the affairs of a company by the Central Government)
  - `ibc-section-9-operational-creditor-application-draft` — Application under Section 9 of the Insolvency and Bankruptcy Code 2016 (Operational Creditor application to initiate Corporate Insolvency Resolution Process before the NCLT)
  - `ibc-section-10-corporate-applicant-draft` — Application under Section 10 of the Insolvency and Bankruptcy Code 2016 (Corporate Applicant / Corporate Debtor self-initiation of CIRP before the NCLT)
  - `nclat-appeal-section-421-draft` — Appeal before the NCLAT under Section 421 of the Companies Act 2013 from an order of the NCLT
  - `nclat-appeal-section-61-ibc-draft` — Appeal before the NCLAT under Section 61 of the IBC 2016 from an order of the NCLT made under the IBC
- **Forum-aware design** — the user supplies `case-config.md` declaring the chosen forum (NCLT bench of competent territorial jurisdiction / NCLAT Principal Bench / NCLAT Chennai Bench), share-capital structure, alleged-act / scheme / capital-arithmetic / strike-off / investigation / operational-debt / self-initiation / appellate-order particulars, and the limitation-clock anchor.

### Notes on this release

This is a **v0.1.0-alpha scaffold release**. The structural skeletons, agent pipeline, base skills, and 10 case-type skill frames are in place. Deep per-skill encoding (full pleading exemplars for each case type, full Section 244 threshold computation per the post-2017-amendment thresholds, full *Tata Consultancy Services* / *Cyrus Investments* / *Mobilox Innovations* / *Innoventive Industries* / *Swiss Ribbons* / *Lalit Kumar Jain* line of Supreme Court precedent encoded in the Verifier, and bench-specific Practice Directions for NCLT Mumbai / Delhi-Principal / Chennai / Kolkata / Bangalore / Ahmedabad / Allahabad / Cuttack / Indore / Amaravati and the NCLAT Principal Bench and NCLAT Chennai Bench) will land in v0.1.0 and onward.

### Released under

MIT License. Authored by Rushikesh R. Mahajan, Advocate, publishing under the wolfgang_rush open-source brand for legal-tech infrastructure.
