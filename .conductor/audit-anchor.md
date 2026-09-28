# Atrytone audit-chain anchor

Atrytone keeps an append-only, hash-chained audit log of every governed
action it takes. This file publishes that chain's head into this
repository — somewhere Atrytone does not control — so that the record can
be checked by someone who does not have to take Atrytone's word for it.

It is written by Atrytone's broker, not by the agent that produced the
change in this push. The agent cannot omit it, and cannot write it: the
path is reserved, and a push whose payload contains it is refused.

## The anchor

```
chain head sequence      48920
chain head hash          459e9c300c376deb03b36987ab17ad2efe3c60c9dca0e37616e9df3a31f25f7a
head row written at      2026-09-28T08:48:30.220Z
chain genesis at         2026-06-26T10:52:06.958Z
published at             2026-09-28T08:48:36.065Z
published into           Rastamasta1/eslint
carried by intent        288f3e47-9845-4c83-8b37-c8b3518e209c
cockpit build            a5629b43
```

## The previous anchor, so a gap is visible

**This is the first anchor published into this repository.** Nothing
before it is anchored here. Later anchors will name this one, so any
gap after this point is visible in this file's git history.

## What this proves

- Every audit row up to sequence 48920 hashes, in order, to the head
  hash above. Each row's hash covers the previous row's hash, so the
  sequence cannot be reordered, and no row can be removed from the middle
  without the following hashes disagreeing.
- This file is committed to this repository, so the hash above existed at
  this commit's date — a date recorded in this repository's history, which
  Atrytone does not administer and cannot rewrite.
- Therefore any later edit to any audit row at or below sequence 48920
  makes Atrytone's recomputed head disagree with the hash committed here,
  and the disagreement is detectable by anyone holding this file.

## What this does NOT prove

- It says nothing about rows written BEFORE the genesis above. 2442
  rows lost their run attribution before the chain existed, and that is
  unrepairable: the identifiers are gone and nothing recorded what they were.
- It does not prove COMPLETENESS. A chain shows that nothing recorded was
  changed. It cannot show that everything that happened was recorded.
- It does not reveal or prove the CONTENT of any row. The hash commits to
  the whole ledger; reading any part of it still requires Atrytone.
- The sequence number counts audit rows across EVERY Atrytone client, not
  only this one. It therefore discloses how many audit rows exist in total,
  and nothing about whose they are.
- It anchors MOMENTS, NOT TIME. A head is published when Atrytone's broker
  pushes to this repository, and at no other time. Between two anchors
  Atrytone was running and was not anchored here. Since 2026-08-19
  Atrytone's worker has no way to push here except through the broker. A
  commit that is not a brokered push (a merge, a person's own commit)
  carries no anchor of its own. Do not read the presence of this file on
  some commits as a guarantee that every commit has one.

## How to check it

1. Note the head hash and sequence above, and this commit's date.
2. Ask Atrytone to recompute the chain over the same range. Its
   `verify_audit_chain()` walks every row in sequence order, recomputing
   each row's hash from the row's own contents and the previous hash.
3. The head it produces for sequence 48920 must equal the hash above.
   If it does not, something at or below that sequence changed after this
   commit was made.
