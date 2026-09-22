# LoanContractTerms -- v2.0

**Schema Pack Version:** 2.0.0
**Status:** Initial release

## Notes

Initial release of the LoanContractTerms schema (`contractAttributes`) for the "Personal Loan" use case, converted to Beckn v2.0.0 LTS. Carries `bapTerms`/
`bppTerms` (each a `staticTermsUrl` + `offlineContract` flag), attached to the
top-level `Contract`. Maps the v1 source's generic `BAP_TERMS`/`BPP_TERMS` intent
tags (with `STATIC_TERMS`/`OFFLINE_CONTRACT` sub-tags) onto a typed extension
rather than reviving a free-form tags bag, which v2.0.0 LTS does not carry on
`Contract`/`Offer`. See `../../ondc_fis_v2_payloads.md` Section 5.

This was not part of the original six schemas built for this pack, but was added
after the `confirm` sandbox payload was found to already reference it (for the
`bapTerms`/`bppTerms` data carried at confirm time) — see `../../README.md` and
`../../INDEX.md` "Core types composed" / gap notes for the history.
