# Fork notes

This repository is a fork of
[microsoft/onnxruntime-swift-package-manager](https://github.com/microsoft/onnxruntime-swift-package-manager)
carrying a small patch series.

## Branches

- `main` **mirrors upstream exactly.** Never commit to it. Sync it with GitHub's "Sync fork" button or
  `git fetch upstream && git push origin upstream/main:main`.
- `patches` **carries the patch series**, rebased onto `main` after every sync:
  `git rebase main patches && git push --force-with-lease origin patches`. Each commit is one
  self-contained change; a patch upstream absorbs is dropped from the series.

## Tags

Releases are tagged on `patches` as `<upstream ORT version>-fork.<n>`, for example `1.24.2-fork.1`:
ONNX Runtime 1.24.2 plus revision 1 of the patch series. The suffix keeps fork tags from ever being
mistaken for upstream's. Consumers pin with `exact:`, since every fork release is a deliberate rebase.

Tags `1.20.1` and `1.20.2` predate this convention (despite their names they ship ORT 1.23.0) and are
kept because older revisions of consuming packages resolve to them.

## Binary mirror

Upstream's `Package.swift` downloads the ONNX Runtime pod archives from `download.onnxruntime.ai`,
which Xcode Cloud cannot resolve. The `patches` branch points at byte-for-byte copies attached to a
GitHub release of this repository named `binary-cache-<date>` (for example `binary-cache-2026.09.09`),
with the checksums unchanged. When upstream bumps ONNX Runtime: download the new archives, verify
their sha256 against upstream's `Package.swift`, attach them to a new `binary-cache-<date>` release,
then rebase `patches` and update the two URLs.

## Current patch series

1. Add the Bool tensor element type to the Objective-C/Swift bindings (`ORTTensorElementDataTypeBool`);
   GLiNER models take a boolean `span_mask` input.
2. Point the binary targets at the GitHub release mirror instead of `download.onnxruntime.ai`.

Patch 1 is a candidate for an upstream pull request.
