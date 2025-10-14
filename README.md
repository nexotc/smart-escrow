# NexOTC Smart Escrow Protocol

**NexOTC** is the non-custodial settlement layer for private markets.

> **Status:** This repository currently hosts the public specification, structure and governance documents for the *Smart Escrow Protocol* (MVP). Source code will be added as development milestones are reached.

---

## Scope (MVP)

- **EscrowContract** – delivery-versus-payment (DvP), deadlines/expiry, safe refund.
- **FeeRouter** – multi-recipient routing for protocol fees and broker commissions (basis points with caps).
- **SyndicateController + AllocationBook** – investor allowlist, min ticket, soft/hard cap, per-investor (“sharded”) escrows, batch settle/refund.
- **TrancheManager** *(optional in MVP)* – simple TGE + linear distributions.
- **Governance (Roles + Timelock)** – parameter changes with delay and audit trail.
- **Event Catalog** – canonical on-chain events for indexing and receipts.

### Design principles
- **Non-custodial by design** — funds never sit in a pooled wallet.
- **Modular & upgrade-aware** — safe upgrade path with explicit governance.
- **Privacy by design** — the protocol does not store personal data; KYC/AML checks are off-chain via providers.
- **Deterministic receipts** — every state transition emits a canonical event.

---

## Event catalog (draft)

- `EscrowCreated(buyer, seller, asset, amount, deadline)`
- `FundsDeposited(party, amount)`
- `Released(to, amount)` / `Refunded(to, amount)` / `Cancelled(by)`
- `SyndicateOpened(softCap, hardCap, minTicket, fundingDeadline)`
- `AllocationApproved(investor, amount)` / `AllocationFunded(investor, amount)`
- `BatchSettled(total)` / `BatchRefunded(total)`

Final names/fields may evolve with audits and integration feedback.

---

## Security & compliance

- No custody of user funds; no storage of PII on-chain or in this repository.
- Off-chain KYC/KYB and wallet risk checks are performed by providers before funding.
- Please report vulnerabilities privately (see **SECURITY.md**).

---

## License

MIT © NexOTC.  
By contributing, you agree that your contributions are licensed under the MIT License.

---

## Contact

Questions or partnership inquiries: **contact@nexotc.com**
