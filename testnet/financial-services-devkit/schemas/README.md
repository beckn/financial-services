# schemas/

This directory is intentionally empty in the devkit itself (mirroring the pattern in
`local-retail/testnet/retail-devkit/schemas/`).

The actual v2.0.0 custom JSON schemas and JSON-LD contexts for this domain
(`LoanProduct`, `LoanOffer`, `SanctionedLoan`, `LoanCharges`,
`LoanApplicationChecklist`, `LoanRepaymentTracking`, `LoanContractTerms`) live at
the repository root under `schema/`, e.g. `schema/LoanProduct/v2.0/`.

The devkit's adapters (`config/financial-services-bap.yaml` /
`financial-services-bpp.yaml`) reference these schemas by their published
`raw.githubusercontent.com` URL rather than a local copy, so no files are
duplicated here.
