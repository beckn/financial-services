# LoanApplicationChecklist -- v2.0

**Schema Pack Version:** 2.0.0
**Status:** Initial release

## Notes

Initial release of the LoanApplicationChecklist schema (`performanceAttributes`,
leg 1 of 2) for the "Personal Loan" use case, converted to Beckn v2.0.0
LTS. Tracks the application/KYC checklist (personal info -> loan offer -> offline
journey -> KYC -> bank verification -> e-mandate -> loan agreement) as
`Contract.performance[]` entries across `on_status` polls and `on_confirm`.
Distinct from `LoanRepaymentTracking` (leg 2, post-disbursement EMI ledger) — both
attach to the same `performanceAttributes` container but have unrelated lifecycles
and field sets. See `../../ondc_fis_v2_payloads.md` Section 3.
