# NexOTC Protocol — Smart Escrow

**NexOTC** is the non-custodial settlement layer for private markets, giving institutions & brokers full control of dealflows and pipelines.

> **Status:** This repo hosts the public specification, structure and governance docs for the
> *NexOTC Protocol* (MVP). Source code will be added as development milestones are reached.

---

## Scope (MVP)

### On-chain modules
- **EscrowFactory** — deploys:
  - **EscrowContract (Per Deal)** for bilateral OTC DvP.
  - **InvestorEscrow (Per Investor)** for syndicates (commit/refund per investor).
  Binds fee/tranche policies at deploy and emits addresses.
- **FeeRouter** — dynamic fee logic with:
  tiered schedules, partner rebates, **payer-split** (e.g., 50/50, 60/40, 100/0),
  **fee floors** (gross and/or net) and multi-party routing (platform, brokers/introducers, leads).
- **TrancheManager** *(optional in MVP)* — TGE, tranches/vesting, and distribution receipts.
- **SyndicateController + AllocationBook** — allowlist & accreditation gating, min ticket, soft/hard
  caps, oversubscription policy, batch **settle/refund** across InvestorEscrows.
- **Governance (Timelock / Multisig)** — owns Factory and module params; upgrades and policy
  changes with delay and audit trail.
- **Event Catalog** — canonical events for indexing, dashboards, and formal receipts.

### Off-chain / Adapters
- **Compliance Adapter** — KYC/KYB, accreditation, AML/wallet risk checks before funding
  (no PII on-chain).
- **Event Index & Webhooks** — receipt generation, statements, exports.

### Design principles
- **Non-custodial** — funds stay in isolated vaults; no pooled wallets.  
- **Modular & upgrade-aware** — clear ownership, timelocked changes.  
- **Policy-driven** — fee tiers, payer-split, rebates, tranches are parameters, not code edits.  
- **Deterministic receipts** — every state change emits an auditable event.  
- **Privacy by design** — personal data stays off-chain; only cryptographic references on-chain.

---

## Event Catalog

**Factory**
- `EscrowDeployed(tradeId, escrow)`
- `InvestorEscrowDeployed(raiseId, investor, vault)`

**Escrow (Per Deal / Per Investor)**
- `FundsDeposited(party, amount)`
- `Settled(to, amount)` / `Refunded(to, amount)`
- `Expired(tradeId)` / `FallbackTriggered(reason)`

**Fees & Commissions**
- `FeeComputed(refId, gross, rebate, net, payerSplit)`
- `FeeRouted(to, amount)` / `CommissionPaid(to, amount)`

**Syndicate**
- `SyndicateOpened(softCap, hardCap, minTicket, deadline)`
- `AllocationSet(investor, amount)` / `InvestorFunded(investor, amount)`
- `CloseResolved(status)`
- `BatchSettled(total)` / `BatchRefunded(total)`

**Tranches (optional)**
- `TrancheScheduled(refId, scheduleHash)` / `TrancheReleased(refId, amount)`

> Final names/fields may evolve with audits and integration feedback.

---

## Security & compliance
- No custody of user funds; no PII on-chain or in this repo.  
- KYC/KYB, accreditation and AML checks are performed off-chain via providers before funding.  
- Please report vulnerabilities privately (see **SECURITY.md**).

---

## License
MIT © NexOTC.  
By contributing, you agree that your contributions are licensed under the MIT License.

---

## Contact
Questions or partnership inquiries: **info@nexotc.com**
