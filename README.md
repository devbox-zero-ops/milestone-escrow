# MilestoneEscrow

**On-Chain Multi-Milestone `ugnot` Escrow & Split-Payout Realm on Gno.land (`onyx-1`)**

Built for **Gnome Tavern Hackathon** — a zero-dependency, stateful Gno realm that lets clients, open-source contributors, and autonomous agents lock native `ugnot` into structured milestone schedules with verifiable deliverable proofs, Checks-Effects-Interactions (CEI) releases, basis-point arbiter dispute splits, and native `gnoweb` Markdown dashboards.

## Live Deployment (`onyx-1` Testnet)

- **Live `gnoweb` Realm URL**: `https://onyx.testnets.gno.land/r/g1kkcw4krnhxadt08gjxs5ud2jhv87rtlzt29thy/milestone_escrow`
- **Live RPC Companion Explorer**: `https://devbox-zero-ops.github.io/milestone-escrow/`
- **Realm Package Path**: `gno.land/r/g1kkcw4krnhxadt08gjxs5ud2jhv87rtlzt29thy/milestone_escrow`
- **Network / Chain ID**: `onyx-1` (`https://rpc.onyx.testnets.gno.land:443`)

## Why MilestoneEscrow?

Freelance work, open-source bounties, and agent-to-agent tasks rarely settle cleanly in one lump sum:
- Paying 100% upfront exposes clients to non-delivery.
- Paying 100% at the end exposes contributors to non-payment.
- Binary win/lose disputes fail when partial work is genuinely delivered.

**MilestoneEscrow** solves this directly in Gno with zero external package dependencies:

| Capability | Function | Security / Accounting Guarantee |
|---|---|---|
| Open multi-milestone escrow | `CreateContract(cur, title, contractor, arbiter, titlesCsv, amountsCsv)` | Direct-user origin guard (`cur.Previous().IsUser()`) + exact `ugnot` deposit check (`deposited == sum(milestones)`) |
| Submit deliverable proof | `SubmitMilestone(cur, contractID, milestoneIdx, proofURI)` | Restricted to `c.Contractor`, only from `PENDING` state, freezes unilateral client cancel |
| Approve & release payout | `ApproveMilestone(cur, contractID, milestoneIdx)` | Restricted to `c.Client`, strict CEI state update before `banker.SendCoins`, auto-completes contract |
| Escalate dispute | `OpenDispute(cur, contractID, milestoneIdx, reason)` | Either party can freeze an unreleased milestone when a designated arbiter exists |
| Basis-point split settlement | `ResolveDispute(cur, contractID, milestoneIdx, contractorBps)` | Arbiter-only (`0..10000` bps), exact conservation (`clientRefund = amount - contractorShare`, zero rounding dust) |
| Cancel unstarted work | `CancelUnstarted(cur, contractID)` | Client recovers remaining `PENDING` funds only if no milestone is `SUBMITTED` or `DISPUTED` |
| Native `gnoweb` dashboard | `Render(path)` | Live routes `""`, `contract/<id>`, `stats` (conservation invariant audit), and `guide` |

## Local Verification (`gno test`)

```bash
cd realm
gno test -v .
```

All 5 test suites pass on Gno `v1.8.0`:
- `TestHappyPathMultiMilestoneEscrow`
- `TestDisputeAndArbiterBasisPointSplit`
- `TestCancelUnstartedMilestonesAndGuards`
- `TestDepositMismatchAndRoleGuards`
- `TestRenderRoutesAndConservationInvariant`

## Architecture

```text
realm/
  gnomod.toml               # Module gno.land/r/g1kkcw4krnhxadt08gjxs5ud2jhv87rtlzt29thy/milestone_escrow
  milestone_escrow.gno      # State machine, CEI ugnot settlement, role guards, events
  render.gno                # Multi-route gnoweb Markdown dashboard & accounting audit
  milestone_escrow_test.gno # Full Gno VM test suite (5/5 PASS)
docs/
  index.html                # Zero-backend Onyx RPC live ABCI vm/qrender explorer + gnokey builder
```
