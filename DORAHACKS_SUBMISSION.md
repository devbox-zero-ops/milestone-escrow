# MilestoneEscrow — Trust-Minimized Multi-Milestone `ugnot` Escrow & Arbiter Split Realm

## One-Line Tagline
Trust-minimized multi-milestone `ugnot` escrow & basis-point arbiter dispute-split realm deployed on Gno.land `onyx-1`, with CEI payout safety and native `gnoweb` rendering.

## Links
- **Live Interactive Demo & Onyx RPC Console**: https://devbox-zero-ops.github.io/milestone-escrow/
- **GitHub Repository**: https://github.com/devbox-zero-ops/milestone-escrow
- **On-Chain Realm Path (`onyx-1`)**: `gno.land/r/g1kkcw4krnhxadt08gjxs5ud2jhv87rtlzt29thy/milestone_escrow`
- **Onyx gnoweb URL**: https://onyx.testnets.gno.land/r/g1kkcw4krnhxadt08gjxs5ud2jhv87rtlzt29thy/milestone_escrow
- **Deploy Block / TX**: Block `#1355214` · TX `p2wbOV2Y1nh4YaCOVcCywDBHqK+Kl8eMhFO3HpsOn8M=`
- **GPAO Approval Queue**: https://gpas.onyx.testnets.gno.land/
- **Demo Video**: (YouTube URL / GitHub Release `v1.0.0` `https://github.com/devbox-zero-ops/milestone-escrow/releases/download/v1.0.0/milestone-escrow-demo.webm`)

## Problem & Solution
DAO grants, open-source bounties, and freelance engineering contracts routinely fail between all-or-nothing upfront trust and slow off-chain arbitration. **MilestoneEscrow** is a pure Gno (`v1.8.0`) on-chain realm that locks native `ugnot` deposits into discrete milestones, releases each milestone strictly under Checks-Effects-Interactions (CEI) accounting, supports optional basis-point (`0..10000 bps`) arbiter dispute splits with zero dust loss, enforces a global conservation invariant (`locked + released + refunded == total_deposited`), and exposes a 4-route human-readable `gnoweb` UI (`""`, `contract/<id>`, `stats`, `guide`).

## Key Features & Verification
- **Strict CEI & Origin-Safe Accounting**: Uses `chain/banker` (`OriginBanker`) + `chain/runtime/unsafe.OriginSend()` and mutates milestone/contract state before every `SendCoins` transfer.
- **Zero-Dust Basis-Point Dispute Splits**: `ResolveDispute(contractID, milestoneIdx, contractorBps)` computes `contractorPayout = amount * bps / 10000` and `clientRefund = amount - contractorPayout` so every single `ugnot` is accounted for.
- **Native `gnoweb` 4-Route Markdown UI**: Live protocol TVL table, per-contract milestone breakdown, accounting & conservation invariant stats, and copy-paste `gnokey` quick-start guide.
- **Verified Unit Tests (`5/5 PASS`)**: Verified locally with native `gno test -v .` covering happy-path approval, arbiter 6500-bps split, full client cancellation refund, role/re-entrancy guards, and global conservation invariant.
- **Deployed On-Chain to `onyx-1`**: Broadcast & committed at Block `#1355214` (currently queued in Onyx's automated GPAO review pipeline alongside new `onyx-1` realms), paired with a live interactive Gno VM State Machine Sandbox & Onyx RPC Console on GitHub Pages.
