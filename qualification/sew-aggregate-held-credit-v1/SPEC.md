# SEW Aggregate Held-Credit Semantic Contract V1

This is the portable semantic contract for `:sew/aggregate-held-credit.v1`.
It contains no chain, contract, nonce, ABI, or FRAME concepts.

## Authorization Boundary

This contract validates allocation and held-credit effect semantics. It does
not authorize a caller, prove that an on-chain operation may execute, verify a
permit, or prove that an authorization was consumed exactly once. An adapter
descriptor root identifies declared input; it is not evidence that deployed
code enforces the descriptor's authority rules.

The reviewed Sew Core EVM profile is pinned to
[`SewCoreVault.sol` at source revision
`17bf2f0693367bfa3909a5aa186f4e806a76a5ea`](https://github.com/Sew-Protocol/sew-protocol/blob/17bf2f0693367bfa3909a5aa186f4e806a76a5ea/contracts/core/SewCoreVault.sol).
That profile has ordinary participant, resolver, keeper, and timelock gates;
it has no Force Authorisation operation. Its pinned profile tests cover
release, dispute resolution, timeout, pull settlement, and role-gated actions,
not Force Authorisation
([`SewCoreVaultProfile.t.sol`](https://github.com/Sew-Protocol/sew-protocol/blob/17bf2f0693367bfa3909a5aa186f4e806a76a5ea/test/foundry/core/SewCoreVaultProfile.t.sol)).
The PRF model must not claim Force Authorisation parity with this EVM profile.

Any future Sew EVM profile that supports Force Authorisation needs a separate,
source-pinned qualification that composes this semantic contract with the
exact on-chain authorization inputs and outcomes. Its preconditions must be
derived from that profile's Solidity implementation and executable tests.

## Authoritative Inputs

- Exact realized allocation, including `:allocation/hash` and canonical rows.
- Aggregate quantity identity (`canonical-quantity-identity.v1`).
- Aggregate target map (`allocation-quantity-target-map.v2`).
- Native location map (`canonical-quantity-native-location-map.v1`).
- SEW adapter descriptor and its descriptor root.
- Allocation scope, aggregate custody scope, and mapping profile.
- Native pre-state root.
- Allocation policy root and compiler semantics root.

The target map is authoritative application input. It is not derived from the
allocation. It must cover exactly the realized allocation row IDs and map them
to the one aggregate held-credit quantity.

## Derived Artifacts

1. Recompute and validate target-map roots and location-map roots.
2. Build `allocation-quantity-target-map-validation.v2`, committing the exact
   realized allocation root and all mapping/state dependencies.
3. Compile `sew/aggregate-held-credit.v1`: positive, uncapped, all-active rows
   produce one normalized aggregate delta.
4. Normalize effects under `canonical-effect-set.v1`.
5. Apply effects to the canonical pre-state to derive the canonical post-state.
6. Build `canonical-effect-transition.v1` from exact semantics, pre-state,
   effects, and post-state.
7. Build `effect-compilation-binding.v1` or `.v2` joining compilation identity
   to the exact transition identity.

## Canonicality

- Allocation rows preserve the claimant-context order established by the
  allocation artifact.
- Target-map targets are canonically sorted by allocation subject ID.
- Effect normalization composes deltas by quantity root, removes net-zero
  deltas, and orders by quantity root.
- State quantities are represented as a sorted map in the state root.
- Effect emission order is therefore irrelevant after normalization.
- Target-map subject coverage and target cardinality remain exact; duplicate
  subjects are rejected.

## Root Projections

- Target-map root: schema, allocation-subjects root, allocation scope root,
  aggregate custody scope root, mapping profile root, and sorted targets.
- Validation root: schema, target-map root, realized-allocation root, mapping
  profile, both scopes, adapter descriptor root, native pre-state root, native
  location-map root, and aggregate quantity root.
- Effect-compilation root: schema, realized-allocation root, allocation-policy
  root, target-map root, mapping profile, compiler semantics root, and effects
  root.
- Effects root: effect schema, effect-semantics root, and normalized effects.
- State roots: state schema and sorted quantity map.
- Transition root: transition schema, effect-semantics root, state-before root,
  effects root, and state-after root.
- Binding root: binding schema, effect-compilation root, and canonical
  transition root.

## Fail-Closed Invariants

- Claimant IDs, requested IDs, and filled IDs must be identical sets.
- Deferred and haircut values are forbidden by this all-active profile.
- Target subjects must equal allocation row IDs exactly.
- The target map must contain one aggregate-held-credit target quantity.
- Aggregate quantity identity, scopes, adapter descriptor, and native locations
  must agree with the validation inputs.
- Effects must be reconstructed from allocation, target map, and compiler
  semantics; a locally self-consistent effects root is insufficient.
- State-after must equal applying exact normalized effects to exact state-before.
- Compilation and transition must be bound by exact root equality.
- The realized-allocation root is retained throughout validation and compilation;
  equivalent-looking rows with another identity are not interchangeable.
