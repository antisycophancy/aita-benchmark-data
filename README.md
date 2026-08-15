# AITA Reversed N20 v1

This is the separately versioned data add-on for the Anti-Sycophancy Benchmark
Suite's AITA condition. It contains 20 source perspectives, 20 fixed
project-created reversals, labels, a locked selection, a data card, and
provenance metadata.

The benchmark software is published separately. This repository contains only
the public sealed pack and its release metadata. It does not contain readable
source prompts, the suite release's Part B fragment, model transcripts, API
keys, or development history.

## Why the pack is sealed

The payload uses AES-256-GCM so ordinary browsing and search indexing do not
expose the benchmark prompts. This is gentle anti-indexing friction. Both key
fragments are public. Part A is in the envelope and Part B is a separate asset
attached to the signed Anti-Sycophancy Benchmark Suite release. The unlock is
associated with the software version but is not tracked in either repository's
Git history.

The seal is not confidentiality, DRM, access control, rights clearance, or a
promise that public copies can be recalled. Read [`RIGHTS.md`](RIGHTS.md)
before downloading or redistributing the pack.

## Release files

| File | Purpose |
| --- | --- |
| `aita-reversed-n20-v1.envelope.json` | Public Part A, pack identity, ciphertext digest, and authenticated metadata. |
| `aita-reversed-n20-v1.sealed` | Authenticated ciphertext containing the benchmark data. |
| `PACK_REGISTRY.json` | Exact file sizes, hashes, pack identity, and correction contact expected by the software release. |
| `SHA256SUMS` | Integrity inventory for this release. |
| `RIGHTS.md` | Rights, privacy, correction, and removal boundary. |

Publisher authenticity comes from the signed release tag or detached release
signature. `SHA256SUMS` checks integrity and inventory after that source has
been authenticated.

## Use with the benchmark suite

Start from an authenticated Anti-Sycophancy Benchmark Suite release. Its agent
skill or CLI first inspects the frozen pack registry without networking:

```bash
./venv/bin/python -m suite_tools.aita_data_pack status --json
```

The agent shows the exact repository, release assets, destination, byte counts,
and hashes, then asks before downloading. It does not download the pack in the
background. After approval, it uses the consent-gated fetch command and emits a
prompt-free verification receipt.

```bash
./venv/bin/python -m suite_tools.aita_data_pack fetch \
  --destination private_question_bank/aita-reversed-n20-v1 \
  --confirm-download \
  --json
```

Preparation receives the downloaded `.envelope.json` path through
`--sealed-pack`. Preparation and generation each display
`AITA sealed-pack key Part B:` and wait. The prompt is visible. Only the
characters typed are hidden. The runner authenticates and opens the pack in
memory, verifies the locked identities, and does not create a redundant
plaintext dataset cache.

Local benchmark transcripts necessarily contain tested prompts. Keep them
private. The suite's public bundle gate refuses raw transcripts from sealed
AITA runs.

## Verify a manual download

After authenticating the signed release, verify every released file:

```bash
shasum -a 256 -c SHA256SUMS
```

Use `sha256sum -c SHA256SUMS` on Linux. The benchmark suite can also verify the
envelope and ciphertext against its frozen registry without opening plaintext:

```bash
./venv/bin/python -m suite_tools.aita_data_pack verify \
  --registry /path/to/benchmark-suite/manifests/aita-sealed-pack-v1.json \
  --destination /path/to/this-release \
  --json
```

## Corrections and removal

Send a pack item ID or source post ID to `research@antisycophancy.ai`. Do not
post personal information in a public issue. A correction creates a successor
version or tombstone. Immutable released bytes are not silently rewritten, and
previously downloaded public copies cannot be recalled.
