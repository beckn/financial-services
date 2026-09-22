## Devkit

A developer toolkit for building and testing applications on the **beckn financial-services testnet**. This devkit provides a pre-configured adapter stack and ready-to-use Postman collections to help you get started quickly with the "Personal Loan" flow, converted to Beckn Protocol v2.0.0 LTS.

> **TEMPORARY, pre-merge only:** `subscriberId`/`senderId`/`receiverId` currently use the shared
> `bap.example.com` / `bpp.example.com` testnet identities from `starter-kit`'s `generic-devkit`
> (borrowing their already-registered keys) purely so the flow can be exercised end-to-end against
> the real DeDi registry before dedicated `financial-services` participant entities exist. Once two
> new entities are registered for this domain, these identities (and the matching keys in
> `config/financial-services-bap.yaml` / `financial-services-bpp.yaml` and
> `manifests/*-node-manifest.yaml`) must be replaced before this PR merges -- do not treat
> `bap.example.com`/`bpp.example.com` as this domain's real, permanent identities.

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Repository Structure](#repository-structure)
- [Quick Start](#quick-start)
- [Importing Postman Collections](#importing-postman-collections)
- [Making API Requests](#making-api-requests)
- [Architecture](#architecture)
- [Troubleshooting](#troubleshooting)

---

## Overview

The Financial Services Devkit enables developers to simulate and test decentralised personal-loan
transactions over the beckn protocol (v2.0.0 LTS). It models the the "Personal Loan" flow
flow: a borrower-facing lending platform (CN / BAP, `bap.lending.example.com`) discovering loan
products from a lender platform (PN / BPP, `bpp.northbridgebank.example.com`), selecting an offer,
completing an out-of-band KYC/application journey, and confirming a sanctioned loan contract. It
bundles a Docker-based adapter setup and Postman collections so you can spin up a local environment
and begin making beckn-compliant API calls within minutes.

---

## Prerequisites

Before you begin, ensure the following tools are installed on your system:

- **Git** — to clone this repository
- **Docker** and **Docker Compose** — to run the adapter stack
  - [Install Docker](https://docs.docker.com/engine/install/)
  - Docker Compose is included with Docker Desktop; for Linux, follow the [Compose plugin guide](https://docs.docker.com/compose/install/)
- **Postman** — to import and run the test collections
  - [Download Postman](https://www.postman.com/downloads/)

---

## Repository Structure

```text
financial-services-devkit/
├── config/          # Adapter configuration (BAP/BPP app config + routing rules)
├── install/         # Docker Compose files, Caddy reverse-proxy config, ngrok tunnel template
│   ├── Caddyfile
│   ├── docker-compose-financial-services.yml
│   ├── docker-compose-financial-services-local.yml
│   └── ngrok.yml.example
├── manifests/       # Node manifests (schema version declarations) for the BAP and BPP
├── postman/         # Postman collection for testing the the Personal Loan flow
├── data/beckn/      # Sample published catalog data (loan-product catalog) served by catalog/publish
└── schemas/         # Empty here — v2.0.0 custom schemas for this domain live at the repo-root
                      # schema/ directory (e.g. schema/LoanProduct/v2.0/); see schemas/README.md
```

---

## Quick Start

Follow these steps to get the devkit running locally:

**1. Clone the repository**

```bash
git clone https://github.com/beckn/financial-services.git
cd financial-services/testnet/financial-services-devkit/install
```

**2. Start the adapter stack**

```bash
docker compose -f docker-compose-financial-services.yml up --build
```

This command builds and starts all required services. The first run may take a few minutes to pull
and build Docker images. Use `docker-compose-financial-services-local.yml` instead if you are running
against a locally-built `beckn-onix:latest` image rather than the published `fidedocker/onix-adapter`
image.

**3. Verify the stack is running**

Once the containers are up, verify the services are healthy:

```bash
docker compose -f docker-compose-financial-services.yml ps
```

All services should show a `running` or `healthy` status.

**4. (Optional) Expose the stack for remote callbacks**

If you need a lender (BPP) hosted elsewhere to reach this stack's `on_*` callbacks, tunnel the
`beckn-router` port via ngrok:

```bash
cp ngrok.yml.example ngrok.yml   # fill in your authtoken (and static domain, if you have one)
ngrok start --all --config ngrok.yml
```

Then set the Postman collection's `public_url` variable to the tunnel URL.

---

## Importing Postman Collections

The `postman/` directory contains a pre-built collection for testing the personal-loan flow.

**Step 1 — Open Postman**

Launch the Postman desktop application.

**Step 2 — Import the collection**

1. Click **Import** in the top-left corner of the Postman window.
2. Select **File** in the import modal.
3. Navigate to the `postman/` directory in your cloned repository.
4. Select `FinancialServices.postman_collection.json` and click **Open**.

---

## Making API Requests

Once the stack is running and the collection is imported:

1. Expand the collection in the Postman sidebar to view available requests.
2. Click on a request to open it.
3. Review the request method, URL, and body.
4. Click **Send** to execute the request.
5. The response will appear in the panel below.

The collection is ordered to reflect the the Personal Loan transaction flow:
`discover` → `select` → `status` → `confirm`, with the corresponding `on_discover` / `on_select` /
`on_status` / `on_confirm` callbacks served by the `sandbox-bpp` mock (see
`sandbox-payloads/beckn.one/testnet-financial-services/response/` at the repo root, including the
distinct `on_status-initiated` / `on_status-in-progress` / `on_status-disbursed` /
`on_status-emi-due` / `on_status-emi-paid` / `on_status-loan-closed` states). Run requests in
sequence for an end-to-end test.

---

## Architecture

The devkit simulates a beckn-compliant BAP (borrower-facing lending platform) and BPP (lender / loan
origination system) adapter pair locally, fronted by a single Caddy reverse proxy (`beckn-router`) and
backed by shared Redis cache. The `catalog/publish` module on the BPP adapter publishes the sample
loan-product catalog under `data/beckn/catalogs/` for discovery.

_(No architecture diagram is included in this devkit; see `local-retail/testnet/retail-devkit/resources/architecture.png` for the equivalent generic BAP/BPP adapter data-flow diagram.)_

---

## Troubleshooting

**Containers fail to start**

Check for port conflicts. Inspect logs with:

```bash
docker compose -f docker-compose-financial-services.yml logs
```

**Postman requests return connection errors**

Ensure the Docker stack is running and the `bap_adapter_url` collection variable points to the
correct host and port (`http://localhost:8081/bap/caller` by default).

**Images fail to build**

Make sure Docker has sufficient resources allocated (RAM/CPU) and that you have a stable internet
connection for pulling base images.

**Stopping the stack**

```bash
docker compose -f docker-compose-financial-services.yml down
```

---

## License

This project is part of the beckn open protocol ecosystem. Refer to the root repository for license
details.
