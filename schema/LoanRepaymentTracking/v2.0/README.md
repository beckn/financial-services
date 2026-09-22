# LoanRepaymentTracking -- v2.0

**Schema Pack Version:** 2.0.0
**Status:** Initial release

## Notes

Initial release of the LoanRepaymentTracking schema (`performanceAttributes`,
leg 2 of 2) for the "Personal Loan" use case, converted to Beckn v2.0.0
LTS. Tracks post-disbursement EMI/repayment state (`EMI_DUE` -> `EMI_PAID` ->
... -> `LOAN_CLOSED`) as `Contract.performance[]` entries sent via unsolicited
`on_status` callbacks as the loan amortizes.

**Caveat carried over from the source design doc**: the raw v1 source sample payloads
for this leg do not expose a distinct per-installment field; all callbacks share
materially the same envelope. This schema's `installmentStatus` vocabulary and
tracking arc are therefore an inferred narrative reconstruction consistent with
the offer's agreed `numberOfInstallments`/`installmentAmount`, not a literal
translation of distinguishable source fields — a real lender integration would
need to confirm its actual per-EMI status vocabulary. See
`../../ondc_fis_v2_payloads.md` Section 5.

Distinct from `LoanApplicationChecklist` (leg 1, pre-disbursement KYC/application
checklist) — both attach to the same `performanceAttributes` container but have
unrelated lifecycles and field sets.
