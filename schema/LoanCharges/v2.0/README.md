# LoanCharges -- v2.0

**Schema Pack Version:** 2.0.0
**Status:** Initial release

## Notes

Initial release of the LoanCharges schema (`considerationAttributes`) for the "Personal Loan" use case, converted to Beckn v2.0.0 LTS. Carries a
`components[]` breakup (principal, interest, processing fee, insurance charges,
other upfront/other charges, net disbursed amount), attached to
`Contract.consideration[]`. See `../../ondc_fis_v2_payloads.md` Section 3/4.
