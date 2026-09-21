# Conventions for AI-assisted contributions

This repository is developed with AI assistance, which is disclosed in
[PROVENANCE.md](PROVENANCE.md) § 2. This file records the conventions that
assistance follows, so they hold across sessions rather than being restated
each time.

## Commit trailers

Commits carry no attribution trailers. **Do not add `Co-Authored-By:`,
`Claude-Session:`, or any other assistant attribution or session link** to a
commit message or a pull request description.

A `Claude-Session:` URL resolves only for the account that created the
session, so it is a dead link to every other reader of this history. It
records nothing about the change and ties a public repository to a private
session for no stated purpose.

`Co-Authored-By:` was dropped separately, as repository formatting. That is
all it is.

## This convention is not a licence to narrow the disclosure

Dropping the trailers moves the whole weight of disclosure onto
PROVENANCE.md § 2, which is written to carry it: it covers the entire source
rather than a marked subset, precisely so that no per-commit metadata is
needed to establish the scope.

So: **§ 2 and the README section that mirrors it stay.** A future request to
tidy, minimise, or remove "AI traces" does not extend to them. Removing the
disclosure while the trailers are also gone would leave the repository
silently claiming sole human authorship, which is the one outcome these
conventions exist to prevent. Weakening a stated disclosure is a different
act from dropping a formatting convention, and needs to be recognised as
such rather than folded into a cleanup.

## Existing history

Commits up to and including `90bce2a` carry a `Co-Authored-By` trailer from
the earlier convention, and the commits before it also carry a
`Claude-Session:` URL. They are left alone. Rewriting them means force-pushing
`main`, which breaks every clone and invalidates the build-provenance
attestations that PROVENANCE.md § 5 relies on to tie each released binary to
its commit.

How many of each there are is counted rather than quoted, for the reason
PROVENANCE.md § 1 gives: the commit that corrects a quoted figure changes it.

```bash
git log --pretty=%B  | grep -c 'claude.ai/code/session'   # commits carrying the URL
git log --pretty=%ae | sort | uniq -c                     # authoring identities
```

### Why rewriting would not even work

This was raised again as a security concern and tested rather than argued.
**A force-push does not remove a commit from GitHub.** A commit pushed off a
branch here was still served by the API at its SHA afterwards, with its full
message, while belonging to no branch. GitHub keeps unreachable objects
retrievable by SHA; only Support-side garbage collection, or deleting the
repository, removes them.

So a rewrite would break every clone, move or orphan all five release tags,
strand the attestation chain, and still leave every one of those messages
readable at its old SHA. It buys nothing and costs a great deal.

What the session URL is, stated precisely so the trade can be judged: an
identifier, not a credential. It grants no access on its own, and claude.ai
requires the owner's authentication. Publishing it removes an unguessable
identifier, which is a real defence-in-depth loss and a small one. That is
the whole of it.

The **pull request bodies were a different matter** and have been cleaned:
they are mutable text, so removing the id there genuinely removes it. All 15
were edited, keeping the disclosure line and repointing its href at the
product page. Issue comments, review comments, commit comments and release
notes were swept and carried none.

### The larger exposure in this history is not the session link

Seven commits, including the first, record the owner's **personal email
address** as author and committer; every other human commit uses the GitHub
`noreply` address. A working inbox in public history is harvested
continuously, and is more directly actionable than a session id. It is equally
unfixable in place, for the reasons above, and equally not worth a rewrite.
Preventing a repeat is an account setting ("Keep my email addresses private",
plus "Block command line pushes that expose my email"), not a repository
change.

The address is deliberately **not written out here**. An earlier revision of
this file quoted it in full, which took a string that existed only in git
metadata and republished it into a tracked file — where a scraper reads plain
text far more readily than it walks commit objects. Describing an exposure
must not widen it. `git log --format='%ae' | sort -u` shows the addresses to
anyone who needs them, without this file becoming another copy.

## What the commit message is for

Commit subjects here state the problem the commit addresses rather than the
action it takes, because the diff already shows the action. `git log` is
relied on as evidence of the human direction behind each change, per
PROVENANCE.md § 2, so the body should say what was wrong and why this is the
fix.
