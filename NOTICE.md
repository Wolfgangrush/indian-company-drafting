# NOTICE — Provenance and Privilege Statement

This document is part of the public release of the `indian-company-drafting` plugin (v0.1.0-alpha and onwards). It declares the provenance of the plugin's content, in order to address any question about advocate-client privilege, client confidentiality, professional ethics, personal-data protection, and commercial confidentiality that may be raised by any reader, complainant, regulator, or Bar Council disciplinary authority.

The plugin is **case-config-aware**: the universal structural skeleton of any Indian company-law tribunal pleading is uniform, and the parties' chosen forum (NCLT bench of competent territorial jurisdiction, NCLAT Principal Bench or NCLAT Chennai Bench), claim quantum / capital arithmetic, share-capital structure, board composition, alleged-oppression-acts / class-action facts / scheme-terms / capital-reduction-arithmetic / strike-off-history / investigation-grounds / operational-debt-particulars / corporate-applicant-financial-distress / NCLT-order-being-appealed, and limitation-clock anchor are supplied by the user via a `case-config.md` file in the case folder.

This NOTICE is published in plain language so that any reader — practising advocate, judge, Bar Council officer, regulator, member of the public, fellow developer — can understand the position without ambiguity.

---

## 1. What this plugin contains

This plugin contains the following categories of content, and **only** the following categories of content:

(a) **Universal company-law-pleading skeleton** — the structural shape of any Indian company-law tribunal pleading (Cause Title with correct forum nomenclature for NCLT bench / NCLAT, Parties block, Statutory Opening invoking the operative section of the Companies Act 2013 or the Insolvency and Bankruptcy Code 2016, Prelude, Facts, Grounds, Prayer, Verification, Affidavit-in-support, Index, List of Documents, accompanying applications).

(b) **Formatting conventions** — text-formatting conventions for pleadings before the National Company Law Tribunal and the National Company Law Appellate Tribunal at the Principal Bench and the regional / Chennai bench, as discernible from the NCLT Rules 2016, the NCLAT Rules 2016, and the publicly available Practice Directions and Standing Orders issued by the benches from time to time.

(c) **Statutory references** — citations to public statutes (Companies Act 2013, Insolvency and Bankruptcy Code 2016, Companies Act 1956 only for legacy transition discipline, Reserve Bank of India Act 1934, Securities and Exchange Board of India Act 1992, Bharatiya Nagarik Suraksha Sanhita 2023, Bharatiya Sakshya Adhiniyam 2023, Limitation Act 1963, Indian Contract Act 1872, Transfer of Property Act 1882, Indian Stamp Act 1899 + applicable State Stamp Acts, applicable State Court-Fees Acts, Limited Liability Partnership Act 2008, Foreign Exchange Management Act 1999).

(d) **Procedural rule references** — citations to public rules (NCLT Rules 2016, NCLAT Rules 2016, Companies (Compromises, Arrangements and Amalgamations) Rules 2016, NCLT (Procedure for Reduction of Share Capital) Rules 2016, Companies (Removal of Names of Companies from the Register of Companies) Rules 2016, Companies (Inspection, Investigation and Inquiry) Rules 2014, IBBI (Application to Adjudicating Authority) Rules 2016, IBBI (Insolvency Resolution Process for Corporate Persons) Regulations 2016, IBBI (Liquidation Process) Regulations 2016, IBBI (Voluntary Liquidation Process) Regulations 2017, Secretarial Standard 1 (Meetings of the Board of Directors) and Secretarial Standard 2 (General Meetings) issued by the Institute of Company Secretaries of India under Section 118(10) of the Companies Act 2013).

(e) **Generic placeholders** — every variable in every template is a placeholder (`[Petitioner-Shareholder]`, `[Respondent-Company]`, `[Respondent-Director-1]`, `[Corporate Identification Number]`, `[Director-Identification-Number]`, `[Date of Incorporation]`, `[Authorised-Share-Capital]`, `[Paid-Up-Share-Capital]`, `[Shareholding-Percentage]`, `[Alleged-Oppressive-Act-Date]`, `[Scheme-of-Arrangement-Date]`, `[Capital-Reduction-Arithmetic]`, `[Strike-Off-Date]`, `[Operational-Debt-Demand-Notice-Date]`, `[Default-Date]`, `[NCLT-Order-Being-Appealed-Date]`). No placeholder is filled with any specific company, director, shareholder, operational creditor, or any other identifying information.

(f) **Anti-hallucination and privacy-firewall workflow** — six agents (Reader, Format, Drafter, Verifier, Refiner, Overseer) that operate on a case folder supplied by the user. The plugin itself contains no case folder. The Reader substitutes every party name, every company name, every director name, every shareholder name, every Corporate Identification Number, every Director Identification Number, every PAN, every share-certificate number, and every outstanding-debt figure with placeholders before downstream AI processing.

---

## 2. What this plugin does NOT contain

This plugin does **not** contain any of the following, and has never contained any of the following at any point in any committed version:

(a) **No specific client matter or company-law case.** No client of the author, and no specific corporate dispute, scheme of arrangement, oppression petition, class action, investigation, IBC application, or appellate matter handled by the author or any client, appears in the plugin — by name, by Corporate Identification Number, by Director Identification Number, by petitioner-shareholder reference, by scheme particulars, by capital-reduction arithmetic, by operational-debt figure, by NCLT order reference, by NCLAT appeal reference, or by any other identifying signature.

(b) **No client communications.** No oral or written communication made to the author by or on behalf of any client (whether a petitioning shareholder, a respondent company, a director, an operational creditor, a corporate debtor, an Interim Resolution Professional, a Resolution Professional, or any other party) appears in the plugin in any form.

(c) **No client documents.** No document or instrument with which the author has become acquainted in the course of professional employment as an advocate appears in the plugin, in original, in redacted, in summary, in extract, or in pattern. This includes — but is not limited to — Memoranda of Association, Articles of Association, board minutes, general-meeting minutes, statutory registers, audited financial statements, share certificates, share-transfer forms, schemes of arrangement, valuation reports, fairness opinions, secretarial audit reports, demand notices under Section 8 IBC, statutory pre-litigation correspondence, NCLT pleadings, NCLT orders, or NCLAT records of any specific company.

(d) **No personal data of any data principal.** The plugin processes no personal data, collects no personal data, stores no personal data.

(e) **No specific board resolution, no specific shareholders' resolution, no specific power-of-attorney, no specific authorisation letter** of any specific company handled by the author or any other advocate.

(f) **No client list, no panel-counsel list of any company, no chamber list, no associate list, no opposing-counsel list, no Member-specific NCLT intelligence, no NCLAT-Bench-specific intelligence.**

(g) **No tracking, no telemetry, no analytics, no opt-in error reporting, no login, no account, no cloud sync.** The plugin runs entirely on the user's machine. The author receives no information about who installs the plugin, who uses it, on what cases, with what consideration, with what outcomes.

---

## 3. The legal distinction

Indian law has long recognised a clear distinction between two categories:

(i) **Specific client communications and documents** — protected under Section 132 of the Bharatiya Sakshya Adhiniyam 2023 (formerly Section 126 of the Indian Evidence Act 1872) and under Rule 17 of the Bar Council of India Standards of Professional Conduct and Etiquette. This category is privileged and confidential.

(ii) **General professional knowledge of company law, insolvency procedure, and pleading craft** — an advocate's accumulated knowledge of how a Section 241-242 oppression petition is structured, how the Section 244 threshold is pleaded, what *Tata Consultancy Services Ltd. v. Cyrus Investments Pvt. Ltd.* (2021) 9 SCC 449 (limited reference) holds about the scope of Section 241-242 review, what *Mobilox Innovations Pvt. Ltd. v. Kirusa Software Pvt. Ltd.* (2018) 1 SCC 353 holds about pre-existing dispute under Section 9 IBC, what *Innoventive Industries Ltd. v. ICICI Bank* (2018) 1 SCC 407 holds about the admission framework under IBC, what *Swiss Ribbons Pvt. Ltd. v. Union of India* (2019) 4 SCC 17 holds about the constitutional validity of the IBC, what the Secretarial Standards SS-1 and SS-2 prescribe about Board and General-Meeting proceedings, how a Section 230 scheme of arrangement is presented before the NCLT, how a Section 66 capital-reduction confirmation is computed and pleaded. This category is the advocate's own professional knowledge. It is not the property of any specific client. It is not privileged.

This plugin operates **entirely within category (ii)**.

Every Indian advocate accumulates this knowledge through years of practice, through study of A. Ramaiya's *Guide to the Companies Act*, Sumant Batra on the *Corporate Insolvency Practice*, Subodh Kumar Jain's *IBC Handbook*, the IBBI handbooks, the various ICSI publications on Secretarial Standards and corporate governance, and the case-law of the Supreme Court, the High Courts, the NCLAT and the NCLT on oppression, mismanagement, schemes, capital reduction, strike-off revival, investigation, and the operation of the IBC. The plugin codifies that accumulated procedural knowledge into machine-readable form. It does not codify any client's confidential information.

The plugin is, in this respect, identical in legal character to a published company-law textbook, a continuing legal education handout, or a senior advocate's drafting-style lecture. The medium is software. The content is procedural knowledge.

---

## 4. The author's professional position

The author is **Rushikesh R. Mahajan**, Advocate, enrolled with the Bar Council of Maharashtra and Goa, practising before the High Courts of India. The plugin is published under the open-source brand **wolfgang_rush**, which is the author's publishing handle for legal-technology infrastructure; the real-identity accountability declared in this section attaches to the author personally and is not displaced by the use of a publishing handle.

The author retains full enrolment, full responsibility, and full liability under the Advocates Act 1961, the Bar Council of India Rules, and the Standards of Professional Conduct and Etiquette.

The plugin is published as a personal contribution to the open-source legal-technology ecosystem. It is published without any commercial channel, without any solicitation of professional work, without any advertisement of professional services, and without any acceptance of work through this repository.

This NOTICE is filed of record in this open-source repository so that any person who reads, reviews, audits, or assesses this plugin has full transparency about its provenance and its scope from the moment of release.

---

## 5. Verification of clean provenance

The repository owner shall maintain, on a private offline record, a build log demonstrating that every line of every file in the plugin was either:

(a) authored from scratch as procedural skeleton, OR
(b) derived from public statute, public rule, public judgment, or public company-law / insolvency-law textbook, OR
(c) derived from the author's own original procedural knowledge as a practitioner.

No line of any file was, at any stage, copied from, paraphrased from, summarised from, or pattern-matched against any specific client matter, corporate dispute, scheme of arrangement, oppression petition, class action, investigation, IBC application, or appellate proceeding, nor against any client communication, nor against any client document.

This NOTICE is the author's signed declaration of that position.

---

## 6. Reporting concerns

If any reader, regulator, fellow advocate, or member of the public believes any specific content in this plugin is derived from a specific client matter or specific confidential communication, the reader is requested to:

(a) identify the file and line at issue in the plugin,
(b) identify the specific client matter or communication believed to be the source,
(c) explain the basis of the belief,

and raise the concern via a GitHub Issue on this repository.

Concerns raised with these particulars will be investigated and the file or line will be removed or rewritten if the concern is well-founded. Concerns raised without these particulars cannot be acted upon.

---

## 7. Standing instruction to forks and derivatives

Any fork, derivative, downstream redistribution, or commercial integration of this plugin or its content shall preserve this NOTICE in unmodified form, and shall extend the same provenance discipline to any content added in the fork or derivative.

This NOTICE travels with the code under the same MIT licence that governs the source.

---

*Signed and dated by way of public commit history on the repository. The author stands by every line of this notice.*
