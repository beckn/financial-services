# Beckn v2.0.0 LTS Financial Services (FIS) Schema Pack

**Protocol Version:** 2.0
**Semantic Model:** generalised (Resource / Offer / Contract)
**Schema Pack Version:** 2.0.0
**Domain:** Financial Services — Lending (the "Personal Loan" use case)

## Overview

This schema pack provides the custom v2.0.0 LTS generalised-model extension schemas
needed for the "Personal Loan" flow, converted from the v1-style spec (2.0.3) to Beckn v2.0.0 LTS. Unlike `local-retail`, this domain has a
single flat schema set (no cross-vertical "core" + sub-vertical extension layering)
because personal lending is the only FIS use case modeled here so far.

The full conversion design, including the executive summary, transaction flow,
schema derivation rationale, complete example payloads for every action
(`discover`/`on_discover` through the post-disbursement `on_status` repayment
callbacks), and the gap analysis behind each judgment call, lives in
`ondc_fis_v2_payloads.md` (kept alongside this README at the repo's schema design
history) — that document is the source of truth for field names/types; this schema
pack implements it.

## Schema Taxonomy

| Schema | Container | Purpose |
|--------|-----------|---------|
| LoanProduct | `resourceAttributes` | Catalog resource / loan product listing: rate/tenure/amount bands, journey type, application form URL |
| LoanOffer | `offerAttributes` | Quoted loan terms: principal, interest rate, tenure, fees, EMI, cooling-off period |
| SanctionedLoan | `commitmentAttributes` | The sanctioned loan once approved: loan account ref, sanctioned amount/rate/tenure, disbursed amount |
| LoanCharges | `considerationAttributes` | Fee/charge breakdown components: principal, interest, processing fee, insurance, net disbursed amount |
| LoanApplicationChecklist | `performanceAttributes` | KYC/application-processing checklist stops: stopType, stopStatus, sequence |
| LoanRepaymentTracking | `performanceAttributes` | Post-disbursement EMI/repayment tracking: installment number, status, remaining installments, loan closure status |
| LoanContractTerms | `contractAttributes` | Static terms-of-engagement declaration: CN/PN `staticTermsUrl` + `offlineContract` flag |

Two distinct schemas attach to `performanceAttributes` (`LoanApplicationChecklist`
and `LoanRepaymentTracking`) because the application/KYC checklist and the
post-disbursement EMI ledger have unrelated lifecycles and field sets, even though
both attach to `Contract.performance[]` entries.

## Core types composed

`FormSubmission` is used by several of these schemas (currently `SanctionedLoan`,
via `commitmentAttributes.formSubmission`) to represent the borrower's out-of-band
application/KYC form submission. **`FormSubmission` is a reserved core Beckn
schema**, defined directly in `api/v2.0.0/beckn.yaml` of the
`protocol-specifications-v2` repo — it is **not** defined by this repo, and this
pack does not (and must not) ship a `schema/FormSubmission/` package, since that
would incorrectly imply this repo owns/defines that type.

Core today only composes `FormSubmission` in one place — `RatingInput.feedbackFormSubmission`
— but nothing in the core JSON Schema restricts `$ref: FormSubmission` to that one
field. `Resource.resourceAttributes`, `Offer.offerAttributes`, and
`Commitment.commitmentAttributes` all resolve to the core `Attributes` container,
which is `additionalProperties: true`, so a domain pack's custom `*Attributes`
schema is free to add a `formSubmission` property reusing `FormSubmission`'s exact
shape without violating core validation. This pack does exactly that.

**Documented stopgap**: `FormSubmission.data` is an open string-keyed map intended
for the user's submitted answers. This flow additionally folds a system-generated
`status` value into it (e.g. `"data": {"status": "SUCCESS"}`) to track the lender's
processing state of the submission across polls — a deliberate, temporary
compromise, not a recommendation. It conflates "what the user typed" with "how the
lender's system is progressing this submission." Two cleaner paths exist for the
future (out of scope here): (a) propose a dedicated `status` property on the core
`FormSubmission` schema upstream, or (b) define a separate, lending-specific
form/application-status-tracking schema instead of reusing `FormSubmission` for
that purpose. See `ondc_fis_v2_payloads.md` Section 5 for the full reasoning.

## Design Decisions

1. **No cross-vertical layering**: unlike `local-retail`'s Retail-Core +
   domain-extension pattern, this pack is a single flat set of seven schemas
   scoped to personal-loan lending. If additional FIS use cases (e.g. insurance,
   mutual funds) are added later, a similar core/extension split could be
   introduced.

2. **Two `performanceAttributes` schemas**: `LoanApplicationChecklist` (pre-
   disbursement KYC/application checklist) and `LoanRepaymentTracking` (post-
   disbursement EMI ledger) are kept as separate schemas rather than one combined
   schema, since they track unrelated lifecycles with non-overlapping fields.

3. **Flat `loanCategory` string**: `LoanProduct.loanCategory` models the source's
   hierarchical category taxonomy (`LOAN` > `PERSONAL_LOAN` > ...) as a flat code
   rather than reconstructing a category graph, since v2.0.0 LTS catalogs have no
   equivalent category-tree primitive. Simplification, not a gap.

4. **`LoanRepaymentTracking` is an inferred reconstruction**: the raw v1 source sample
   payloads for the post-disbursement leg do not expose a distinguishable
   per-installment field across callbacks. The `installmentStatus` vocabulary and
   tracking arc are a coherent narrative reconstruction consistent with the
   offer's agreed installment terms, not a literal source translation.

5. **`BAP_TERMS`/`OFFLINE_CONTRACT` tags**: mapped to a `contractAttributes` bag
   (`LoanContractTerms`) on the top-level `Contract`. This was not part of the
   original six schemas requested for this pack — it was added after the
   `confirm` sandbox payload was found to already reference it (built by a
   separate devkit/payloads pass working from the same source design doc). See
   `ondc_fis_v2_payloads.md` Section 5 for the original rationale.
