# SanctionedLoan -- v2.0

**Schema Pack Version:** 2.0.0
**Status:** Initial release

## Notes

Initial release of the SanctionedLoan schema (`commitmentAttributes`) for the "Personal Loan" use case, converted to Beckn v2.0.0 LTS. Carries the loan
account reference and sanctioned principal/rate/tenure/EMI/disbursed amount,
attached to `Contract.commitments[]` at `confirm`/`on_confirm`.

`formSubmission` composes the core `beckn:FormSubmission` schema as-is (not
redefined here) to carry the offline/branch-journey KYC submission tied to this
commitment. This composition folds a system-generated `status` value into
`FormSubmission.data` (an open string-keyed map) as a documented, temporary
stopgap — see `financial-services/schema/INDEX.md` ("Core types composed") and
`../../ondc_fis_v2_payloads.md` Section 5 for the full reasoning and the two
longer-term alternatives considered.
