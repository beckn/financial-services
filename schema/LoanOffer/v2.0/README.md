# LoanOffer -- v2.0

**Schema Pack Version:** 2.0.0
**Status:** Initial release

## Notes

Initial release of the LoanOffer schema (`offerAttributes`) for the Personal Loan
"Personal Loan" use case, converted to Beckn v2.0.0 LTS. Carries applicant-specific
quoted terms (principal, rate, tenure, fees, EMI, cooling-off period) returned at
`select`/`on_select` time, before a Contract exists. See `../../ondc_fis_v2_payloads.md`
Section 3/4 for field derivation and the `on_select` payload example.
