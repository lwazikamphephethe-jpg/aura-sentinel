# aura-sentinel
Aura Sentinel prototype repository
# AURA Sentinel

AI and cybersecurity specialist for AURA Group. **Stage 1** (this version) is the foundation: a permission policy and an approval workflow, enforced in code, so no agent can act in the outside world without the owner saying yes.

- **Owner:** the founder approves every external commitment. Agents only recommend.
- **Budget:** R0. No paid services, no dependencies, nothing to install.
- **Language:** JavaScript (ES modules), Node 20 or newer.
- **Safe by default:** stage 1 only does dry runs. Nothing is emailed, spent, signed or published.

## Try it in 30 seconds (Codespaces or any terminal)

```
npm test          # 45 automated checks
npm run demo      # safe walk-through of the whole approval system
```

## Set up your owner phrase

```
npm run setup
```

Choose a phrase (12+ characters). It prints two lines. Put them in a file called `.env` (already gitignored) or save them as Codespaces secrets. Your phrase itself is never stored.

## Use it

```
# An agent asks for something (here, ATLAS wants to send an email)
npm run sentinel -- request --agent ATLAS --action send_email \
   --payload '{"to":"x@example.co.za"}' --reason "First outreach"

npm run sentinel -- list           # what is waiting for you
npm run sentinel -- approve <id>   # asks for your phrase
npm run sentinel -- execute <id>   # runs it (dry run in stage 1)
npm run sentinel -- verify-audit   # has the audit log been edited?
```

## The three tiers

| Tier | Meaning | What happens |
|------|---------|--------------|
| GREEN | Internal, reversible | Runs automatically, always logged |
| AMBER | Reaches outside or changes records | Waits for your approval |
| RED | Money, legal, access, destructive | Your approval + 60 second cooling-off + confirmation code |

Anything not on the list, or outside an agent's job, is **denied** and cannot be approved. Details: `docs/PERMISSION_POLICY.md`.

## What is in this folder

```
src/policy/       tiers, action list, agent scopes, classify() (the rules)
src/approvals/    gate (request/approve/execute), owner check, approval store
src/audit/        tamper-evident audit log
src/cli.js        command line        src/demo.js   safe demo
test/             automated tests     support/      test helper
docs/             policy, workflow, incident policy, roadmap, decision log
AURA_SENTINEL_INSTRUCTIONS.md   rules for any AI assistant working in this repo
```

## Honest limits of stage 1

- It is a command line tool. The phone dashboard and API come in later stages (`docs/ROADMAP.md`).
- The agent name on a request is trusted for now. Stage 2 ties each agent to its own access token.
- Budget cap is per request. A running total across requests comes in stage 2.
- The audit log detects edits in the middle, but cannot detect lines cut off the end. Stage 2 anchors the latest hash outside this machine.
- Anyone with full access to your Codespace and your `.env` can bypass any software check. Keep both private.