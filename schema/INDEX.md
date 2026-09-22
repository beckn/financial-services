# Beckn v2.0.0 LTS Financial Services Schema Pack Index

Complete index of seven custom Beckn v2.0.0 LTS schema packs for the
"Personal Loan" use case.

---

## Quick Navigation

### Schema 1: LoanProduct
- **Purpose:** Catalog resource / loan product listing (rate/tenure/amount bands, journey type, application form link)
- **Container:** `resourceAttributes`
- **Prefix:** `lpra` -> https://schema.nfh.global/LoanProduct#
- **Files:** [attributes.yaml](./LoanProduct/v2.0/attributes.yaml) - [context.jsonld](./LoanProduct/v2.0/context.jsonld) - [vocab.jsonld](./LoanProduct/v2.0/vocab.jsonld) - [profile.json](./LoanProduct/v2.0/profile.json) - [renderer.json](./LoanProduct/v2.0/renderer.json) - [README.md](./LoanProduct/v2.0/README.md) - [example](./LoanProduct/v2.0/examples/loan-product-example.json)

**Key Properties:** `loanCategory`, `interestRateType`, `minInterestRate`/`maxInterestRate`, `minTenureMonths`/`maxTenureMonths`, `minLoanAmount`/`maxLoanAmount`, `currency`, `applicationFormUrl`, `journeyType`

---

### Schema 2: LoanOffer
- **Purpose:** Quoted loan terms for a specific applicant, pre-contract
- **Container:** `offerAttributes`
- **Prefix:** `loa` -> https://schema.nfh.global/LoanOffer#
- **Files:** [attributes.yaml](./LoanOffer/v2.0/attributes.yaml) - [context.jsonld](./LoanOffer/v2.0/context.jsonld) - [vocab.jsonld](./LoanOffer/v2.0/vocab.jsonld) - [profile.json](./LoanOffer/v2.0/profile.json) - [renderer.json](./LoanOffer/v2.0/renderer.json) - [README.md](./LoanOffer/v2.0/README.md) - [example](./LoanOffer/v2.0/examples/loan-offer-example.json)

**Key Properties:** `principalAmount`, `interestRate`, `tenureMonths`, `applicationFee`, `foreclosureFeePct`, `annualPercentageRate`, `repaymentFrequency`, `numberOfInstallments`, `installmentAmount`, `coolOffPeriod`, `keyFactStatementUrl`

---

### Schema 3: SanctionedLoan
- **Purpose:** The sanctioned loan once approved
- **Container:** `commitmentAttributes`
- **Prefix:** `sla` -> https://schema.nfh.global/SanctionedLoan#
- **Files:** [attributes.yaml](./SanctionedLoan/v2.0/attributes.yaml) - [context.jsonld](./SanctionedLoan/v2.0/context.jsonld) - [vocab.jsonld](./SanctionedLoan/v2.0/vocab.jsonld) - [profile.json](./SanctionedLoan/v2.0/profile.json) - [renderer.json](./SanctionedLoan/v2.0/renderer.json) - [README.md](./SanctionedLoan/v2.0/README.md) - [example](./SanctionedLoan/v2.0/examples/sanctioned-loan-example.json)

**Key Properties:** `loanAccountRef`, `principalAmount`, `sanctionedInterestRate`, `sanctionedTenureMonths`, `installmentAmount`, `numberOfInstallments`, `repaymentFrequency`, `disbursedAmount`, `formSubmission` (composes core `FormSubmission` — see "Core types composed" below)

---

### Schema 4: LoanCharges
- **Purpose:** Fee/charge breakdown for the loan
- **Container:** `considerationAttributes`
- **Prefix:** `lca` -> https://schema.nfh.global/LoanCharges#
- **Files:** [attributes.yaml](./LoanCharges/v2.0/attributes.yaml) - [context.jsonld](./LoanCharges/v2.0/context.jsonld) - [vocab.jsonld](./LoanCharges/v2.0/vocab.jsonld) - [profile.json](./LoanCharges/v2.0/profile.json) - [renderer.json](./LoanCharges/v2.0/renderer.json) - [README.md](./LoanCharges/v2.0/README.md) - [example](./LoanCharges/v2.0/examples/loan-charges-example.json)

**Key Properties:** `components[]` (`type`: PRINCIPAL/INTEREST/PROCESSING_FEE/INSURANCE_CHARGES/OTHER_UPFRONT_CHARGES/OTHER_CHARGES/NET_DISBURSED_AMOUNT, `value`, `currency`, `description`)

---

### Schema 5: LoanApplicationChecklist
- **Purpose:** KYC/application-processing checklist stops (pre-disbursement)
- **Container:** `performanceAttributes`
- **Prefix:** `laca` -> https://schema.nfh.global/LoanApplicationChecklist#
- **Files:** [attributes.yaml](./LoanApplicationChecklist/v2.0/attributes.yaml) - [context.jsonld](./LoanApplicationChecklist/v2.0/context.jsonld) - [vocab.jsonld](./LoanApplicationChecklist/v2.0/vocab.jsonld) - [profile.json](./LoanApplicationChecklist/v2.0/profile.json) - [renderer.json](./LoanApplicationChecklist/v2.0/renderer.json) - [README.md](./LoanApplicationChecklist/v2.0/README.md) - [example](./LoanApplicationChecklist/v2.0/examples/loan-application-checklist-example.json)

**Key Properties:** `stopType` (PERSONAL_INFORMATION/LOAN_OFFER/JOURNEY_OFFLINE/KYC/BANK_ACCOUNT_VERIFICATION/REPAYMENT_SETUP/LOAN_AGREEMENT), `stopStatus` (PENDING/INITIATED/IN_PROGRESS/SUCCESSFUL/FINALIZED/SELECTED), `sequence`

---

### Schema 6: LoanRepaymentTracking
- **Purpose:** Post-disbursement EMI/repayment tracking
- **Container:** `performanceAttributes`
- **Prefix:** `lrta` -> https://schema.nfh.global/LoanRepaymentTracking#
- **Files:** [attributes.yaml](./LoanRepaymentTracking/v2.0/attributes.yaml) - [context.jsonld](./LoanRepaymentTracking/v2.0/context.jsonld) - [vocab.jsonld](./LoanRepaymentTracking/v2.0/vocab.jsonld) - [profile.json](./LoanRepaymentTracking/v2.0/profile.json) - [renderer.json](./LoanRepaymentTracking/v2.0/renderer.json) - [README.md](./LoanRepaymentTracking/v2.0/README.md) - [example](./LoanRepaymentTracking/v2.0/examples/loan-repayment-tracking-example.json)

**Key Properties:** `installmentStatus` (EMI_DUE/EMI_PAID/LOAN_CLOSED), `installmentNumber`, `dueDate`, `paidAmount`, `outstandingPrincipal`

---

### Schema 7: LoanContractTerms
- **Purpose:** Static terms-of-engagement declaration (maps the v1 source's `BAP_TERMS`/`BPP_TERMS` intent tags)
- **Container:** `contractAttributes`
- **Prefix:** `lcta` -> https://schema.nfh.global/LoanContractTerms#
- **Files:** [attributes.yaml](./LoanContractTerms/v2.0/attributes.yaml) - [context.jsonld](./LoanContractTerms/v2.0/context.jsonld) - [vocab.jsonld](./LoanContractTerms/v2.0/vocab.jsonld) - [profile.json](./LoanContractTerms/v2.0/profile.json) - [renderer.json](./LoanContractTerms/v2.0/renderer.json) - [README.md](./LoanContractTerms/v2.0/README.md) - [example](./LoanContractTerms/v2.0/examples/loan-contract-terms-example.json)

**Key Properties:** `borrowerTerms` / `lenderTerms` (each: `staticTermsUrl`, `offlineContract`)

*Added after the initial six-schema pass, once the `confirm` sandbox payload (built separately from this source doc) was found to already depend on it — see `README.md` for the reconciliation note.*

---

## Core types composed (not owned by this pack)

`FormSubmission` is used by composition inside `SanctionedLoan.commitmentAttributes`
to carry the borrower's out-of-band KYC/application form submission. It is a
**reserved core Beckn schema** defined in `api/v2.0.0/beckn.yaml` of the
`protocol-specifications-v2` repo, not something this repo defines — there is
deliberately **no** `schema/FormSubmission/` package here. See `README.md`'s
"Core types composed" section for the full reasoning, including the documented
`data.status` stopgap.

---

## Technical Reference

### File Standards
Each schema pack contains 6 files plus one example:

| File | Purpose | Format |
|------|---------|--------|
| `attributes.yaml` | Complete schema definition | OpenAPI 3.1.1 |
| `context.jsonld` | Namespace and prefix mappings | JSON-LD Context |
| `vocab.jsonld` | Semantic vocabulary definitions | RDF/RDFS |
| `profile.json` | Beckn protocol configuration | JSON |
| `renderer.json` | UI/display templates | JSON |
| `README.md` | Full documentation | Markdown |
| `examples/*.json` | Concrete embedded instance | JSON |

### Naming Conventions
- **Container Names:** camelCase (`resourceAttributes`, `offerAttributes`, `commitmentAttributes`, `considerationAttributes`, `performanceAttributes`)
- **Properties:** camelCase
- **Classes:** PascalCase (`LoanProduct`, `LoanOffer`, ...)
- **Prefixes:** short lowercase (`lpra`, `loa`, `sla`, `lca`, `laca`, `lrta`, `lcta`)

### Semantic Integration
All schemas use Beckn core:
- **Context:** `https://schema.beckn.io/core/v2/context.jsonld#generalised`
- **Vocabulary:** `https://schema.beckn.io/core/v2/vocab.jsonld`
- **Own anchor:** `https://schema.nfh.global/<TypeName>/v2.0/context.jsonld` (not yet published to the registry — this repo is the source that would eventually be published there)

### Profile Configuration
All profiles specify:
- `protocol_version: "2.0"`
- `semantic_model: "generalised"`
- `version: "2.0.0"`

---

## Directory Structure

```
schema/
├── INDEX.md (this file)
├── README.md
├── context.jsonld
├── vocab.jsonld
├── LoanProduct/v2.0/{attributes.yaml,context.jsonld,vocab.jsonld,profile.json,renderer.json,README.md,examples/}
├── LoanOffer/v2.0/{...}
├── SanctionedLoan/v2.0/{...}
├── LoanCharges/v2.0/{...}
├── LoanApplicationChecklist/v2.0/{...}
├── LoanRepaymentTracking/v2.0/{...}
└── LoanContractTerms/v2.0/{...}
```

---

**Source design doc:** `ondc_fis_v2_payloads.md` (the "Personal Loan" flow, spec version 2.0.3, converted to Beckn v2.0.0 LTS)
**Status:** Initial release, ready for integration review
