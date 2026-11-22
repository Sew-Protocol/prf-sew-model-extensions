# Extraction boundary

This note tracks ownership of Sew economic and qualification artifacts across
the Sew package and PRF consumer boundary.

## Current PRF consumer status

PRF profiles now resolve the Sew model and economic adapters from immutable
Git subdirectory dependencies. The PRF working copy removed the production
source/resource copies for the model, slash economics, pro-rata application,
slashing evidence, yield integration, and financial lifecycle. Their local PRF
directories are integration-test harnesses and dependency manifests. The Sew
profile bundle supplies the financial-lifecycle, yield-integration, and
pro-rata-application profiles. The local `workspaces/sew` overlay remains as
development/test scaffolding. The five Sew scenario inputs and expected
outputs are consumed from package resources and the canonical qualification
suite. The PRF runner and its claims registry remain local because they include
Force Authorization claims.

PRF path checks, package/profile boundary checks, the focused package suite
(735 tests / 2,460 assertions), and the extraction integration suites passed.
The package is still an unpublished candidate, so these results do not qualify
a published artifact.

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
| `:prf/sew-escrow-kernel-v1`, `:prf/sew-protocol-model-v1` | Model package is owned here and pinned from PRF profiles; PRF's production source copy has been removed. Force Authorization remains a separate PRF-owned integration. |
| `:prf/sew-yield-integration-v1` | External `extensions/sew-yield-integration/`; shared `:prf/yield-runtime-v1` remains in PRF. |
| `:prf/sew-core-extension-v1` | Existing adapter under `src/prf/extensions/sew_core_model/`; its source and history remain in this repository. |
| `:prf/sew-force-authorisation-v1` | Not included here; another agent owns this extraction. Keep it in PRF until that work is reviewed and switched. |
| `:prf/sew-analysis-v1`, `:prf/sew-research-models-v1`, `:prf/sew-research-v1` | External `extensions/sew-analysis/`, `extensions/sew-research-models/`, and `extensions/sew-research/`. Shared PRF simulation primitives remain in PRF. |
| `:prf/sew-financial-lifecycle-v1` | External `extensions/sew-financial-lifecycle/`. |
| `:prf/sew-pro-rata-economics-v1`, `:prf/sew-pro-rata-slashing-v1`, `:prf/sew-pro-rata-application-v1` | External Sew packages. Generic pro-rata allocation and verification remain in PRF. |
| `:prf/sew-pro-rata-test-vectors-v1` | Sew slash-allocation vectors are external; the generic vector wrapper and liquidity fixtures remain in PRF. |
| `:prf/sew-bounty-adapter-v1`, `:prf/sew-bounty-route-v1` | External `extensions/sew-bounty/` and `extensions/sew-bounty-route/`. Generic bounty composition remains in PRF. |
| Sew domain-reference validation material | Canonical suite, inputs, and expected outputs are in `qualification/sew-domain-reference-v1/`; PRF keeps only trace artifacts required by its Solidity equivalence manifest. |

Adapter profile dependencies and scenario resources are switched. The PRF
runner, claims registry, shared framework components, and Force Authorization
integration retain separate ownership and must not be removed as part of a
bulk adapter cleanup.

## Keep in PRF

- Generic pro-rata allocation and its portable contract remain PRF concepts
  and implementations. Sew-specific settlement behavior belongs in this repo.
- `config/defaults.edn` in the current `agent-c-3` workspace sets the generic
  yield fallback `:default-asset-decimals` to 18. `token-decimals` exposes this
  fallback as metadata, while the current quantization functions ignore the
  decimals argument and always floor to integer base units. It does not change
  current numeric results and is a generic yield-runtime default, not Sew
  settlement policy. Its `:pro-rata-parallelism` value is execution tuning.
- Generic yield and liquid-lending suites remain PRF-owned. Examples and sample
  claims remain non-authoritative PRF examples unless a Sew application
  contract adopts them.
- PRF now separates the generic `pro-rata` profile from the optional
  `sew-pro-rata-application` profile. The Sew financial-lifecycle,
  yield-integration, and pro-rata application descriptors are supplied by this
  repository's profile bundle; PRF still supplies the framework profile viewer
  and component declarations needed to compose them.

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
| `config/defaults.edn` asset decimals | The current PRF source contains `:yield/default-asset-decimals 18`. It supplies generic token-decimal metadata, but the quantization functions currently ignore that argument and floor to integer base units. It does not presently change numeric results. Keep this generic yield-runtime fallback in PRF; Sew deployments should provide token metadata explicitly when they need a non-default value. |
| Generic `profiles/pro-rata*.edn` and `pro-rata/portable-contract/` | Remain PRF-owned. The Sew-specific realization is copied under `extensions/sew-pro-rata-application/`; the generic allocation contract and portable profile stay in PRF. |
| Sew yield and financial lifecycle profiles | The implementations are in `extensions/sew-yield-integration/` and `extensions/sew-financial-lifecycle/`. Their profile descriptors are in the Sew package profile bundle and PRF discovers them from the pinned dependency classpath. |
| Scenario assumptions and expected results | The five Sew domain-reference scenarios and expected results are in `qualification/sew-domain-reference-v1/`. PRF retains the trace artifacts referenced by its Solidity equivalence manifest. The similarly named economic-assumptions file is under generic `suites/reference-validation-v1`; yield and liquid-lending suites remain PRF-owned. |
| Sew sample fixtures | `data/fixtures/protocol/sew.edn` is in `examples/sew-protocol-fixture.edn` as a non-authoritative sample. The aggregate held-credit contract and normative fixture are in `qualification/sew-aggregate-held-credit-v1/`. Their PRF copies have been removed. |
| Examples and sample claims | The non-authoritative Sew economic assumptions example is now in `examples/economic-assumptions.edn` and has been removed from PRF. Generic pro-rata requests and yield demos remain PRF examples. The `sew-claims.edn` registry remains bundled with PRF's `sew-runner`; its claims have no validation dates, cite scenario files outside the extracted Sew suite, and include force-authorisation claims owned by other work. Do not present it as qualified evidence or move it as part of this extraction. |
| PRF README economic language | The current README already describes pro-rata parameters as versioned reference-application inputs, not PRF-wide defaults. Keep that distinction when the repository documentation changes. |

## Removal gate

The published package revision and PRF adapter consumer gate are in place.
Remaining removal work is limited to the model mirror, runner-owned
integration, and any assets that still have PRF-local consumers.
