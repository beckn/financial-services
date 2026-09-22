# Decentralized Financial Services Protocol
## Introduction

The financial services sector plays a pivotal role in the global economy, encompassing a wide array of institutions and services that facilitate the management, investment, and movement of money. These services cater to the diverse needs of individuals, businesses, and governments alike. As of today, the financial services sector has evolved and diversified, offering a range of essential services. The financial services sector's continued growth and adaptation to emerging technologies are crucial for fostering economic development and prosperity on a global scale. As technology continues to advance, new types of financial services may emerge, further reshaping the landscape of this vital sector.

The **Decentralized Financial Service Protocol** of DFSP is an adaptation of [beckn protocol](https://github.com/beckn/protocol-specifications) that aims to unify various financial services that exist today by implementing an interoperable API specification.

## Contents

- [Status](#status)
- [Release History](#release-history)
- [Repository Structure](#repository-structure)
- [Local Testing (Devkit)](#local-testing-devkit)
- [Working Group Members](#working-group-members)
- [Implementing the Specification](#implementing-the-specification)
- [Acknowledgements](#acknowledgements)

---

## Status

This repository now also includes a **Beckn v2.0.0 LTS-compliant schema pack, devkit, and sandbox payloads** for the "Personal Loan" use case, alongside the original v1-adaptation `api/`/`docs/`/`examples/` content. The two live side by side for now — nothing under the original `api/financial-services.yaml` spec or its examples was changed; the v2 pack is new, additive content built from a from-scratch conversion of the Personal Loan flow to the v2.0.0 LTS generalised model (Resource/Offer/Contract).

If you're looking to try the v2.0.0 flow quickly, start with [Local Testing (Devkit)](#local-testing-devkit) below.

---

## Release History

| Version | Release Date    | Adaptation to Core Spec Version | Authors      |
|:-------:|-----------------|---------------------------------|--------------|
| 0.1.0   | 22nd July, 2023 | 1.1.0                           | Ravi Prakash |
| 2.0.0   | 22nd September, 2026 | [core-v2.0.0-lts](https://github.com/beckn/protocol-specifications-v2/releases/tag/core-v2.0.0-lts) | Mayuresh Nirhali |

---

## Repository Structure

```
financial-services/
├── api/                                 # v1-adaptation API spec (core 1.1.0) -- unchanged
│   ├── core/
│   ├── financial-services.yaml
│   └── README.md
├── docs/                                # v1-adaptation implementation guide -- unchanged
├── examples/                            # v1-adaptation worked examples -- unchanged
│   ├── health-insurance/
│   ├── invoice-based-loans/
│   ├── mutual-funds/
│   └── personal-loans/
├── schema/                               # NEW -- Beckn v2.0.0 LTS custom schema pack (Personal Loan)
│   ├── LoanProduct/v2.0/                 #   resourceAttributes
│   ├── LoanOffer/v2.0/                   #   offerAttributes
│   ├── SanctionedLoan/v2.0/               #   commitmentAttributes
│   ├── LoanCharges/v2.0/                 #   considerationAttributes
│   ├── LoanApplicationChecklist/v2.0/    #   performanceAttributes
│   ├── LoanRepaymentTracking/v2.0/       #   performanceAttributes
│   ├── LoanContractTerms/v2.0/           #   contractAttributes
│   ├── INDEX.md                          #   full schema pack index
│   └── README.md                         #   design decisions, gap analysis
├── testnet/
│   └── financial-services-devkit/        # NEW -- local BAP/PN adapter stack for the v2.0.0 flow
├── sandbox-payloads/                     # NEW -- canned v2.0.0 request/response fixtures
│   └── beckn.one/testnet-financial-services/{request,response}/
├── LICENSE.md
└── README.md                             # This file
```

---

## Local Testing (Devkit)

[`testnet/financial-services-devkit/`](./testnet/financial-services-devkit/) wires the "Personal Loan" v2.0.0 LTS flow (`discover` → `on_discover` → `select` → `on_select` → `confirm` → `on_confirm` → `status`/`on_status`) to a runnable local CN/PN adapter stack, so it can be driven with Postman and observed end to end — not just read as static JSON. It bundles ONIX adapter configs and routing rules (copied and adapted from the [starter-kit](https://github.com/beckn/starter-kit) generic devkit), a Caddy reverse proxy with optional ngrok tunnel support, a docker-compose stack, node manifests referencing the schema pack above, and a ready-to-import Postman collection.

Its mock PN is backed by canned response fixtures under [`sandbox-payloads/`](./sandbox-payloads/beckn.one/testnet-financial-services/).

See **[testnet/financial-services-devkit/README.md](./testnet/financial-services-devkit/README.md)** for prerequisites, quick start, and configuration reference.

## Working Group Members

| Name             | Role                           | Github Username |
|------------------|--------------------------------|-----------------|
| Mayuresh Nirhali | Contributor                    |                 |
| Ravi Prakash     | Maintainer, Protocol Architect | @ravi-prakash-v |
| Pramod Varma     | Maintainer, Reviewer           | @pramodkvarma   |
| Sujith Nair      | Reviewer                       | @sjthnrk        |
| Hrushikesh Mehta | Subject Matter Expert          | @hrushmehta     |
| Antriksh Parmar  | Subject Matter Expert          | @Ratonhnhake-ton|
| Mohit Monga      | Subject Matter Expert          | @Mmonga12       |
| Ashish Desai     | Subject Matter Expert          |                 |

## Implementing the specification

To understanding how to implement the specification click [here](./docs)

## Acknowledgements

The author(s) of this specification would like to thank the following volunteers for their contribution to the development of this specification

### Version 0.1.0
- Hrushikesh Mehta - ONDC
- Antriksh Parmar - ONDC
- Mohit Monga - ONDC
- Ashish Desai - ONDC



