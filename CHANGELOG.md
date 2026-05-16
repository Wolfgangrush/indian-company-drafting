# Changelog

All notable changes to the `indian-company-drafting` plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/) and this project adheres to [Semantic Versioning](https://semver.org/).

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

MIT License. Authored by Rushikesh R. Mahajan, Advocate, publishing under the Wolfgang Rush open-source brand for legal-tech infrastructure.
