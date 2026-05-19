# indian-company-drafting

> **Open-source Claude-compatible plugin for drafting Indian company-law tribunal litigation pleadings — before the National Company Law Tribunal (NCLT) and the National Company Law Appellate Tribunal (NCLAT).**
>
> Six-agent drafting pipeline · ten case-type skills · case-config-aware · Companies Act 2013 + IBC 2016 + NCLT Rules 2016 + NCLAT Rules 2016 + IBBI Regulations + Secretarial Standards SS-1 / SS-2 + Bharatiya Nagarik Suraksha Sanhita 2023 + Bharatiya Sakshya Adhiniyam 2023 discipline encoded. Companies Act 1956 → 2013 legacy-citation transition discipline enforced by the Verifier.
>
> Released under MIT. Open infrastructure for the legal community. No commercial engagement offered through this repository — see Disclaimer below.

---

## Table of contents

1. [What this plugin does](#what-this-plugin-does)
2. [Case-type skills (full inventory with statutory authority)](#case-type-skills-full-inventory-with-statutory-authority)
3. [The 6-agent drafting pipeline (what each agent does)](#the-6-agent-drafting-pipeline)
4. [Installation](#installation) — Claude Desktop application
5. [Your first pleading — step-by-step walkthrough](#your-first-pleading--step-by-step-walkthrough)
6. [The `case-config.md` file](#the-case-configmd-file)
7. [Built-in compliance disciplines](#built-in-compliance-disciplines)
8. [Privacy firewall — extra discipline for company-law content](#privacy-firewall--extra-discipline-for-company-law-content)
9. [Why MIT License](#why-mit-license)
10. [Sibling plugins](#sibling-plugins)
11. [Why this exists](#why-this-exists)
12. [Roadmap](#roadmap)
13. [Contributing](#contributing)
14. [Contact](#contact)
15. [Author and brand](#author-and-brand)
16. [Provenance and privilege statement](#provenance-and-privilege-statement)
17. [Disclaimer and Bar Council of India Rule 36 compliance](#disclaimer-and-bar-council-of-india-rule-36-compliance)
18. [License](#license)

---

## What this plugin does

This plugin lets an Indian advocate, sitting inside the Claude Desktop application, point at a case folder on disk and obtain a complete company-law tribunal pleading in `.docx` form — Cause Title in the correct NCLT-bench / NCLAT nomenclature, Parties block, Statutory Opening invoking the operative section of the Companies Act 2013 or the IBC 2016, Prelude, Facts, Grounds, Prayer, Verification, Affidavit-in-support, Index, List of Documents, and the accompanying applications (ad-interim ex-parte injunction restraining alteration of share capital / dilution / board reconstitution pending the Section 241-242 petition; application for waiver of Section 244 threshold; ad-interim moratorium under Section 14 IBC; urgent listing; substituted service; application for waiver of pre-deposit before NCLAT where statutorily applicable; condonation of delay under Section 421(3) Companies Act 2013 or Section 61(2) IBC) — formatted in the tribunal's idiom and the case-type-specific structure, sourced from a `case-config.md` file the user places in the case folder.

The pipeline is six agents running in sequence:

1. **Reader** — extracts company-law case-facts (parties, Corporate Identification Number, Director Identification Numbers, share-capital structure, board composition, alleged oppression / mismanagement acts or scheme terms or capital-reduction arithmetic or strike-off history or investigation grounds or operational-debt particulars or corporate-applicant financial distress or NCLT-order being appealed) from the case folder with a per-document audit log, and applies the **company-law privacy firewall** (every party name, every company name, every director name, every shareholder name, every CIN / DIN / PAN / share-certificate number, and every outstanding-debt figure substituted with structural placeholders before downstream AI processing; the placeholder → real-value mapping is stored locally on the user's machine only).
2. **Format** — loads the case-type skill template, reads the user's `case-config.md`, and pre-substitutes NCLT bench / NCLAT bench / case-number prefix / filing fee / statutory opening / limitation anchor into a `format-shell.md` ready for the Drafter.
3. **Drafter** — writes the actual pleading. Cause Title in the correct forum nomenclature, Parties block, Statutory Opening invoking the operative section of the Companies Act 2013 or the IBC 2016, Prelude, Facts as numbered narrative paragraphs with inline exhibit markers, Grounds with statutory anchors and document anchors, Prayer with case-type-specific reliefs, Verification, Affidavit-in-support, Index, List of Documents, and accompanying applications.
4. **Verifier** — anti-hallucination firewall **plus** statutory-currency check (CrPC 1973 → BNSS 2023 transitions; IEA 1872 → BSA 2023 transitions; **Companies Act 1956 → 2013 transitions** for any legacy citation) **plus** NCLT / NCLAT forum-jurisdictional check **plus** Section 244 threshold check for Section 241-242 petitions **plus** Section 230(2) — 230(11) disclosure-discipline check for scheme applications **plus** Section 66(3) / Section 66(4) creditor-protection check for capital-reduction applications **plus** Section 252 limitation check (3 years from strike-off date for the company / 20 years for any person aggrieved) **plus** Section 213 maintainability check (members-by-number-and-value threshold) **plus** Section 8 IBC demand-notice ingredient check for Section 9 application **plus** Section 10(3) board-resolution + special-resolution ingredient check for Section 10 corporate-applicant **plus** Section 421(3) Companies Act / Section 61(2) IBC limitation-period check for NCLAT appeals **plus** SS-1 (Board Meetings) / SS-2 (General Meetings) compliance check **plus** CIRP timeline check (Section 12 IBC — 180 days extendable to 330 days) **plus** *Mobilox Innovations* pre-existing-dispute check for Section 9 IBC **plus** *Innoventive Industries* admission framework check for Section 7 / 9 / 10 IBC.
5. **Refiner** — applies Verifier flags, polishes language to the formal NCLT / NCLAT register, enforces internal numbering and exhibit-cross-reference consistency, strips AI-style markers, and re-substitutes real party names, real CIN / DIN, real share-capital figures, real scheme particulars, and real authorised-signatory names into the final `.docx` (strictly on the user's local machine — the underlying AI never holds real values).
6. **Overseer** — reads the polished draft with an opposing-counsel lens (Respondent Company / promoter's counsel for a Section 241-242 petition; Bank's / Financial Creditor's counsel for a Section 9 Operational Creditor application; objector-shareholders' counsel for a Section 230-232 scheme; defending-Director's counsel for a Section 213 investigation; ROC's counsel for a Section 252 revival; Corporate-Debtor / Resolution-Professional counsel for any IBC dispute; opposing party's counsel for any NCLAT appeal). Flags attackable Section 244 maintainability defects, Section 8 IBC demand-notice ingredient gaps, *Mobilox Innovations* pre-existing-dispute weakness, scheme-disclosure defects under Section 230, capital-reduction-arithmetic errors, broken Section 252 limitation, broken Section 421 / Section 61 limitation, internal contradictions, Innoventive moratorium gaps, SS-1 / SS-2 procedural defects in the board / general-meeting record relied on, Section 241(2) public-interest standing defects, and Section 245 class-action threshold defects.

The output is what an advocate would put before the NCLT or the NCLAT for filing — **not a template. Not a checklist. A pleading** — ready for the advocate's review, professional verification, signature, filing fee, and filing.

---

## Case-type skills (full inventory with statutory authority)

The plugin ships with ten case-type skills, each covering a distinct company-law-tribunal case-type:

### 1. `nclt-section-241-242-petition-draft`

**Statutory authority:** Companies Act 2013 — Section 241 (application by member / Central Government on oppression and mismanagement) + Section 242 (powers of the Tribunal — wide-ranging reliefs including winding-up regulation, share-purchase orders, supersession of board, removal of director, regulation of conduct of the company) + Section 244 (eligibility threshold for members — not less than 100 members or one-tenth of total number of members or one-tenth of the issued share capital, whichever is less — with the waiver power under Section 244(1) proviso); NCLT Rules 2016, Rules 81 — 89 (procedure for oppression and mismanagement applications); *Tata Consultancy Services Ltd. v. Cyrus Investments Pvt. Ltd.* (2021) 9 SCC 449 (limited reference on Section 241-242 review scope). **Use case:** a shareholder, a group of shareholders, or the Central Government seeking relief against oppression of any member or against the conduct of the affairs of the company in a manner prejudicial to public interest or in a manner prejudicial or oppressive to any member, before the NCLT bench of the company's registered office. **Output:** complete Company Petition with Cause Title in NCLT nomenclature, Section 241 + 242 statutory opening, full Facts paragraphs anchored to alleged-oppressive-act dates and documentary exhibits (Articles of Association, board minutes, general-meeting minutes, share-transfer records, financial statements), Grounds with Section 241 / 242 anchors and Section 244 maintainability pleadings, Prayer for the suite of Section 242 reliefs, accompanying I.A. for waiver of Section 244 threshold (where applicable) and ad-interim injunction restraining the Respondents from altering share capital / reconstituting the Board / declaring dividends pending the Petition.

### 2. `nclt-section-245-class-action-draft`

**Statutory authority:** Companies Act 2013 — Section 245 (class action by members or depositors — not less than 100 members / depositors or members / depositors holding 1/10th of the issued share capital / deposits, whichever is less — with Section 245(4) factors the NCLT must consider before admission); NCLT Rules 2016, Rule 84 (procedure for class action). **Use case:** a class of members or depositors of a company alleging that the management or conduct of the affairs of the company is being conducted in a manner prejudicial to the interests of the company or its members or depositors, and seeking class-relief restraining the company from ultra vires acts, declaring a resolution void, restraining a fraudulent or unlawful act by directors, claiming damages or compensation, or other suitable Section 245 reliefs. **Output:** complete Class Action Application with Section 245 + 245(4) statutory openings, Cause Title in NCLT nomenclature, full Facts paragraphs anchored to the alleged-class-injury, threshold compliance pleaded with particularity, advertisement / public-notice particulars for the class, Prayer for the suite of Section 245 reliefs, accompanying I.A. for advertisement directions and ad-interim relief pending hearing.

### 3. `nclt-scheme-of-arrangement-draft`

**Statutory authority:** Companies Act 2013 — Section 230 (power to compromise or make arrangements with creditors and members — disclosure obligations under Section 230(2); valuation report under Section 230(3); meeting-convening application under Section 230(1); statutory authority over schemes that include reduction of share capital or buy-back; cross-border merger under Section 234) + Section 231 (power of Tribunal to enforce compromise or arrangement) + Section 232 (merger and amalgamation of companies — fast-track merger under Section 233 where applicable); Companies (Compromises, Arrangements and Amalgamations) Rules 2016; NCLT Rules 2016. **Use case:** a company, or any creditor or member, applying to the NCLT for sanction of a scheme of compromise / arrangement / merger / amalgamation / demerger / share-buy-back / debt-restructuring, with the statutorily required disclosures (valuation report; effect on key managerial personnel; promoters' shareholding pre and post; auditor's certificate; SEBI / RBI / CCI / Income-tax / regional director / official liquidator no-objection where applicable). **Output:** complete First-Motion Application with prayer for direction to convene meetings of creditors / members in classes under Section 230(1), followed by Second-Motion Petition with prayer for sanction of the scheme under Section 230(6) / Section 232(3); Cause Title in NCLT nomenclature; full Facts paragraphs anchored to all statutorily required disclosures; accompanying I.A.s for newspaper-publication directions, dispensation of meeting (where 90% creditor consent is obtained as per the Rules), and any urgent listing.

### 4. `nclt-section-66-reduction-of-capital-draft`

**Statutory authority:** Companies Act 2013 — Section 66 (reduction of share capital with confirmation of the NCLT — Section 66(1) requirements, Section 66(2) creditor-protection regime, Section 66(3) special-resolution pre-condition, Section 66(4) public-notice and creditor-objection regime, Section 66(5) order-of-confirmation, Section 66(6) registration with ROC); NCLT (Procedure for Reduction of Share Capital of Company) Rules 2016. **Use case:** a company resolving to reduce its share capital by extinguishing or reducing the liability on shares, by cancelling paid-up capital lost or unrepresented by available assets, or by paying off paid-up capital in excess of the company's needs, and seeking the NCLT's confirmation of the reduction. **Output:** complete Reduction Application with prayer for NCLT confirmation of the special resolution reducing capital; Cause Title in NCLT nomenclature; full Facts paragraphs anchored to the special-resolution date, the auditor's certificate on creditor-position, the list of creditors with consents / unsatisfied creditors with proposed settlement, the proposed amended Memorandum of Association reflecting the reduced capital; Grounds anchoring each Section 66 sub-clause and confirming Section 66(3) and Section 66(4) compliance; accompanying I.A.s for newspaper publication, creditor-objection directions, and final-order publication.

### 5. `nclt-section-252-revival-struck-off-company-draft`

**Statutory authority:** Companies Act 2013 — Section 252 (appeal against striking off / application for restoration of the company name — Section 252(1) appeal by the company within 3 years from the order of strike-off; Section 252(3) application by the company, member, creditor, or workman within 20 years of the publication of the strike-off notice if the company was carrying on business or in operation when its name was struck off); Companies (Removal of Names of Companies from the Register of Companies) Rules 2016 (Rule 11 for restoration). **Use case:** a company (or a member, creditor, or workman of a company) whose name has been struck off the Register of Companies under Section 248 by the Registrar of Companies, seeking restoration of the name to the Register on the ground that the company was carrying on business or in operation, or on any other ground that the NCLT considers just to restore the company. **Output:** complete Section 252 Appeal / Application with Cause Title naming the Registrar of Companies as the Respondent and the company (or member / creditor / workman) as the Petitioner; statutory opening invoking Section 252(1) for the 3-year appeal or Section 252(3) for the 20-year application; full Facts paragraphs anchored to date of strike-off, evidence of operation (bank statements, GST returns, income-tax returns, statutory filings under Section 92 / 137 of the Companies Act 2013), and prayer for restoration with consequential directions for filing of overdue returns and payment of statutory dues.

### 6. `nclt-section-213-investigation-draft`

**Statutory authority:** Companies Act 2013 — Section 213 (NCLT-ordered investigation into the affairs of a company by the Central Government — Section 213(a) on application of not less than 100 members or members holding not less than 1/10th of the total voting power; Section 213(b) on application of any person on the ground that the affairs of the company are conducted with intent to defraud creditors / members / others or for any fraudulent or unlawful purpose, or that the management has been guilty of fraud / misfeasance / other misconduct or that the members have not been given all the information regarding the affairs of the company); Companies (Inspection, Investigation and Inquiry) Rules 2014. **Use case:** members of a company (or any other person with locus on fraud / misfeasance grounds) seeking an NCLT order directing the Central Government to investigate the affairs of the company. **Output:** complete Application under Section 213 with Cause Title in NCLT nomenclature; threshold compliance pleaded with particularity (member-count / voting-power compliance under Section 213(a); fraud / misfeasance grounds with prima-facie material under Section 213(b)); Facts anchored to the alleged-fraud / misconduct / mismanagement particulars; Prayer for direction to the Central Government to investigate; accompanying I.A. for interim directions preserving books of account / records / documents pending the investigation.

### 7. `ibc-section-9-operational-creditor-application-draft`

**Statutory authority:** Insolvency and Bankruptcy Code 2016 — Section 8 (demand notice / invoice in Form 3 / 4 issued by Operational Creditor; 10-day reply window for Corporate Debtor — pre-existing-dispute discipline anchored on *Mobilox Innovations Pvt. Ltd. v. Kirusa Software Pvt. Ltd.* (2018) 1 SCC 353) + Section 9 (Operational Creditor application to NCLT after expiry of the 10-day notice period without payment or dispute) + Section 4 default threshold (currently ₹1 crore) + Section 5(20) Operational-Creditor definition + Section 5(21) Operational-Debt definition + Section 14 moratorium + Section 60(1) territorial jurisdiction + Section 10A suspension-window check; IBBI (Application to Adjudicating Authority) Rules 2016 (Form 5 filing discipline); *Innoventive Industries v. ICICI Bank* (2018) 1 SCC 407 (admission framework adapted to Section 9); *Swiss Ribbons Pvt. Ltd. v. Union of India* (2019) 4 SCC 17 (IBC constitutional validity). **Use case:** Operational Creditor (typically a supplier, employee, contractor, government-statutory-due creditor, or service provider) initiating CIRP against a Corporate Debtor before the NCLT for default of an operational debt of ≥ ₹1 crore, after due compliance with the Section 8 demand-notice regime. **Output:** complete Section 9 Application with Form 5 structure (Particulars of Operational Creditor, Corporate Debtor, Proposed IRP — optional for Section 9 unlike Section 7), Operational-Debt particulars with invoices and demand-notice service trail, Mobilox-style pre-existing-dispute pre-emption (positively pleading no notice of dispute received in the 10-day window), prayer for admission + moratorium + appointment of IRP (or direction to IBBI to nominate one, where the applicant has not proposed an IRP).

### 8. `ibc-section-10-corporate-applicant-draft`

**Statutory authority:** Insolvency and Bankruptcy Code 2016 — Section 10 (Corporate Applicant / Corporate Debtor self-initiation of CIRP — Section 10(1) authorisation by special resolution of members under Section 10(3)(c) or by 3/4ths of partners under the LLP Act; Section 10(3)(a) particulars of debt and default; Section 10(3)(b) proposed IRP with written consent; Section 10(4) admission framework; Section 11 ineligibility regime for repeat applicants and applicants in ongoing CIRP) + Section 4 default threshold (currently ₹1 crore) + Section 14 moratorium; IBBI (Application to Adjudicating Authority) Rules 2016 (Form 6 filing discipline); *Swiss Ribbons* constitutional validity; *Innoventive Industries* admission framework adapted to Section 10. **Use case:** a Corporate Debtor itself initiating CIRP against itself before the NCLT, supported by the special resolution of the members or the requisite resolution of partners, the proposed IRP's consent in Form 2, and the financial-debt / operational-debt schedule. **Output:** complete Section 10 Application with Form 6 structure; statutory opening invoking Section 10 read with the IBBI (AAA) Rules; Facts anchored to the date of default, the special resolution under Section 10(3)(c), the proposed IRP's Form 2 consent, the books of accounts and audited financial statements, the list of creditors with debt-particulars; Section 11 ineligibility pre-emption (positively pleading that none of the Section 11 disqualifications apply); prayer for admission + moratorium + appointment of proposed IRP.

### 9. `nclat-appeal-section-421-draft`

**Statutory authority:** Companies Act 2013 — Section 421 (appeal to NCLAT from any order of the NCLT — Section 421(1) right of appeal by any person aggrieved; Section 421(3) limitation of 45 days from the date of receipt of the NCLT order, extendable up to another 45 days on sufficient cause; Section 421(4) NCLAT may pass such orders thereon as it thinks fit, confirming, modifying or setting aside the order appealed against) + Section 422 (expeditious disposal — endeavour to dispose within 6 months); NCLAT Rules 2016 (the NCLAT (Practice) Rules and the Standing Orders of the NCLAT Principal Bench and Chennai Bench). **Use case:** any party aggrieved by an order of the NCLT in a non-IBC matter (oppression and mismanagement under Sections 241-242, scheme under Sections 230-232, reduction under Section 66, restoration under Section 252, investigation under Section 213, class action under Section 245, or any other matter under the Companies Act 2013), preferring an appeal to the NCLAT. **Output:** complete Memorandum of Appeal with Cause Title in NCLAT nomenclature; statutory opening invoking Section 421; full Grounds (erroneous appreciation of evidence / misapplication of law / procedural irregularity / jurisdictional defect / breach of natural justice / failure to consider material / perversity); Prayer for setting aside / modifying the NCLT order with consequential directions; accompanying I.A.s for stay of operation of the NCLT order, condonation of delay under the Section 421(3) proviso (where applicable), and urgent listing.

### 10. `nclat-appeal-section-61-ibc-draft`

**Statutory authority:** Insolvency and Bankruptcy Code 2016 — Section 61 (appeal to NCLAT from an order of the NCLT made under the IBC — Section 61(1) right of appeal; Section 61(2) limitation of 30 days from the date of the order, extendable by 15 days on sufficient cause; Section 61(3) grounds for an appeal against an order of admission of an application under Section 7 / 9 / 10 — material irregularity, fraud, default threshold not satisfied, debt not due / payable; Section 61(4) grounds for an appeal against an order approving a resolution plan; Section 62 further appeal to the Supreme Court only on substantial questions of law) + Section 64 expeditious disposal (endeavour to dispose within 30 days, extendable on sufficient cause to 90 days); NCLAT Rules 2016; *Innoventive Industries v. ICICI Bank* (2018) 1 SCC 407 (admission framework as the lens for Section 61 review). **Use case:** any party aggrieved by an order of the NCLT in an IBC matter (admission / rejection of Section 7 / 9 / 10 application, approval of resolution plan under Section 31, liquidation order under Section 33, distribution of liquidation proceeds, IRP / RP / liquidator-related disputes, voidable-transaction orders under Sections 43 — 51, avoidance-transaction orders, personal-guarantor IRP orders under Section 100), preferring an appeal to the NCLAT. **Output:** complete Memorandum of Appeal with Cause Title in NCLAT nomenclature; statutory opening invoking Section 61; Grounds anchored to the Section 61(3) / Section 61(4) heads (as applicable); Prayer for setting aside / modifying the NCLT order with consequential directions; accompanying I.A.s for stay of operation of the NCLT order, condonation of delay under Section 61(2) proviso (where applicable), and urgent listing.

### Shared infrastructure skills

- **`_drafting_common`** — anti-pollution rules, privacy firewall, AI-style-marker blacklist, citation discipline, **statutory currency rules** (CrPC → BNSS / IEA → BSA / **Companies Act 1956 → Companies Act 2013** transitions), **IBC default-threshold rules**, **Section 8 IBC demand-notice ingredient discipline (Mobilox-style)**, **Innoventive admission framework**, **NCLAT pre-deposit discipline (where statutorily applicable)**, **Limitation Act 1963 Article map plus the special limitation regimes under Section 421(3) Companies Act and Section 61(2) IBC**, **Vidya Drolia non-arbitrability framework as applied to oppression / mismanagement / Section 9 IBC matters**.
- **`_company_drafting_base`** — universal Indian company-law-tribunal pleading skeleton (Cause Title in correct NCLT-bench / NCLAT nomenclature, Parties block, Statutory Opening, Prelude, Facts, Grounds, Prayer, Verification, Affidavit-in-support, Index, List of Documents, accompanying applications) — company-law-flavoured, with Section 244 threshold paragraphs, SS-1 / SS-2 compliance paragraphs, CIRP-timeline paragraphs, and the Companies Act 1956 → 2013 legacy-citation transition discipline built in.

---

## The 6-agent drafting pipeline

| Agent | What it reads | What it writes | Key company-law-domain specialisation |
|---|---|---|---|
| **`reader`** | Every file in the case folder + the case-type skill's expected exhibits list | `case-facts.md` with per-document audit log + privacy-firewalled placeholder mapping in the header | Privacy firewall — substitutes party names + company names + director names + shareholder names + CIN + DIN + PAN + share-certificate numbers + outstanding-debt figures before downstream AI processing; mapping stored locally only |
| **`format`** | `case-facts.md` + `case-config.md` + case-type SKILL.md + `_company_drafting_base` | `format-shell.md` with NCLT bench / NCLAT bench / case-number-prefix / filing-fee / statutory-opening / limitation-anchor pre-substituted | Resolves NCLT-Mumbai vs NCLT-Delhi-Principal vs NCLT-Chennai vs other regional bench nomenclature; resolves NCLAT Principal Bench vs NCLAT Chennai Bench |
| **`drafter`** | `case-facts.md` + `format-shell.md` + case-type SKILL.md + `_company_drafting_base` + law PDFs | `draft-v1.md` + `draft-v1.docx` | Writes Cause Title + Parties + Statutory Opening invoking the relevant section of the Companies Act 2013 or IBC 2016 + Prelude + Facts (with inline exhibit markers) + Grounds + Prayer + Verification + Affidavit + Index + List of Documents + accompanying applications |
| **`verifier`** | `draft-v1.md` + `case-facts.md` + `case-config.md` + law PDFs | `verification-report.md` | Anti-hallucination + statutory-currency (CrPC → BNSS / IEA → BSA / **Companies Act 1956 → 2013**) + NCLT / NCLAT forum jurisdiction + Section 244 threshold + Section 230 scheme-disclosure + Section 66 creditor-protection + Section 252 limitation + Section 213 maintainability + Section 8 IBC demand-notice ingredient (Mobilox) + Section 10(3) board-resolution-special-resolution ingredient + Section 421(3) / Section 61(2) limitation + SS-1 / SS-2 compliance + CIRP timeline + *Innoventive* admission framework |
| **`refiner`** | `draft-v1.md` + `verification-report.md` + `case-config.md` + `case-facts.md` | `draft-v2.md` + `draft-v2.docx` | Polish to NCLT / NCLAT formal register + internal numbering / cross-reference / exhibit-marker consistency + privacy-firewall reversal (real values re-substituted from local mapping into final `.docx`) |
| **`overseer`** | `draft-v2.md` + `case-facts.md` + `case-config.md` | `opposing-notes.md` + `final-draft.docx` | Opposing-counsel critique — Section 244 maintainability defects, Section 8 IBC demand-notice gaps, *Mobilox* pre-existing-dispute weaknesses, scheme-disclosure defects under Section 230, capital-reduction-arithmetic errors, broken Section 252 / Section 421 / Section 61 limitation, Innoventive moratorium gaps, SS-1 / SS-2 procedural defects |

---

## Installation

This is a Claude-compatible plugin in the Anthropic plugin format, designed to run inside the **Claude Desktop application** (available at <https://claude.ai/download>). The plugin folder location depends on your OS:

| OS | Plugin folder path |
|---|---|
| **macOS** | `~/Library/Application Support/Claude/plugins/` |
| **Windows** | `%APPDATA%\Claude\plugins\` (typically `C:\Users\<you>\AppData\Roaming\Claude\plugins\`) |
| **Linux** | `~/.config/Claude/plugins/` |

Clone the plugin into that folder:

```bash
# macOS / Linux
mkdir -p ~/Library/Application\ Support/Claude/plugins   # adjust per OS table
cd ~/Library/Application\ Support/Claude/plugins
git clone https://github.com/Wolfgangrush/indian-company-drafting.git indian-company-drafting

# Windows (PowerShell)
mkdir -Force $env:APPDATA\Claude\plugins
cd $env:APPDATA\Claude\plugins
git clone https://github.com/Wolfgangrush/indian-company-drafting.git indian-company-drafting
```

Restart the Claude Desktop application. The plugin is auto-discovered on the next session start.

### Anthropic Plugin Marketplace (when available)

When the plugin lands on the Anthropic Plugin Marketplace, you will be able to install it from inside the application's plugin browser without `git`. Until then, the manual clone steps above are canonical.

### Verifying the install

In a Claude session, type:

- *"draft Section 241 petition"* / *"draft oppression and mismanagement petition"* — triggers `nclt-section-241-242-petition-draft`
- *"draft class action"* / *"draft Section 245 application"* — triggers `nclt-section-245-class-action-draft`
- *"draft scheme of arrangement"* / *"draft Section 230 scheme"* / *"draft merger application"* — triggers `nclt-scheme-of-arrangement-draft`
- *"draft Section 66 reduction"* / *"draft capital reduction petition"* — triggers `nclt-section-66-reduction-of-capital-draft`
- *"draft Section 252 revival"* / *"draft restoration of company"* — triggers `nclt-section-252-revival-struck-off-company-draft`
- *"draft Section 213 investigation"* / *"draft NCLT investigation application"* — triggers `nclt-section-213-investigation-draft`
- *"draft Section 9 IBC"* / *"draft operational creditor application"* — triggers `ibc-section-9-operational-creditor-application-draft`
- *"draft Section 10 IBC"* / *"draft corporate applicant"* — triggers `ibc-section-10-corporate-applicant-draft`
- *"draft NCLAT appeal Companies Act"* / *"draft Section 421 appeal"* — triggers `nclat-appeal-section-421-draft`
- *"draft NCLAT appeal IBC"* / *"draft Section 61 appeal"* — triggers `nclat-appeal-section-61-ibc-draft`

---

## Your first pleading — step-by-step walkthrough

Suppose you wish to draft a **Section 241-242 Oppression and Mismanagement Petition** on behalf of a minority shareholder against the controlling shareholders and the company.

### Step 1 — create a case folder

```
~/Desktop/cases/
└── nclt-241-2026-MINORITY-OPPRESSION/
    ├── case-config.md           ← declares NCLT bench + Section 244 threshold compliance + parties
    ├── inputs/
    │   ├── memorandum-of-association.pdf
    │   ├── articles-of-association.pdf
    │   ├── annual-returns-mgt-7-recent.pdf
    │   ├── financial-statements-3-years.pdf
    │   ├── board-minutes-impugned-period.pdf
    │   ├── general-meeting-minutes-impugned-period.pdf
    │   ├── share-transfer-records.pdf
    │   ├── statutory-register-of-members.pdf
    │   ├── alleged-oppressive-resolutions.pdf
    │   ├── correspondence-petitioner-to-board.pdf
    │   └── board-resolution-of-petitioner-shareholder-where-corporate.pdf
    └── laws/
        ├── companies-act-2013.pdf
        ├── nclt-rules-2016.pdf
        ├── secretarial-standards-ss1-ss2.pdf
        └── limitation-act-1963.pdf
```

### Step 2 — write `case-config.md`

```yaml
forum: "National Company Law Tribunal, Mumbai Bench"
case_type: "nclt-section-241-242-petition"
case_number_year: 2026
respondent_company_cin: "[CIN-Placeholder]"
authorised_share_capital_rupees: 100000000        # ₹10 crore
paid_up_share_capital_rupees: 50000000            # ₹5 crore
petitioner_shareholding_percentage: 14            # 14% — above 1/10th threshold under Section 244
section_244_threshold_basis: "ten_percent_of_issued_share_capital"
section_244_compliance: "satisfied"
limitation_anchor_date: "[Alleged-First-Oppressive-Act-Date]"
limitation_filing_date: "[Date-of-Filing-Placeholder]"
parties:
  - role: "Petitioner"
    party_type: "Shareholder holding 14% of issued share capital"
    party_name: "[Petitioner-Shareholder-Placeholder]"
  - role: "Respondent No. 1"
    party_type: "Respondent Company"
    party_name: "[Respondent-Company-Placeholder]"
  - role: "Respondent No. 2"
    party_type: "Controlling Shareholder / Director"
    party_name: "[Respondent-Director-1-Placeholder]"
  - role: "Respondent No. 3"
    party_type: "Director"
    party_name: "[Respondent-Director-2-Placeholder]"
```

### Step 3 — invoke the plugin

Open Claude Desktop, navigate to the case folder, and type:

> *draft Section 241 petition*

The pipeline runs:

1. **Reader** reads every PDF in `inputs/`, builds `case-facts.md` with privacy-firewalled placeholder mapping, validates that all required statutes are in `laws/`.
2. **Format** loads the `nclt-section-241-242-petition-draft` skill, reads `case-config.md`, builds `format-shell.md`.
3. **Drafter** writes `draft-v1.md` and `draft-v1.docx`.
4. **Verifier** reads `draft-v1.md` against `case-facts.md`, writes `verification-report.md`.
5. **Refiner** applies the verification flags, polishes the prose, re-substitutes real values, writes `draft-v2.docx`.
6. **Overseer** reads `draft-v2.docx` with the Respondent's-counsel lens, writes `opposing-notes.md` and `final-draft.docx`.

The advocate now reviews `final-draft.docx` against `opposing-notes.md`, makes professional adjustments, applies filing fee, signs the verification, swears the affidavit, and files.

---

## The `case-config.md` file

This file declares all forum-level / case-type-level / matter-level constants that the plugin substitutes into the format shell. Keep it on the user's local machine — `.gitignore` excludes it from any git repo.

Minimum fields:

- `forum` — exact name of the NCLT bench / NCLAT bench (e.g. *"National Company Law Tribunal, Mumbai Bench"* / *"National Company Law Tribunal, Principal Bench, New Delhi"* / *"National Company Law Appellate Tribunal, Principal Bench, New Delhi"* / *"National Company Law Appellate Tribunal, Chennai Bench"*)
- `case_type` — one of the ten supported case types
- `case_number_year` — current year for case-number placeholder
- `respondent_company_cin` — CIN placeholder of the company concerned
- `authorised_share_capital_rupees` / `paid_up_share_capital_rupees` — share-capital figures for Section 244 threshold calculation (Section 241-242 / Section 245 / Section 213) and for Section 66 reduction arithmetic
- `petitioner_shareholding_percentage` + `section_244_threshold_basis` + `section_244_compliance` — for Section 241-242 / Section 245 / Section 213 maintainability
- `scheme_terms_summary` + `valuation_report_reference` + `auditor_certificate_reference` — for Section 230-232 scheme
- `capital_reduction_method` + `auditor_certificate_on_creditors` — for Section 66 reduction
- `strike_off_order_date` + `evidence_of_operation_summary` — for Section 252 revival
- `investigation_grounds_summary` + `prima_facie_material_reference` — for Section 213
- `operational_debt_quantum_rupees` + `section_8_demand_notice_date` + `notice_reply_received` (yes / no) — for Section 9 IBC
- `corporate_applicant_special_resolution_date` + `proposed_irp_name_placeholder` + `proposed_irp_ibbi_registration_placeholder` — for Section 10 IBC
- `nclt_order_being_appealed_date` + `nclt_order_being_appealed_short_summary` — for Section 421 / Section 61 appeals
- `limitation_anchor_date` + `limitation_filing_date`
- `parties` — list of party-role + party-type + party-name-placeholder

Case-type-specific fields (for the relevant skill) layer on top of the minimum schema — see each case-type SKILL.md.

---

## Built-in compliance disciplines

The Verifier enforces several disciplines mandatory in Indian company-law practice — see `skills/_drafting_common/SKILL.md` and `skills/_company_drafting_base/SKILL.md` for the full discipline framework. Headline disciplines:

- **Companies Act 1956 → Companies Act 2013 transition** — every Sec citation is reviewed against the modern Act. Verifier flags any reference to legacy 1956 Act provisions (e.g. Section 397 / 398 → corresponding Section 241 / 242 of 2013; Section 100 / 102 → Section 66 of 2013; Section 391-394 → Sections 230-232 of 2013; Section 235 / 237 → Section 213 of 2013; Section 560 → Section 248 — 252 of 2013). The obsolete *"Statement in lieu of prospectus"* under Section 70 of the 1956 Act is flagged as obsolete and removed.
- **Section 244 threshold discipline** — for Section 241-242 petitions, the Verifier confirms the petitioner satisfies the not-less-than-100-members OR one-tenth-of-total-members OR one-tenth-of-issued-share-capital threshold (whichever is less); where the petitioner falls below the threshold, the Section 244(1) proviso waiver application is mandatory and attached.
- **Section 230 scheme-disclosure discipline** — Section 230(2) disclosure of all material facts, latest financial position, latest auditor's report, pending investigation, scheme effect on creditors / KMP / promoters; Section 230(3) valuation report; Section 230(7) notice to regulators (SEBI / RBI / CCI / Income-tax / ROC / regional director / Official Liquidator) — all checked.
- **Section 66 creditor-protection discipline** — special resolution under Section 66(1); auditor's certificate on absence of arrears of deposit repayment / interest on deposits under Section 66(2)(b); list of creditors with consents / unsatisfied creditors with proposed settlement under Section 66(2)(c); public notice under Section 66(4) — all checked.
- **Section 252 limitation discipline** — for Section 252(1) appeal: 3 years from the date of strike-off order; for Section 252(3) application: 20 years from publication of the strike-off notice. Evidence of operation pleaded with documentary anchors (bank statements / GST returns / income-tax returns).
- **Section 213 maintainability discipline** — for Section 213(a) application: not less than 100 members OR members holding 1/10th of total voting power; for Section 213(b) application: prima-facie material on fraud / misfeasance / unlawful purpose with documentary anchors.
- **Section 8 IBC demand-notice ingredient discipline (Mobilox-style)** — for any Section 9 application: notice in Form 3 / 4 served on the Corporate Debtor at registered office; 10-day reply window expired without payment AND without notice of pre-existing dispute; the Drafter pre-empts the *Mobilox Innovations* defence (any plausible dispute supported by some material, even if not finally adjudicated, is enough to defeat Section 9 admission).
- **Section 10 IBC special-resolution discipline** — for any Section 10 application: special resolution of members under Section 10(3)(c) read with Section 114(2) Companies Act 2013 (3/4 majority); or for LLP — resolution of 3/4 of partners; the Drafter checks the resolution's registered-with-MCA filing where applicable.
- **IBC default-threshold discipline** — Section 9 (₹1 crore current threshold), Section 10 (₹1 crore current threshold), Section 10A suspension-window check (25 March 2020 — 24 March 2021).
- **Section 421(3) Companies Act limitation discipline** — 45 days from receipt of NCLT order, extendable on sufficient cause to a further 45 days; condonation application mandatory beyond 45 days.
- **Section 61(2) IBC limitation discipline** — 30 days from the date of the NCLT order, extendable on sufficient cause to a further 15 days; condonation application mandatory beyond 30 days.
- **SS-1 / SS-2 compliance discipline** — any board / general-meeting record relied on is checked against Secretarial Standard 1 (Meetings of the Board of Directors) and Secretarial Standard 2 (General Meetings) — notice period, quorum, recording of minutes, signed minutes, attendance register. Defects flagged for the Refiner.
- **Statutory-currency discipline** — CrPC 1973 references converted to BNSS 2023 (Section 200 → 223; Section 482 → 528); IEA 1872 references converted to BSA 2023 (Section 65B → 63; Section 126 → 132); Companies Act 1956 references converted to Companies Act 2013.
- **Innoventive Industries IBC admission discipline** — at admission stage only default + completeness + IRP eligibility are examined; merits not re-adjudicated. (Operates as a lens for Section 9 / Section 10 admission and as a lens for Section 61 appellate review of admission orders.)
- **CIRP timeline discipline** — Section 12 IBC: 180 days from CIRP commencement extendable to 330 days; relevant in any Section 9 / Section 10 prayer and in any Section 61 appeal challenging an admission, plan-approval, or liquidation order.

---

## Privacy firewall — extra discipline for company-law content

Company-law tribunal pleadings contain sensitive material — KYC of directors and shareholders, Corporate Identification Numbers, Director Identification Numbers, PAN, share-certificate numbers, valuation reports, scheme particulars, financial statements, operational-debt invoices, board minutes. The plugin's privacy discipline:

1. **Reader** substitutes every party name (Petitioner, Respondent Company, Respondent Director, Operational Creditor, Corporate Debtor, Authorised Signatory), every CIN, every DIN, every PAN, every share-certificate number, every outstanding-debt figure, every operational-debt invoice number, and every authorised-signatory name with structural placeholders before downstream processing.
2. The placeholder → real-value mapping is stored in the header of `case-facts.md` on the user's local machine **only**.
3. **Format / Drafter / Verifier / Overseer** operate **only** on placeholder-substituted content. The underlying AI runtime never holds raw CIN / DIN / share-capital figures.
4. **Refiner** re-substitutes real values into the final `.docx`, strictly on the user's machine.
5. `.gitignore` excludes `case-facts.md` and `case-config.md` so they cannot be committed accidentally.

The user can verify the firewall by inspecting `case-facts.md` after the Reader runs — every party name appears as `[Petitioner-A]` / `[Respondent-B]`, every CIN as `[CIN-Placeholder]`, every DIN as `[DIN-Placeholder]`, every share-certificate number as `[Share-Certificate-Placeholder]`. The mapping is in the header of the same file.

---

## Why MIT License

The MIT licence is the most permissive widely-recognised open-source licence. Anyone may use, modify, distribute, sublicense, or sell the plugin or any derivative. The licence is short, well-understood, and compatible with every other open-source licence the legal community is likely to encounter. No proprietary gating, no field-of-use restriction, no contributor licence agreement (CLA) gymnastics. The accompanying `NOTICE.md` does not modify the licence — it documents the provenance and the privilege position so that any future audit can verify the plugin's clean origin.

---

## Sibling plugins

This plugin is one in the **Wolfgang Rush** family of Indian legal-drafting plugins. All thirteen siblings ship under the same six-agent pipeline (Reader → Format → Drafter → Verifier → Refiner → Overseer) and the family-of-plugins doctrine — each plugin narrowly scoped to one practice area / forum:

| Plugin | GitHub repo | Scope |
|---|---|---|
| `supreme-court-drafting` | [supreme-court-drafting-litigation](https://github.com/Wolfgangrush/supreme-court-drafting-litigation) | SLPs · Writ Art 32 · Transfer · Review · Curative — Supreme Court of India |
| `indian-hc-drafting` | [indian-hc-drafting-litigation](https://github.com/Wolfgangrush/indian-hc-drafting-litigation) | Pleadings across all 25 Indian High Courts (bench-config-aware) |
| `district-court-drafting` | [district-court-drafting-litigation](https://github.com/Wolfgangrush/district-court-drafting-litigation) | Plaints · WS · CPC applications · BNSS complaints across 25+ States (state-config) |
| `indian-family-drafting` | [indian-family-drafting-litigation](https://github.com/Wolfgangrush/indian-family-drafting-litigation) | HMA · SMA · IDA · matrimonial · custody · DV Act · maintenance · adoption |
| `indian-contracts-drafting` | [indian-contracts-drafting-litigation](https://github.com/Wolfgangrush/indian-contracts-drafting-litigation) | MSA · NDA · employment · lease · sale · GPA · SHA · will · loan · arbitration |
| `indian-banking-drafting` | [indian-banking-drafting-litigation](https://github.com/Wolfgangrush/indian-banking-drafting-litigation) | DRT · SARFAESI · NI Act 138 · IBC §7 / §95 · DRAT |
| `indian-labour-drafting` | [indian-labour-drafting-litigation](https://github.com/Wolfgangrush/indian-labour-drafting-litigation) | ID Act · POSH · PG · EPF · ESI · MW · IESO + State exemplars |
| `indian-property-drafting` | [indian-property-drafting-litigation](https://github.com/Wolfgangrush/indian-property-drafting-litigation) | Gift · Exchange · Release · Trust · Wakf · Easement · Partition · Settlement · Mortgage · TIR |
| `indian-company-drafting` (this) | [indian-company-drafting](https://github.com/Wolfgangrush/indian-company-drafting) | NCLT (241/242 · 245 · 230-232 · 66 · 252 · 213) · NCLAT (421 + 61) · IBC §9 / §10 |
| `indian-tax-drafting` | [indian-tax-drafting](https://github.com/Wolfgangrush/indian-tax-drafting) | Form 35 CIT(A) · Form 36 ITAT · Form 10A · Sec 148A · 263/264 · 271/270A · 144C · 201 · 260A |
| `indian-consumer-drafting` | [indian-consumer-drafting](https://github.com/Wolfgangrush/indian-consumer-drafting) | District / State / NCDRC + medical-negligence + product liability |
| `indian-mact-drafting` | [indian-mact-drafting](https://github.com/Wolfgangrush/indian-mact-drafting) | MV Act 1988 (2019 amended) · Sarla Verma + Pranay Sethi · state-config |
| `indian-ip-drafting` | [indian-ip-drafting](https://github.com/Wolfgangrush/indian-ip-drafting) | Copyright · Trade Marks · Patents · Designs + HC IP Divisions (post-IPAB-abolition) + Anton Piller / John Doe |

Each plugin can be installed independently, each plugin's Rule 36 firewall is narrow and reviewable, each plugin's bench / forum discipline is depth-validated within its scope, and the user installs only what they need.

---

## Why this exists

Indian company-law tribunal practice currently has no open-source pleading-drafting infrastructure. Practising advocates piece together pleadings from their own past drafts, from senior advocates' templates, from the various textbook precedent collections (Ramaiya, Sumant Batra, Subodh Kumar Jain, Aggarwal on Companies Act, the ICSI publications on SS-1 / SS-2), and from such precedent volumes as the publishers issue from time to time. The result is uneven quality, uneven compliance with the latest statutory-currency rules (Companies Act 1956 → 2013 transition is still imperfectly internalised across the bar; BNSS 2023 / BSA 2023 transitions are very recent), uneven discipline on the procedural traps (Section 244 threshold, Section 230 disclosure under Section 230(2), Section 66 creditor-protection regime, Section 252 limitation, Section 421(3) / Section 61(2) appellate limitation, Section 8 IBC pre-existing-dispute discipline per Mobilox), and routine omissions that opposing counsel exploit.

A plugin that codifies the procedural skeletons + the statutory-currency rules + the Verifier-side discipline + the privacy firewall is the first piece of infrastructure that the Indian company-law tribunal practice has had — the second piece is community contribution from advocates who file regularly in specific NCLT benches and the NCLAT and who deepen the bench-specific Practice Directions discipline.

Foreign legal-AI tools cannot fill this gap. The procedural conventions are jurisdiction-specific; the statutory framework is Companies Act 2013 + IBC 2016 + NCLT Rules 2016 + NCLAT Rules 2016 which no foreign training data has indexed at depth; the formatting requirements at the Registry counter of an NCLT / NCLAT bench are matters of bench practice that no foreign tool has encountered.

This plugin opens that door. It is most-deeply-validated for the practice idiom of the author at the Bombay High Court Nagpur Bench (with the NCLT Mumbai Bench and NCLAT as the primary appellate / first-instance company-law fora), and shall be deepened with respect to other benches as community contributors raise GitHub issues and Pull Requests with their bench's specific Practice Directions.

---

## Roadmap

- [x] **v0.1.0-alpha (current)** — universal company-law tribunal pleading skeleton + 10 case-type skills + 6-agent pipeline + privacy firewall + Verifier disciplines + 0 bench-specific exemplars
- [ ] **v0.1.x** — bug fixes, quality-gate iteration, language-register polish, formatting refinements driven by user feedback
- [ ] **v0.x onward** — bench-specific Practice Direction calibration deepening per NCLT Mumbai / Delhi Principal / Chennai / Kolkata / Bangalore / Ahmedabad / Allahabad / Cuttack / Indore / Amaravati and the NCLAT Principal Bench and NCLAT Chennai Bench; additional case-type skills (Section 233 fast-track merger / Section 248 voluntary strike-off application / Section 271 winding-up petition / Section 35 IBC liquidator-stage challenges / Section 43 — 51 IBC avoidance transactions / Section 60(5) IBC residual jurisdiction applications); and procedural-rule updates as they arrive
- [ ] **v1.0.0** — stable release after community-validated formatting and field-testing

Per-bench deep validation will arrive in the order advocates contribute. The plugin's case-config architecture means any advocate filing regularly before a given NCLT bench can deepen the calibration for that bench by opening an issue or pull request with their bench's idiom — no central roadmap is needed to enable that. The roadmap above is therefore intentionally open-ended.

---

## Contributing

Advocates with regular NCLT / NCLAT practice are invited to contribute bench-config calibration for the specific tribunal they practise before. Open a GitHub issue with:

- Your practice bench (e.g., *"NCLT Mumbai Bench Court IV"* / *"NCLAT Principal Bench New Delhi"* / *"NCLAT Chennai Bench"*)
- Your bench's Cause Title format
- Your bench's case-number convention (e.g., *"C.P. No. ___ of 2026"* / *"C.A. No. ___ of 2026"* / *"Comp. App. (AT) (CH) (Ins) No. ___ of 2026"*)
- Your bench's filing-counter conventions (annexure markers / index format / verification format / softcopy submission rules)
- Recent Practice Directions issued by the bench affecting pleading format

Pull requests are welcome with a one-paragraph explanation of the change and a reference to the bench rule or Practice Direction that justifies it.

This plugin is open source under MIT.

---

## Contact

Author and maintainer: **Rushikesh R. Mahajan**, Advocate, enrolled with the Bar Council of Maharashtra and Goa.

GitHub: <https://github.com/Wolfgangrush>

Issues raised with reproducible context are handled on a best-effort basis; this is an open-source contribution maintained outside the author's professional engagements and does not constitute a vehicle for legal services.

---

## Author and brand

The author is **Rushikesh R. Mahajan**, Advocate, practising before the Bombay High Court, Nagpur Bench. The plugin is published under the open-source brand **Wolfgang Rush**, which is the author's publishing handle for legal-technology infrastructure. Personal accountability under the Advocates Act 1961 attaches to the author regardless of the use of a publishing handle.

---

## Provenance and privilege statement

See `NOTICE.md` for the full provenance + privilege + privacy + Rule 36 compliance statement. The short version:

- The plugin contains only universal procedural skeletons, formatting conventions, statutory references, and generic placeholders
- The plugin contains no specific client matter, no client communications, no client documents, no personal data of any data principal, and no tracking / telemetry / analytics
- The plugin is, in legal character, identical to a published company-law textbook — procedural knowledge in machine-readable form
- The author retains full enrolment, full responsibility, and full liability under the Advocates Act 1961 and the Bar Council of India Rules

---

## Compliance posture — Supreme Court e-Committee AI framework

This plugin is **assistive drafting infrastructure**, not autonomous decision-making software. Its operational posture is aligned with the Supreme Court of India e-Committee's stated position on AI in legal work.

> *"AI and digital tools must be used as supportive instruments and should not be allowed to override judicial reasoning."*
>
> — **Justice Rajesh Bindal**, Judge, Supreme Court of India
> [*Judicial Process Re-engineering and Digital Transformation*](https://www.sci.gov.in/press-release-dated-april-12-2026/) conference, 11–12 April 2026
> Organised by the Supreme Court e-Committee in collaboration with the Department of Justice, Government of India.
> ([Coverage — Law Trend](https://lawtrend.in/ai-must-not-replace-judicial-reasoning-warns-supreme-court-justice-rajesh-bindal/))

The same posture underpins the Supreme Court's own AI infrastructure for the judiciary:

- **[SUPACE](https://www.drishtiias.com/daily-news-analysis/ai-portal-supace)** — *Supreme Court Portal for Assistance in Court Efficiency.* AI-enabled assistive tool launched on 6 April 2021 by then-CJI S.A. Bobde. Provides legal research, fact extraction, document review, and drafting assistance to judges and legal researchers. **By design, SUPACE is not a decision-making system** — it processes facts and surfaces them to the human user. The Supreme Court has recommended adoption across all Indian High Courts.

- **[SUVAS](https://www.drishtijudiciary.com/current-affairs/supreme-court-vidhik-anuvaad-software-suvas)** — *Supreme Court Vidhik Anuvaad Software.* AI-powered translation tool launched in November 2019 by then-CJI S.A. Bobde. Translates judicial documents, orders, and judgments between English and ten Indian regional languages.

### What this plugin does — and does not — do under that framework

**Does:**

- Generate structural skeletons of pleadings, drawing on public statutes, schedule forms, and court rules.
- Run a six-agent assistive pipeline (Reader → Formatter → Drafter → Verifier → Refiner → Overseer) over the user's case facts.
- Surface citations, procedural anchors, and bench-specific conventions for advocate review.

**Does NOT:**

- Generate final filings autonomously.
- Substitute for advocate professional judgment.
- Replace human verification.
- Operate without an enrolled advocate retaining full professional responsibility.

**Every draft produced through this plugin must be advocate-owned and human-verified before filing.** The enrolled advocate using this plugin retains full professional responsibility under the Advocates Act 1961 and the Bar Council of India Rules, including verification of facts, accuracy of citations, correctness of legal grounds, propriety of the prayer, and signature on every pleading filed.

This is the same standard the Supreme Court itself applies to its own AI infrastructure (SUPACE / SUVAS): **AI as supportive instrument, never as decision-maker.**

---

## Disclaimer and Bar Council of India Rule 36 compliance

This repository is published as a personal open-source contribution to the legal-technology ecosystem. It is **not** an advertisement of professional services, **not** a solicitation of work, **not** an undertaking to act as counsel in any matter, and **not** a vehicle through which the author accepts professional engagement. No commercial engagement is offered through this repository.

Bar Council of India Rule 36 of the Standards of Professional Conduct and Etiquette prohibits an advocate from soliciting work or advertising professional services through any medium. This repository complies with Rule 36 in both letter and spirit:

- No commercial offering is made through this repository
- No representation of professional results is made
- No invitation to engage the author professionally is made
- The README contains no contact details inviting professional retainer

The plugin is published in the same legal character as any practitioner's open-source library, public continuing-legal-education contribution, or published textbook chapter — the medium is software, the content is procedural knowledge, the practitioner retains full Bar discipline and accountability.

---

## License

MIT — see `LICENSE`.
