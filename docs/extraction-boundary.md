# Extraction boundary

This note tracks ownership of Sew economic and qualification artifacts across
the Sew package and PRF consumer boundary.

## Current PRF consumer status

PRF profiles now resolve the Sew adapters from immutable Git subdirectory
dependencies. The PRF working copy removed the local adapter directories, the
obsolete local `workspaces/sew` overlay, and the duplicate
`suites/sew-domain-reference-v1` outputs. The copied trace retained for one
generic forensic-graph test is explicitly a non-authoritative test fixture.

The in-tree `packages/sew-protocol-model` mirror remains in PRF because its
boundary overlaps Force Authorization work owned by another agent. The five
Sew scenario inputs and expected outputs are consumed from the Sew package's
classpath resources and canonical qualification suite. The PRF runner and its
claims registry remain local because they include Force Authorization claims.

PRF path checks and profile boundary/load checks passed after the adapter
switch. The full PRF test suite was not run for that source-removal change.

## Sew-owned candidate contents

- The Sew model package includes
  `resources/sew/policies/default-slash-distribution-v1.edn`. Its content
  matches the copy in PRF's current `packages/sew-protocol-model` tree.
- The Sew-specific yield adapter is under `extensions/sew-yield-integration`.
- The Sew financial lifecycle models and tests are under
  `extensions/sew-financial-lifecycle`.
- The Sew pro-rata economics adapter is under
  `extensions/sew-pro-rata-economics`.
- Sew-specific allocation application, held-custody projection, and slash-plan
  application adapters are under `extensions/sew-pro-rata-application`.
- The Sew bounty escrow-to-workflow route is under
  `extensions/sew-bounty-route`.
- The Sew application adapter for generic with-bounty plans is under
  `extensions/sew-bounty`.
- Optional Sew analysis projections and validators are under
  `extensions/sew-analysis`.
- Sew-specific economic and adversarial research models are under
  `extensions/sew-research-models`; generic Monte Carlo primitives remain in
  PRF.
- Sew research analyses are under `extensions/sew-research`; the shared PRF
  simulation runtime remains in PRF.
- The Sew pro-rata slashing and evidence adapter is under
  `extensions/sew-pro-rata-slashing`.
- Sew slash-allocation conformance vectors are under
  `extensions/sew-pro-rata-test-vectors`; generic liquidity-fulfillment
  vectors remain in PRF.

These packages are available at immutable revisions in the public Sew
repository. PRF profiles consume them directly; the remaining local mirrors
listed above are separate ownership work.

The model and adapters expose focused subdirectory packages. Their manifests
pin sibling Sew packages to an immutable Git revision, rather than depending on
local checkout paths. Consumers can select the packages they need without
loading every Sew adapter.

## Sew model extensions repository

[`Sew-Protocol/prf-sew-model-extensions`](https://github.com/Sew-Protocol/prf-sew-model-extensions)
contains the Solidity Sew Core behavior adapter, protocol model, and Sew-owned
extensions. Package manifests pin immutable revisions from this repository.
The original `prf-sew-core-model` repository remains the source history for the
initial extraction; new consumer dependencies use this repository.

## PRF component migration map

This maps PRF component declarations to their Sew package owners. “External”
means the PRF profile selects a pinned Sew subdirectory dependency.

| PRF component | Candidate disposition |
| --- | --- |
| `:prf/sew-escrow-kernel-v1`, `:prf/sew-protocol-model-v1` | Model package exists here and is pinned from PRF profiles; PRF keeps its model mirror pending Force Authorization ownership work. |
| `:prf/sew-yield-integration-v1` | External `extensions/sew-yield-integration/`; shared `:prf/yield-runtime-v1` remains in PRF. |
| `:prf/sew-core-extension-v1` | Existing adapter under `src/prf/extensions/sew_core_model/`; its source and history remain in this repository. |
| `:prf/sew-force-authorisation-v1` | Not included here; another agent owns this extraction. Keep it in PRF until that work is reviewed and switched. |
| `:prf/sew-analysis-v1`, `:prf/sew-research-models-v1`, `:prf/sew-research-v1` | External `extensions/sew-analysis/`, `extensions/sew-research-models/`, and `extensions/sew-research/`. Shared PRF simulation primitives remain in PRF. |
| `:prf/sew-financial-lifecycle-v1` | External `extensions/sew-financial-lifecycle/`. |
| `:prf/sew-pro-rata-economics-v1`, `:prf/sew-pro-rata-slashing-v1`, `:prf/sew-pro-rata-application-v1` | External Sew packages. Generic pro-rata allocation and verification remain in PRF. |
| `:prf/sew-pro-rata-test-vectors-v1` | Sew slash-allocation vectors are external; the generic vector wrapper and liquidity fixtures remain in PRF. |
| `:prf/sew-bounty-adapter-v1`, `:prf/sew-bounty-route-v1` | External `extensions/sew-bounty/` and `extensions/sew-bounty-route/`. Generic bounty composition remains in PRF. |
| Sew domain-reference validation material | Canonical suite, inputs, and expected outputs are in `qualification/sew-domain-reference-v1/`; PRF keeps only trace artifacts required by its Solidity equivalence manifest. |

Adapter profile dependencies and scenario resources are switched. The model
mirror, PRF runner, claims registry, and shared components require separate
ownership decisions and must not be removed as part of a bulk adapter cleanup.

## Keep in PRF

- Generic pro-rata allocation and its portable contract remain PRF concepts
  and implementations. Sew-specific settlement behavior belongs in this repo.
- `config/defaults.edn` in the current `agent-c-3` workspace has no asset
  decimals default. Its `:pro-rata-parallelism` value is execution tuning, not
  an allocation rule.
- Generic yield and liquid-lending suites remain PRF-owned. Examples and sample
  claims remain non-authoritative PRF examples unless a Sew application
  contract adopts them.
- PRF now separates the generic `pro-rata` profile from the optional
  `sew-pro-rata-application` profile. The latter still includes the in-tree
  Sew model and is not an independently consumable package.

## Sew domain-reference qualification suite

The package owns the canonical
`qualification/sew-domain-reference-v1` suite and the five scenario inputs
under `extensions/sew-reference-validation/resources/scenarios/edn`. Its
replay, invariant-checker, projection, and trace-export adapter delegates to
PRF's generic reference-validation harness at the pinned PRF core revision.

PRF scenario loading resolves those inputs from package classpath resources;
the duplicate suite bundle and scenario source copies have been removed from
PRF.

## Audit of the requested economic artifacts

This audit distinguishes Sew-owned definitions from reusable PRF mechanisms
and illustrative inputs. It records the remaining PRF ownership boundaries.

| Artifact group | Disposition |
| --- | --- |
| `protocols/sew/policies/default-slash-distribution-v1.edn` | Sew-owned policy is in `packages/sew-protocol-model/resources/sew/policies/`; PRF consumers now use the package resource. |
| `config/defaults.edn` asset decimals | The inspected staging snapshot has `:yield/default-asset-decimals 18`; the current `agent-c-3` source no longer has this key. In the staging code, the value feeds `token-decimals`, but the current quantization functions ignore their decimals argument, so this fallback does not presently change numeric results. Keep the shared fallback with PRF unless the yield API semantics change; protocol deployments should supply token metadata explicitly. |
| Generic `profiles/pro-rata*.edn` and `pro-rata/portable-contract/` | Remain PRF-owned. The Sew-specific realization is copied under `extensions/sew-pro-rata-application/`; the generic allocation contract and portable profile stay in PRF. |
| Sew yield and financial lifecycle profiles | The Sew yield integration and financial lifecycle code/tests are in `extensions/sew-yield-integration/` and `extensions/sew-financial-lifecycle/`. PRF profiles remain consumer compositions. |
| Scenario assumptions and expected results | The five Sew domain-reference scenarios and expected results are in `qualification/sew-domain-reference-v1/`. PRF retains the trace artifacts referenced by its Solidity equivalence manifest. The similarly named economic-assumptions file is under generic `suites/reference-validation-v1`; yield and liquid-lending suites remain PRF-owned. |
| Sew sample fixtures | `data/fixtures/protocol/sew.edn` is in `examples/sew-protocol-fixture.edn` as a non-authoritative sample. The aggregate held-credit contract and normative fixture are in `qualification/sew-aggregate-held-credit-v1/`. Their PRF copies have been removed. |
| Examples and sample claims | The non-authoritative Sew economic assumptions example is now in `examples/economic-assumptions.edn` and has been removed from PRF. Generic pro-rata requests and yield demos remain PRF examples. The `sew-claims.edn` registry remains bundled with PRF's `sew-runner`; its claims have no validation dates, cite scenario files outside the extracted Sew suite, and include force-authorisation claims owned by other work. Do not present it as qualified evidence or move it as part of this extraction. |
| PRF README economic language | The current README already describes pro-rata parameters as versioned reference-application inputs, not PRF-wide defaults. Keep that distinction when the repository documentation changes. |

## Removal gate

The published package revision and PRF adapter consumer gate are in place.
Remaining removal work is limited to the model mirror, runner-owned
integration, and any assets that still have PRF-local consumers.
